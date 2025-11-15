# Documentation for Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu`
- **Type**: .cu
- **Location**: Samples/2_Concepts_and_Techniques/reductionMultiBlockCG
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

/*
  Parallel reduction

  This sample shows how to perform a reduction operation on an array of values
  to produce a single value in a single kernel (as opposed to two or more
  kernel calls as shown in the "reduction" CUDA Sample).  Single-pass
  reduction requires Cooperative Groups.

  Reductions are a very common computation in parallel algorithms.  Any time
  an array of values needs to be reduced to a single value using a binary
  associative operator, a reduction can be used.  Example applications include
  statistics computations such as mean and standard deviation, and image
  processing applications such as finding the total luminance of an
  image.

  This code performs sum reductions, but any associative operator such as
  min() or max() could also be used.

  It assumes the input size is a power of 2.

  COMMAND LINE ARGUMENTS

  "--n=<N>"         :Specify the number of elements to reduce (default 33554432)
  "--threads=<N>"   :Specify the number of threads per block (default 128)
  "--maxblocks=<N>" :Specify the maximum number of thread blocks to launch
 (kernel 6 only, default 64)
*/

// includes, system
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_functions.h>

const char *sSDKsample = "reductionMultiBlockCG";

#include <cooperative_groups.h>
#include <cooperative_groups/reduce.h>
#include <cuda_runtime_api.h>

namespace cg = cooperative_groups;

/*
  Parallel sum reduction using shared memory
  - takes log(n) steps for n input elements
  - uses n/2 threads
  - only works for power-of-2 arrays

  This version adds multiple elements per thread sequentially. This reduces the
  overall cost of the algorithm while keeping the work complexity O(n) and the
  step complexity O(log n).
  (Brent's Theorem optimization)

  See the CUDA SDK "reduction" sample for more information.
*/

__device__ void reduceBlock(double *sdata, const cg::thread_block &cta)
{
    const unsigned int        tid    = cta.thread_rank();
    cg::thread_block_tile<32> tile32 = cg::tiled_partition<32>(cta);

    sdata[tid] = cg::reduce(tile32, sdata[tid], cg::plus<double>());
    cg::sync(cta);

    double beta = 0.0;
    if (cta.thread_rank() == 0) {
        beta = 0;
        for (int i = 0; i < blockDim.x; i += tile32.size()) {
            beta += sdata[i];
        }
        sdata[0] = beta;
    }
    cg::sync(cta);
}

// This reduction kernel reduces an arbitrary size array in a single kernel
// invocation
//
// For more details on the reduction algorithm (notably the multi-pass
// approach), see the "reduction" sample in the CUDA SDK.
extern "C" __global__ void reduceSinglePassMultiBlockCG(const float *g_idata, float *g_odata, unsigned int n)
{
    // Handle to thread block group
    cg::thread_block block = cg::this_thread_block();
    cg::grid_group   grid  = cg::this_grid();

    extern double __shared__ sdata[];

    // Stride over grid and add the values to a shared memory buffer
    sdata[block.thread_rank()] = 0;

    for (int i = grid.thread_rank(); i < n; i += grid.size()) {
        sdata[block.thread_rank()] += g_idata[i];
    }

    cg::sync(block);

    // Reduce each block (called once per block)
    reduceBlock(sdata, block);
    // Write out the result to global memory
    if (block.thread_rank() == 0) {
        g_odata[blockIdx.x] = sdata[0];
    }
    cg::sync(grid);

    if (grid.thread_rank() == 0) {
        for (int block = 1; block < gridDim.x; block++) {
            g_odata[0] += g_odata[block];
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
// Wrapper function for kernel launch
////////////////////////////////////////////////////////////////////////////////
void call_reduceSinglePassMultiBlockCG(int size, int threads, int numBlocks, float *d_idata, float *d_odata)
{
    int   smemSize     = threads * sizeof(double);
    void *kernelArgs[] = {
        (void *)&d_idata,
        (void *)&d_odata,
        (void *)&size,
    };

    dim3 dimBlock(threads, 1, 1);
    dim3 dimGrid(numBlocks, 1, 1);

    cudaLaunchCooperativeKernel((void *)reduceSinglePassMultiBlockCG, dimGrid, dimBlock, kernelArgs, smemSize, NULL);
    // check if kernel execution generated an error
    getLastCudaError("Kernel execution failed");
}

////////////////////////////////////////////////////////////////////////////////
// declaration, forward
bool runTest(int argc, char **argv, int device);

////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    cudaDeviceProp deviceProp = {0};
    int            dev;

    printf("%s Starting...\n\n", sSDKsample);

    dev = findCudaDevice(argc, (const char **)argv);
    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, dev));
    if (!deviceProp.cooperativeLaunch) {
        printf("\nSelected GPU (%d) does not support Cooperative Kernel Launch, "
               "Waiving the run\n",
               dev);
        exit(EXIT_WAIVED);
    }

    bool bTestPassed = false;
    bTestPassed      = runTest(argc, argv, dev);

    exit(bTestPassed ? EXIT_SUCCESS : EXIT_FAILURE);
}

