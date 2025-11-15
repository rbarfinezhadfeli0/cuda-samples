# Documentation for Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu

## File Metadata

- **Path**: `Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/fp16ScalarProduct
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

#include <cstdio>
#include <cstdlib>
#include <ctime>

#include "cuda_fp16.h"
#include "helper_cuda.h"

#define NUM_OF_BLOCKS  128
#define NUM_OF_THREADS 128

__forceinline__ __device__ void reduceInShared_intrinsics(half2 *const v)
{
    if (threadIdx.x < 64)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 64]);
    __syncthreads();
    if (threadIdx.x < 32)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 32]);
    __syncthreads();
    if (threadIdx.x < 16)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 16]);
    __syncthreads();
    if (threadIdx.x < 8)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 8]);
    __syncthreads();
    if (threadIdx.x < 4)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 4]);
    __syncthreads();
    if (threadIdx.x < 2)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 2]);
    __syncthreads();
    if (threadIdx.x < 1)
        v[threadIdx.x] = __hadd2(v[threadIdx.x], v[threadIdx.x + 1]);
    __syncthreads();
}

__forceinline__ __device__ void reduceInShared_native(half2 *const v)
{
    if (threadIdx.x < 64)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 64];
    __syncthreads();
    if (threadIdx.x < 32)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 32];
    __syncthreads();
    if (threadIdx.x < 16)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 16];
    __syncthreads();
    if (threadIdx.x < 8)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 8];
    __syncthreads();
    if (threadIdx.x < 4)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 4];
    __syncthreads();
    if (threadIdx.x < 2)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 2];
    __syncthreads();
    if (threadIdx.x < 1)
        v[threadIdx.x] = v[threadIdx.x] + v[threadIdx.x + 1];
    __syncthreads();
}

__global__ void
scalarProductKernel_intrinsics(half2 const *const a, half2 const *const b, float *const results, size_t const size)
{
    const int        stride = gridDim.x * blockDim.x;
    __shared__ half2 shArray[NUM_OF_THREADS];

    shArray[threadIdx.x] = __float2half2_rn(0.f);
    half2 value          = __float2half2_rn(0.f);

    for (int i = threadIdx.x + blockDim.x + blockIdx.x; i < size; i += stride) {
        value = __hfma2(a[i], b[i], value);
    }

    shArray[threadIdx.x] = value;
    __syncthreads();
    reduceInShared_intrinsics(shArray);

    if (threadIdx.x == 0) {
        half2 result        = shArray[0];
        float f_result      = __low2float(result) + __high2float(result);
        results[blockIdx.x] = f_result;
    }
}

__global__ void
scalarProductKernel_native(half2 const *const a, half2 const *const b, float *const results, size_t const size)
{
    const int        stride = gridDim.x * blockDim.x;
    __shared__ half2 shArray[NUM_OF_THREADS];

    half2 value(0.f, 0.f);
    shArray[threadIdx.x] = value;

    for (int i = threadIdx.x + blockDim.x + blockIdx.x; i < size; i += stride) {
        value = a[i] * b[i] + value;
    }

    shArray[threadIdx.x] = value;
    __syncthreads();
    reduceInShared_native(shArray);

    if (threadIdx.x == 0) {
        half2 result        = shArray[0];
        float f_result      = (float)result.y + (float)result.x;
        results[blockIdx.x] = f_result;
    }
}

void generateInput(half2 *a, size_t size)
{
    for (size_t i = 0; i < size; ++i) {
        half2 temp;
        temp.x = static_cast<float>(rand() % 4);
        temp.y = static_cast<float>(rand() % 2);
        a[i]   = temp;
    }
}

int main(int argc, char *argv[])
{
    srand((unsigned int)time(NULL));
    size_t size = NUM_OF_BLOCKS * NUM_OF_THREADS * 16;

    half2 *vec[2];
    half2 *devVec[2];

    float *results;
    float *devResults;

    int devID = findCudaDevice(argc, (const char **)argv);

    cudaDeviceProp devProp;
    checkCudaErrors(cudaGetDeviceProperties(&devProp, devID));

    if (devProp.major < 5 || (devProp.major == 5 && devProp.minor < 3)) {
        printf("ERROR: fp16ScalarProduct requires GPU devices with compute SM 5.3 or "
               "higher.\n");
        return EXIT_WAIVED;
    }

    for (int i = 0; i < 2; ++i) {
        checkCudaErrors(cudaMallocHost((void **)&vec[i], size * sizeof *vec[i]));
        checkCudaErrors(cudaMalloc((void **)&devVec[i], size * sizeof *devVec[i]));
    }

    checkCudaErrors(cudaMallocHost((void **)&results, NUM_OF_BLOCKS * sizeof *results));
    checkCudaErrors(cudaMalloc((void **)&devResults, NUM_OF_BLOCKS * sizeof *devResults));

    for (int i = 0; i < 2; ++i) {
        generateInput(vec[i], size);
        checkCudaErrors(cudaMemcpy(devVec[i], vec[i], size * sizeof *vec[i], cudaMemcpyHostToDevice));
    }

    scalarProductKernel_native<<<NUM_OF_BLOCKS, NUM_OF_THREADS>>>(devVec[0], devVec[1], devResults, size);

    checkCudaErrors(cudaMemcpy(results, devResults, NUM_OF_BLOCKS * sizeof *results, cudaMemcpyDeviceToHost));

    float result_native = 0;
    for (int i = 0; i < NUM_OF_BLOCKS; ++i) {
        result_native += results[i];
    }
    printf("Result native operators\t: %f \n", result_native);

    scalarProductKernel_intrinsics<<<NUM_OF_BLOCKS, NUM_OF_THREADS>>>(devVec[0], devVec[1], devResults, size);

    checkCudaErrors(cudaMemcpy(results, devResults, NUM_OF_BLOCKS * sizeof *results, cudaMemcpyDeviceToHost));

    float result_intrinsics = 0;
    for (int i = 0; i < NUM_OF_BLOCKS; ++i) {
        result_intrinsics += results[i];
    }
    printf("Result intrinsics\t: %f \n", result_intrinsics);

    printf("&&&& fp16ScalarProduct %s\n", (fabs(result_intrinsics - result_native) < 0.00001) ? "PASSED" : "FAILED");

    for (int i = 0; i < 2; ++i) {
        checkCudaErrors(cudaFree(devVec[i]));
        checkCudaErrors(cudaFreeHost(vec[i]));
    }

    checkCudaErrors(cudaFree(devResults));
    checkCudaErrors(cudaFreeHost(results));

    return EXIT_SUCCESS;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu`.

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

- **Total Lines**: 213
- **Approximate Size**: 7483 bytes

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
