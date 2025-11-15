# Documentation for Samples/3_CUDA_Features/newdelete/newdelete.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/newdelete/newdelete.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/newdelete
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

// This sample demonstrates dynamic global memory allocation through device C++
// new and delete operators and virtual function declarations available with
// CUDA 4.0.

#include <cooperative_groups.h>
#include <stdio.h>

namespace cg = cooperative_groups;
#include <algorithm>
#include <helper_cuda.h>
#include <stdlib.h>
#include <vector>

const char *sSDKsample = "newdelete";

#include "container.hpp"

////////////////////////////////////////////////////////////////////////////////
//
// Kernels to allocate and instantiate Container objects on the device heap
//
////////////////////////////////////////////////////////////////////////////////

__global__ void vectorCreate(Container<int> **g_container, int max_size)
{
    // The Vector object and the data storage are allocated in device heap memory.
    // This makes it persistent for the lifetime of the CUDA context.
    // The grid has only one thread as only a single object instance is needed.

    *g_container = new Vector<int>(max_size);
}

////////////////////////////////////////////////////////////////////////////////
//
// Kernels to fill and consume shared Container objects.
//
////////////////////////////////////////////////////////////////////////////////

__global__ void containerFill(Container<int> **g_container)
{
    // All threads of the grid cooperatively populate the shared Container object
    // with data.
    if (threadIdx.x == 0) {
        (*g_container)->push(blockIdx.x);
    }
}

__global__ void containerConsume(Container<int> **g_container, int *d_result)
{
    // All threads of the grid cooperatively consume the data from the shared
    // Container object.
    int idx = blockIdx.x * blockDim.x + threadIdx.x;

    int v;

    if ((*g_container)->pop(v)) {
        d_result[idx] = v;
    }
    else {
        d_result[idx] = -1;
    }
}

////////////////////////////////////////////////////////////////////////////////
//
// Kernel to delete shared Container objects.
//
////////////////////////////////////////////////////////////////////////////////

__global__ void containerDelete(Container<int> **g_container) { delete *g_container; }

////////////////////////////////////////////////////////////////////////////////
//
// Kernels to using of placement new to put shared Vector objects and data in
// shared memory
//
////////////////////////////////////////////////////////////////////////////////

__global__ void placementNew(int *d_result)
{
    // Handle to thread block group
    cg::thread_block         cta = cg::this_thread_block();
    __shared__ unsigned char __align__(8) s_buffer[sizeof(Vector<int>)];
    __shared__ int           __align__(8) s_data[1024];
    __shared__ Vector<int> *s_vector;

    // The first thread of the block initializes the shared Vector object.
    // The placement new operator enables the Vector object and the data array top
    // be placed in shared memory.
    if (threadIdx.x == 0) {
        s_vector = new (s_buffer) Vector<int>(1024, s_data);
    }

    cg::sync(cta);

    if ((threadIdx.x & 1) == 0) {
        s_vector->push(threadIdx.x >> 1);
    }

    // Need to sync as the vector implementation does not support concurrent
    // push/pop operations.
    cg::sync(cta);

    int v;

    if (s_vector->pop(v)) {
        d_result[threadIdx.x] = v;
    }
    else {
        d_result[threadIdx.x] = -1;
    }

    // Note: deleting objects placed in shared memory is not necessary (lifetime
    // of shared memory is that of the block)
}

struct ComplexType_t
{
    int   a;
    int   b;
    float c;
    float d;
};

__global__ void complexVector(int *d_result)
{
    // Handle to thread block group
    cg::thread_block         cta = cg::this_thread_block();
    __shared__ unsigned char __align__(8) s_buffer[sizeof(Vector<ComplexType_t>)];
    __shared__ ComplexType_t __align__(8) s_data[1024];
    __shared__ Vector<ComplexType_t> *s_vector;

    // The first thread of the block initializes the shared Vector object.
    // The placement new operator enables the Vector object and the data array top
    // be placed in shared memory.
    if (threadIdx.x == 0) {
        s_vector = new (s_buffer) Vector<ComplexType_t>(1024, s_data);
    }

    cg::sync(cta);

    if ((threadIdx.x & 1) == 0) {
        ComplexType_t data;
        data.a = threadIdx.x >> 1;
        data.b = blockIdx.x;
        data.c = threadIdx.x / (float)(blockDim.x);
        data.d = blockIdx.x / (float)(gridDim.x);

        s_vector->push(data);
    }

    cg::sync(cta);

    ComplexType_t v;

    if (s_vector->pop(v)) {
        d_result[threadIdx.x] = v.a;
    }
    else {
        d_result[threadIdx.x] = -1;
    }

    // Note: deleting objects placed in shared memory is not necessary (lifetime
    // of shared memory is that of the block)
}

