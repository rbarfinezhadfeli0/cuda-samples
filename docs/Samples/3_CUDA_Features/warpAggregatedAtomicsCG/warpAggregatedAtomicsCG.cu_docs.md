# Documentation for Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/warpAggregatedAtomicsCG
- **Binary**: No

## Purpose and Role

This is a CUDA source file containing GPU kernel implementations and host code.

## Original Source Content

```cu
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

#include <stdio.h>
// includes, project
#include <cooperative_groups.h>
#include <cooperative_groups/reduce.h>
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_functions.h>

namespace cg = cooperative_groups;

#define NUM_ELEMS             10000000
#define NUM_THREADS_PER_BLOCK 512

// warp-aggregated atomic increment
__device__ int atomicAggInc(int *counter)
{
    cg::coalesced_group active = cg::coalesced_threads();

    // leader does the update
    int res = 0;
    if (active.thread_rank() == 0) {
        res = atomicAdd(counter, active.size());
    }

    // broadcast result
    res = active.shfl(res, 0);

    // each thread computes its own value
    return res + active.thread_rank();
}

__global__ void filter_arr(int *dst, int *nres, const int *src, int n)
{
    int id = threadIdx.x + blockIdx.x * blockDim.x;

    for (int i = id; i < n; i += gridDim.x * blockDim.x) {
        if (src[i] > 0)
            dst[atomicAggInc(nres)] = src[i];
    }
}

// warp-aggregated atomic multi bucket increment
#if __CUDA_ARCH__ >= 700
__device__ int atomicAggIncMulti(const int bucket, int *counter)
{
    cg::coalesced_group active = cg::coalesced_threads();
    // group all threads with same bucket value.
    auto labeledGroup = cg::labeled_partition(active, bucket);

    int res = 0;
    if (labeledGroup.thread_rank() == 0) {
        res = atomicAdd(&counter[bucket], labeledGroup.size());
    }

    // broadcast result
    res = labeledGroup.shfl(res, 0);

    // each thread computes its own value
    return res + labeledGroup.thread_rank();
}
#endif

// Places individual value indices into its corresponding buckets.
__global__ void
mapToBuckets(const int *srcArr, int *indicesBuckets, int *bucketCounters, const int srcSize, const int numOfBuckets)
{
#if __CUDA_ARCH__ >= 700
    cg::grid_group grid = cg::this_grid();

    for (int i = grid.thread_rank(); i < srcSize; i += grid.size()) {
        const int bucket = srcArr[i];
        if (bucket < numOfBuckets) {
            indicesBuckets[atomicAggIncMulti(bucket, bucketCounters)] = i;
        }
    }
#endif
}

int mapIndicesToBuckets(int *h_srcArr, int *d_srcArr, int numOfBuckets)
{
    int *d_indicesBuckets, *d_bucketCounters;
    int *cpuBucketCounters = new int[numOfBuckets];
    int *h_bucketCounters  = new int[numOfBuckets];

    memset(cpuBucketCounters, 0, sizeof(int) * numOfBuckets);
    // Initialize each bucket counters.
    for (int i = 0; i < numOfBuckets; i++) {
        h_bucketCounters[i] = i * NUM_ELEMS;
    }

    checkCudaErrors(cudaMalloc(&d_indicesBuckets, sizeof(int) * NUM_ELEMS * numOfBuckets));
    checkCudaErrors(cudaMalloc(&d_bucketCounters, sizeof(int) * numOfBuckets));

    checkCudaErrors(cudaMemcpy(d_bucketCounters, h_bucketCounters, sizeof(int) * numOfBuckets, cudaMemcpyHostToDevice));

    dim3 dimBlock(NUM_THREADS_PER_BLOCK, 1, 1);
    dim3 dimGrid((NUM_ELEMS / NUM_THREADS_PER_BLOCK), 1, 1);

    mapToBuckets<<<dimGrid, dimBlock>>>(d_srcArr, d_indicesBuckets, d_bucketCounters, NUM_ELEMS, numOfBuckets);

    checkCudaErrors(cudaMemcpy(h_bucketCounters, d_bucketCounters, sizeof(int) * numOfBuckets, cudaMemcpyDeviceToHost));

    for (int i = 0; i < NUM_ELEMS; i++) {
        cpuBucketCounters[h_srcArr[i]]++;
    }

    bool allMatch   = true;
    int  finalElems = 0;
    for (int i = 0; i < numOfBuckets; i++) {
        finalElems += (h_bucketCounters[i] - i * NUM_ELEMS);
        if (cpuBucketCounters[i] != (h_bucketCounters[i] - i * NUM_ELEMS)) {
            allMatch = false;
            break;
        }
    }

    if (!allMatch && finalElems != NUM_ELEMS) {
        return EXIT_FAILURE;
    }
    return EXIT_SUCCESS;
}

// Warp-aggregated atomic Max in multi bucket
#if __CUDA_ARCH__ >= 700
__device__ void atomicAggMaxMulti(const int bucket, int *counter, const int valueForMax)
{
    cg::coalesced_group active = cg::coalesced_threads();
    // group all threads with same bucket value.
    auto labeledGroup = cg::labeled_partition(active, bucket);

    const int maxValueInGroup = cg::reduce(labeledGroup, valueForMax, cg::greater<int>());

    if (labeledGroup.thread_rank() == 0) {
        atomicMax(&counter[bucket], maxValueInGroup);
    }
}
#endif

// Performs max calculation in each buckets.
__global__ void calculateMaxInEachBuckets(const int *srcArr,
                                          const int *valueInBuckets,
                                          int       *bucketsMax,
                                          const int  srcSize,
                                          const int  numOfBuckets)
{
#if __CUDA_ARCH__ >= 700
    cg::grid_group grid = cg::this_grid();

    for (int i = grid.thread_rank(); i < srcSize; i += grid.size()) {
        const int bucket = srcArr[i];
        if (bucket < numOfBuckets) {
            atomicAggMaxMulti(bucket, bucketsMax, valueInBuckets[i]);
        }
    }
#endif
}

int calculateMaxInBuckets(int *h_srcArr, int *d_srcArr, int numOfBuckets)
{
    int *d_valueInBuckets, *d_bucketsMax;
    int *h_valueInBuckets = new int[NUM_ELEMS];
    int *cpuBucketsMax    = new int[numOfBuckets];
    int *h_bucketsMax     = new int[numOfBuckets];

    memset(cpuBucketsMax, 0, sizeof(int) * numOfBuckets);

    // Here we create values which is assumed to correspond to each
    // buckets of srcArr at same array index.
    for (int i = 0; i < NUM_ELEMS; i++) {
        h_valueInBuckets[i] = rand();
    }

    checkCudaErrors(cudaMalloc(&d_valueInBuckets, sizeof(int) * NUM_ELEMS));
    checkCudaErrors(cudaMalloc(&d_bucketsMax, sizeof(int) * numOfBuckets));

    checkCudaErrors(cudaMemset(d_bucketsMax, 0, sizeof(int) * numOfBuckets));
    checkCudaErrors(cudaMemcpy(d_valueInBuckets, h_valueInBuckets, sizeof(int) * NUM_ELEMS, cudaMemcpyHostToDevice));

    dim3 dimBlock(NUM_THREADS_PER_BLOCK, 1, 1);
    dim3 dimGrid((NUM_ELEMS / NUM_THREADS_PER_BLOCK), 1, 1);

    calculateMaxInEachBuckets<<<dimGrid, dimBlock>>>(d_srcArr, d_valueInBuckets, d_bucketsMax, NUM_ELEMS, numOfBuckets);

    checkCudaErrors(cudaMemcpy(h_bucketsMax, d_bucketsMax, sizeof(int) * numOfBuckets, cudaMemcpyDeviceToHost));

    for (int i = 0; i < NUM_ELEMS; i++) {
        if (cpuBucketsMax[h_srcArr[i]] < h_valueInBuckets[i]) {
            cpuBucketsMax[h_srcArr[i]] = h_valueInBuckets[i];
        }
    }

    bool allMatch   = true;
    int  finalElems = 0;
    for (int i = 0; i < numOfBuckets; i++) {
        if (cpuBucketsMax[i] != h_bucketsMax[i]) {
            allMatch = false;
            printf("CPU i=%d  max = %d mismatches GPU max = %d\n", i, cpuBucketsMax[i], h_bucketsMax[i]);
            break;
        }
    }
    if (allMatch) {
        printf("CPU max matches GPU max\n");
    }

    delete[] h_valueInBuckets;
    delete[] cpuBucketsMax;
    delete[] h_bucketsMax;
    checkCudaErrors(cudaFree(d_valueInBuckets));
    checkCudaErrors(cudaFree(d_bucketsMax));

    if (!allMatch && finalElems != NUM_ELEMS) {
        return EXIT_FAILURE;
    }

    return EXIT_SUCCESS;
}

int main(int argc, char **argv)
{
    int *data_to_filter, *filtered_data, nres = 0;
    int *d_data_to_filter, *d_filtered_data, *d_nres;

    int numOfBuckets = 5;

    data_to_filter = reinterpret_cast<int *>(malloc(sizeof(int) * NUM_ELEMS));

    // Generate input data.
    for (int i = 0; i < NUM_ELEMS; i++) {
        data_to_filter[i] = rand() % numOfBuckets;
    }

    int devId = findCudaDevice(argc, (const char **)argv);

    checkCudaErrors(cudaMalloc(&d_data_to_filter, sizeof(int) * NUM_ELEMS));
    checkCudaErrors(cudaMalloc(&d_filtered_data, sizeof(int) * NUM_ELEMS));
    checkCudaErrors(cudaMalloc(&d_nres, sizeof(int)));

    checkCudaErrors(cudaMemcpy(d_data_to_filter, data_to_filter, sizeof(int) * NUM_ELEMS, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemset(d_nres, 0, sizeof(int)));

    dim3 dimBlock(NUM_THREADS_PER_BLOCK, 1, 1);
    dim3 dimGrid((NUM_ELEMS / NUM_THREADS_PER_BLOCK) + 1, 1, 1);

    filter_arr<<<dimGrid, dimBlock>>>(d_filtered_data, d_nres, d_data_to_filter, NUM_ELEMS);

    checkCudaErrors(cudaMemcpy(&nres, d_nres, sizeof(int), cudaMemcpyDeviceToHost));

    filtered_data = reinterpret_cast<int *>(malloc(sizeof(int) * nres));

    checkCudaErrors(cudaMemcpy(filtered_data, d_filtered_data, sizeof(int) * nres, cudaMemcpyDeviceToHost));

    int *host_filtered_data = reinterpret_cast<int *>(malloc(sizeof(int) * NUM_ELEMS));

    // Generate host output with host filtering code.
    int host_flt_count = 0;
    for (int i = 0; i < NUM_ELEMS; i++) {
        if (data_to_filter[i] > 0) {
            host_filtered_data[host_flt_count++] = data_to_filter[i];
        }
    }

    int major = 0;
    checkCudaErrors(cudaDeviceGetAttribute(&major, cudaDevAttrComputeCapabilityMajor, devId));

    int mapIndicesToBucketsStatus   = EXIT_SUCCESS;
    int calculateMaxInBucketsStatus = EXIT_SUCCESS;
    // atomicAggIncMulti & atomicAggMaxMulti require a GPU of Volta (SM7X) architecture or higher,
    // so that it can take advantage of the new MATCH capability of Volta hardware
    if (major >= 7) {
        mapIndicesToBucketsStatus   = mapIndicesToBuckets(data_to_filter, d_data_to_filter, numOfBuckets);
        calculateMaxInBucketsStatus = calculateMaxInBuckets(data_to_filter, d_data_to_filter, numOfBuckets);
    }

    printf("\nWarp Aggregated Atomics %s \n",
           (host_flt_count == nres) && (mapIndicesToBucketsStatus == EXIT_SUCCESS)
                   && (calculateMaxInBucketsStatus == EXIT_SUCCESS)
               ? "PASSED"
               : "FAILED");

    checkCudaErrors(cudaFree(d_data_to_filter));
    checkCudaErrors(cudaFree(d_filtered_data));
    checkCudaErrors(cudaFree(d_nres));
    free(data_to_filter);
    free(filtered_data);
    free(host_filtered_data);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 314
- **Approximate Size**: 11301 bytes

### Content Structure

#### Functions and Kernels

This file contains function definitions and potentially CUDA kernel launches.
Functions in this file handle:

- **Initialization**: Setting up CUDA context and allocating resources
- **Computation**: Core algorithmic implementations
- **Cleanup**: Freeing resources and error checking

#### Error Handling

The code implements error handling through:

- CUDA error checking macros
- Return code validation
- Exception handling where appropriate

#### Memory Management

Memory operations include:

- Device memory allocation (cudaMalloc)
- Host memory allocation
- Memory transfers (cudaMemcpy)
- Proper cleanup and deallocation

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

### Computational Complexity

The algorithms in this file are designed with performance in mind:

- **GPU Parallelism**: Leveraging thousands of CUDA cores
- **Memory Bandwidth**: Optimizing data transfer patterns
- **Occupancy**: Maximizing GPU utilization
- **Latency Hiding**: Using asynchronous operations where beneficial

### Optimization Opportunities

Potential areas for optimization:

1. Kernel launch configuration tuning
2. Shared memory usage
3. Coalesced memory access
4. Reduction of host-device transfers

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

To test this file:

1. Build the sample using CMake
2. Run the executable with appropriate parameters
3. Verify output against expected results
4. Check for memory leaks using cuda-memcheck
5. Profile performance using NVIDIA profiling tools

### Integration Tests

This file is tested as part of the overall sample application, ensuring:

- Correct functionality
- Expected performance characteristics
- Compatibility across different GPU architectures

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Running

```bash
./{executable_name} [options]
```

Refer to the sample's README for specific command-line options and usage patterns.

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