////////////////////////////////////////////////////////////////////////////////
//! Compute sum reduction on CPU
//! We use Kahan summation for an accurate sum of large arrays.
//! http://en.wikipedia.org/wiki/Kahan_summation_algorithm
//!
//! @param data       pointer to input data
//! @param size       number of input data elements
////////////////////////////////////////////////////////////////////////////////
template <class T> T reduceCPU(T *data, int size)
{
    T sum = data[0];
    T c   = (T)0.0;

    for (int i = 1; i < size; i++) {
        T y = data[i] - c;
        T t = sum + y;
        c   = (t - sum) - y;
        sum = t;
    }

    return sum;
}

unsigned int nextPow2(unsigned int x)
{
    --x;
    x |= x >> 1;
    x |= x >> 2;
    x |= x >> 4;
    x |= x >> 8;
    x |= x >> 16;
    return ++x;
}

////////////////////////////////////////////////////////////////////////////////
// Compute the number of threads and blocks to use for the reduction
// We set threads / block to the minimum of maxThreads and n/2.
////////////////////////////////////////////////////////////////////////////////
void getNumBlocksAndThreads(int n, int maxBlocks, int maxThreads, int &blocks, int &threads)
{
    if (n == 1) {
        threads = 1;
        blocks  = 1;
    }
    else {
        checkCudaErrors(cudaOccupancyMaxPotentialBlockSize(&blocks, &threads, reduceSinglePassMultiBlockCG));
    }

    blocks = min(maxBlocks, blocks);
}

////////////////////////////////////////////////////////////////////////////////
// This function performs a reduction of the input data multiple times and
// measures the average reduction time.
////////////////////////////////////////////////////////////////////////////////
float benchmarkReduce(int                 n,
                      int                 numThreads,
                      int                 numBlocks,
                      int                 maxThreads,
                      int                 maxBlocks,
                      int                 testIterations,
                      StopWatchInterface *timer,
                      float              *h_odata,
                      float              *d_idata,
                      float              *d_odata)
{
    float       gpu_result = 0;
    cudaError_t error;

    printf("\nLaunching %s kernel\n", "SinglePass Multi Block Cooperative Groups");
    for (int i = 0; i < testIterations; ++i) {
        gpu_result = 0;
        sdkStartTimer(&timer);
        call_reduceSinglePassMultiBlockCG(n, numThreads, numBlocks, d_idata, d_odata);
        cudaDeviceSynchronize();
        sdkStopTimer(&timer);
    }

    // copy final sum from device to host
    error = cudaMemcpy(&gpu_result, d_odata, sizeof(float), cudaMemcpyDeviceToHost);
    checkCudaErrors(error);

    return gpu_result;
}