////////////////////////////////////////////////////////////////////////////////
//
// Host code
//
////////////////////////////////////////////////////////////////////////////////

bool checkResult(int *d_result, int N)
{
    std::vector<int> h_result;
    h_result.resize(N);

    checkCudaErrors(cudaMemcpy(&h_result[0], d_result, N * sizeof(int), cudaMemcpyDeviceToHost));
    std::sort(h_result.begin(), h_result.end());

    bool success = true;
    bool test    = false;

    int value = 0;

    for (int i = 0; i < N; ++i) {
        if (h_result[i] != -1) {
            test = true;
        }

        if (test && (value++) != h_result[i]) {
            success = false;
        }
    }

    return success;
}

bool testContainer(Container<int> **d_container, int blocks, int threads)
{
    int *d_result;
    cudaMalloc(&d_result, blocks * threads * sizeof(int));

    containerFill<<<blocks, threads>>>(d_container);
    containerConsume<<<blocks, threads>>>(d_container, d_result);
    containerDelete<<<1, 1>>>(d_container);
    checkCudaErrors(cudaDeviceSynchronize());

    bool success = checkResult(d_result, blocks * threads);

    cudaFree(d_result);

    return success;
}

bool testPlacementNew(int threads)
{
    int *d_result;
    cudaMalloc(&d_result, threads * sizeof(int));

    placementNew<<<1, threads>>>(d_result);
    checkCudaErrors(cudaDeviceSynchronize());

    bool success = checkResult(d_result, threads);

    cudaFree(d_result);

    return success;
}

bool testComplexType(int threads)
{
    int *d_result;
    cudaMalloc(&d_result, threads * sizeof(int));

    complexVector<<<1, threads>>>(d_result);
    checkCudaErrors(cudaDeviceSynchronize());

    bool success = checkResult(d_result, threads);

    cudaFree(d_result);

    return success;
}

////////////////////////////////////////////////////////////////////////////////
//
// MAIN
//
////////////////////////////////////////////////////////////////////////////////

int main(int argc, char **argv)
{
    printf("%s Starting...\n\n", sSDKsample);

    // use command-line specified CUDA device, otherwise use device with highest
    // Gflops/s
    findCudaDevice(argc, (const char **)argv);

    // set the heap size for device size new/delete to 128 MB
    checkCudaErrors(cudaDeviceSetLimit(cudaLimitMallocHeapSize, 128 * (1 << 20)));

    Container<int> **d_container;
    checkCudaErrors(cudaMalloc(&d_container, sizeof(Container<int> **)));

    bool bTest       = false;
    int  test_passed = 0;

    printf(" > Container = Vector test ");
    vectorCreate<<<1, 1>>>(d_container, 128 * 128);
    bTest = testContainer(d_container, 128, 128);
    printf(bTest ? "OK\n\n" : "NOT OK\n\n");
    test_passed += (bTest ? 1 : 0);

    checkCudaErrors(cudaFree(d_container));

    printf(" > Container = Vector, using placement new on SMEM buffer test ");
    bTest = testPlacementNew(1024);
    printf(bTest ? "OK\n\n" : "NOT OK\n\n");
    test_passed += (bTest ? 1 : 0);

    printf(" > Container = Vector, with user defined datatype test ");
    bTest = testComplexType(1024);
    printf(bTest ? "OK\n\n" : "NOT OK\n\n");
    test_passed += (bTest ? 1 : 0);

    printf("Test Summary: %d/3 succesfully run\n", test_passed);

    exit(test_passed == 3 ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/newdelete/newdelete.cu`.

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

- **Total Lines**: 318
- **Approximate Size**: 9643 bytes

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
