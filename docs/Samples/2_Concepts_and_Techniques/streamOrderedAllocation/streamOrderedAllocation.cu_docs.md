# Documentation for Samples/2_Concepts_and_Techniques/streamOrderedAllocation/streamOrderedAllocation.cu

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/streamOrderedAllocation/streamOrderedAllocation.cu`
- **Type**: .cu
- **Location**: Samples/2_Concepts_and_Techniques/streamOrderedAllocation
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
 * This sample demonstrates stream ordered memory allocation on a GPU using
 * cudaMallocAsync and cudaMemPool family of APIs.
 *
 * basicStreamOrderedAllocation(): demonstrates stream ordered allocation using
 * cudaMallocAsync/cudaFreeAsync APIs with default settings.
 *
 * streamOrderedAllocationPostSync(): demonstrates if there's a synchronization
 * in between allocations, then setting the release threshold on the pool will
 * make sure the synchronize will not free memory back to the OS.
 */

// System includes
#include <assert.h>
#include <climits>
#include <stdio.h>

// CUDA runtime
#include <cuda_runtime.h>

// helper functions and utilities to work with CUDA
#include <helper_cuda.h>
#include <helper_functions.h>

#define MAX_ITER 20

/* Add two vectors on the GPU */
__global__ void vectorAddGPU(const float *a, const float *b, float *c, int N)
{
    int idx = blockIdx.x * blockDim.x + threadIdx.x;

    if (idx < N) {
        c[idx] = a[idx] + b[idx];
    }
}

int basicStreamOrderedAllocation(const int dev, const int nelem, const float *a, const float *b, float *c)
{
    float *d_a, *d_b, *d_c; // Device buffers
    float  errorNorm, refNorm, ref, diff;
    size_t bytes = nelem * sizeof(float);

    cudaStream_t stream;
    printf("Starting basicStreamOrderedAllocation()\n");
    checkCudaErrors(cudaSetDevice(dev));
    checkCudaErrors(cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking));

    checkCudaErrors(cudaMallocAsync(&d_a, bytes, stream));
    checkCudaErrors(cudaMallocAsync(&d_b, bytes, stream));
    checkCudaErrors(cudaMallocAsync(&d_c, bytes, stream));
    checkCudaErrors(cudaMemcpyAsync(d_a, a, bytes, cudaMemcpyHostToDevice, stream));
    checkCudaErrors(cudaMemcpyAsync(d_b, b, bytes, cudaMemcpyHostToDevice, stream));

    dim3 block(256);
    dim3 grid((unsigned int)ceil(nelem / (float)block.x));
    vectorAddGPU<<<grid, block, 0, stream>>>(d_a, d_b, d_c, nelem);

    checkCudaErrors(cudaFreeAsync(d_a, stream));
    checkCudaErrors(cudaFreeAsync(d_b, stream));
    checkCudaErrors(cudaMemcpyAsync(c, d_c, bytes, cudaMemcpyDeviceToHost, stream));
    checkCudaErrors(cudaFreeAsync(d_c, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));

    /* Compare the results */
    printf("> Checking the results from vectorAddGPU() ...\n");
    errorNorm = 0.f;
    refNorm   = 0.f;

    for (int n = 0; n < nelem; n++) {
        ref  = a[n] + b[n];
        diff = c[n] - ref;
        errorNorm += diff * diff;
        refNorm += ref * ref;
    }

    errorNorm = (float)sqrt((double)errorNorm);
    refNorm   = (float)sqrt((double)refNorm);
    if (errorNorm / refNorm < 1.e-6f)
        printf("basicStreamOrderedAllocation PASSED\n");

    checkCudaErrors(cudaStreamDestroy(stream));

    return errorNorm / refNorm < 1.e-6f ? EXIT_SUCCESS : EXIT_FAILURE;
}

// streamOrderedAllocationPostSync(): demonstrates If the application wants the
// memory to persist in the pool beyond synchronization, then it sets the
// release threshold on the pool. This way, when the application reaches the
// "steady state", it is no longer allocating/freeing memory from the OS.
int streamOrderedAllocationPostSync(const int dev, const int nelem, const float *a, const float *b, float *c)
{
    float *d_a, *d_b, *d_c; // Device buffers
    float  errorNorm, refNorm, ref, diff;
    size_t bytes = nelem * sizeof(float);

    cudaStream_t  stream;
    cudaMemPool_t memPool;
    cudaEvent_t   start, end;
    printf("Starting streamOrderedAllocationPostSync()\n");
    checkCudaErrors(cudaSetDevice(dev));
    checkCudaErrors(cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking));
    checkCudaErrors(cudaEventCreate(&start));
    checkCudaErrors(cudaEventCreate(&end));

    checkCudaErrors(cudaDeviceGetDefaultMemPool(&memPool, dev));
    uint64_t thresholdVal = ULONG_MAX;
    // set high release threshold on the default pool so that cudaFreeAsync will
    // not actually release memory to the system. By default, the release
    // threshold for a memory pool is set to zero. This implies that the CUDA
    // driver is allowed to release a memory chunk back to the system as long as
    // it does not contain any active suballocations.
    checkCudaErrors(cudaMemPoolSetAttribute(memPool, cudaMemPoolAttrReleaseThreshold, (void *)&thresholdVal));

    // Record the start event
    checkCudaErrors(cudaEventRecord(start, stream));
    for (int i = 0; i < MAX_ITER; i++) {
        checkCudaErrors(cudaMallocAsync(&d_a, bytes, stream));
        checkCudaErrors(cudaMallocAsync(&d_b, bytes, stream));
        checkCudaErrors(cudaMallocAsync(&d_c, bytes, stream));
        checkCudaErrors(cudaMemcpyAsync(d_a, a, bytes, cudaMemcpyHostToDevice, stream));
        checkCudaErrors(cudaMemcpyAsync(d_b, b, bytes, cudaMemcpyHostToDevice, stream));

        dim3 block(256);
        dim3 grid((unsigned int)ceil(nelem / (float)block.x));
        vectorAddGPU<<<grid, block, 0, stream>>>(d_a, d_b, d_c, nelem);

        checkCudaErrors(cudaFreeAsync(d_a, stream));
        checkCudaErrors(cudaFreeAsync(d_b, stream));
        checkCudaErrors(cudaMemcpyAsync(c, d_c, bytes, cudaMemcpyDeviceToHost, stream));
        checkCudaErrors(cudaFreeAsync(d_c, stream));
        checkCudaErrors(cudaStreamSynchronize(stream));
    }
    checkCudaErrors(cudaEventRecord(end, stream));
    // Wait for the end event to complete
    checkCudaErrors(cudaEventSynchronize(end));

    float msecTotal = 0.0f;
    checkCudaErrors(cudaEventElapsedTime(&msecTotal, start, end));
    printf("Total elapsed time = %f ms over %d iterations\n", msecTotal, MAX_ITER);

    /* Compare the results */
    printf("> Checking the results from vectorAddGPU() ...\n");
    errorNorm = 0.f;
    refNorm   = 0.f;

    for (int n = 0; n < nelem; n++) {
        ref  = a[n] + b[n];
        diff = c[n] - ref;
        errorNorm += diff * diff;
        refNorm += ref * ref;
    }

    errorNorm = (float)sqrt((double)errorNorm);
    refNorm   = (float)sqrt((double)refNorm);
    if (errorNorm / refNorm < 1.e-6f)
        printf("streamOrderedAllocationPostSync PASSED\n");

    checkCudaErrors(cudaStreamDestroy(stream));

    return errorNorm / refNorm < 1.e-6f ? EXIT_SUCCESS : EXIT_FAILURE;
}

int main(int argc, char **argv)
{
    int    nelem;
    int    dev = 0; // use default device 0
    size_t bytes;
    float *a, *b, *c; // Host

    if (checkCmdLineFlag(argc, (const char **)argv, "help")) {
        printf("Usage:  streamOrderedAllocation [OPTION]\n\n");
        printf("Options:\n");
        printf("  --device=[device #]  Specify the device to be used\n");
        return EXIT_SUCCESS;
    }

    dev = findCudaDevice(argc, (const char **)argv);

    int isMemPoolSupported = 0;
    checkCudaErrors(cudaDeviceGetAttribute(&isMemPoolSupported, cudaDevAttrMemoryPoolsSupported, dev));
    if (!isMemPoolSupported) {
        printf("Waiving execution as device does not support Memory Pools\n");
        exit(EXIT_WAIVED);
    }

    // Allocate CPU memory.
    nelem = 1048576;
    bytes = nelem * sizeof(float);

    a = (float *)malloc(bytes);
    b = (float *)malloc(bytes);
    c = (float *)malloc(bytes);
    /* Initialize the vectors. */
    for (int n = 0; n < nelem; n++) {
        a[n] = rand() / (float)RAND_MAX;
        b[n] = rand() / (float)RAND_MAX;
    }

    int ret1 = basicStreamOrderedAllocation(dev, nelem, a, b, c);
    int ret2 = streamOrderedAllocationPostSync(dev, nelem, a, b, c);

    /* Memory clean up */
    free(a);
    free(b);
    free(c);

    return ((ret1 == EXIT_SUCCESS && ret2 == EXIT_SUCCESS) ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/streamOrderedAllocation/streamOrderedAllocation.cu`.

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

- **Total Lines**: 236
- **Approximate Size**: 9161 bytes

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