////////////////////////////////////////////////////////////////////////////////
// The main function which runs the reduction test.
////////////////////////////////////////////////////////////////////////////////
bool runTest(int argc, char **argv, int device)
{
    int  size        = 1 << 25; // number of elements to reduce
    bool bTestPassed = false;

    if (checkCmdLineFlag(argc, (const char **)argv, "n")) {
        size = getCmdLineArgumentInt(argc, (const char **)argv, "n");
    }

    printf("%d elements\n", size);

    // Set the device to be used
    cudaDeviceProp prop = {0};
    checkCudaErrors(cudaSetDevice(device));
    checkCudaErrors(cudaGetDeviceProperties(&prop, device));

    // create random input data on CPU
    unsigned int bytes = size * sizeof(float);

    float *h_idata = (float *)malloc(bytes);

    for (int i = 0; i < size; i++) {
        // Keep the numbers small so we don't get truncation error in the sum
        h_idata[i] = (rand() & 0xFF) / (float)RAND_MAX;
    }

    // Determine the launch configuration (threads, blocks)
    int maxThreads = 0;
    int maxBlocks  = 0;

    if (checkCmdLineFlag(argc, (const char **)argv, "threads")) {
        maxThreads = getCmdLineArgumentInt(argc, (const char **)argv, "threads");
    }
    else {
        maxThreads = prop.maxThreadsPerBlock;
    }

    if (checkCmdLineFlag(argc, (const char **)argv, "maxblocks")) {
        maxBlocks = getCmdLineArgumentInt(argc, (const char **)argv, "maxblocks");
    }
    else {
        maxBlocks = prop.multiProcessorCount * (prop.maxThreadsPerMultiProcessor / prop.maxThreadsPerBlock);
    }

    int numBlocks  = 0;
    int numThreads = 0;
    getNumBlocksAndThreads(size, maxBlocks, maxThreads, numBlocks, numThreads);

    // We calculate the occupancy to know how many block can actually fit on the
    // GPU
    int numBlocksPerSm = 0;
    checkCudaErrors(cudaOccupancyMaxActiveBlocksPerMultiprocessor(
        &numBlocksPerSm, reduceSinglePassMultiBlockCG, numThreads, numThreads * sizeof(double)));

    int numSms = prop.multiProcessorCount;
    if (numBlocks > numBlocksPerSm * numSms) {
        numBlocks = numBlocksPerSm * numSms;
    }
    printf("numThreads: %d\n", numThreads);
    printf("numBlocks: %d\n", numBlocks);

    // allocate mem for the result on host side
    float *h_odata = (float *)malloc(numBlocks * sizeof(float));

    // allocate device memory and data
    float *d_idata = NULL;
    float *d_odata = NULL;

    checkCudaErrors(cudaMalloc((void **)&d_idata, bytes));
    checkCudaErrors(cudaMalloc((void **)&d_odata, numBlocks * sizeof(float)));

    // copy data directly to device memory
    checkCudaErrors(cudaMemcpy(d_idata, h_idata, bytes, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(d_odata, h_idata, numBlocks * sizeof(float), cudaMemcpyHostToDevice));

    int testIterations = 100;

    StopWatchInterface *timer = 0;
    sdkCreateTimer(&timer);

    float gpu_result = 0;

    gpu_result = benchmarkReduce(
        size, numThreads, numBlocks, maxThreads, maxBlocks, testIterations, timer, h_odata, d_idata, d_odata);

    float reduceTime = sdkGetAverageTimerValue(&timer);
    printf("Average time: %f ms\n", reduceTime);
    printf("Bandwidth:    %f GB/s\n\n", (size * sizeof(int)) / (reduceTime * 1.0e6));

    // compute reference solution
    float cpu_result = reduceCPU<float>(h_idata, size);
    printf("GPU result = %0.12f\n", gpu_result);
    printf("CPU result = %0.12f\n", cpu_result);

    double threshold = 1e-8 * size;
    double diff      = abs((double)gpu_result - (double)cpu_result);
    bTestPassed      = (diff < threshold);

    // cleanup
    sdkDeleteTimer(&timer);

    free(h_idata);
    free(h_odata);
    cudaFree(d_idata);
    cudaFree(d_odata);

    return bTestPassed;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu`.

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

- **Total Lines**: 390
- **Approximate Size**: 13500 bytes

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
