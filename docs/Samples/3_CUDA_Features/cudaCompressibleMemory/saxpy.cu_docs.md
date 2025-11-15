# Documentation for Samples/3_CUDA_Features/cudaCompressibleMemory/saxpy.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/cudaCompressibleMemory/saxpy.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/cudaCompressibleMemory
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

//
// This sample uses the compressible memory allocation if device supports it
// and performs saxpy on it.
// Compressible memory may give better performance if the data is amenable to
// compression.

#include <cuda.h>
#include <stdio.h>
#define CUDA_DRIVER_API
#include "compMalloc.h"
#include "helper_cuda.h"

__global__ void saxpy(const float a, const float4 *x, const float4 *y, float4 *z, const size_t n)
{
    for (size_t i = blockIdx.x * blockDim.x + threadIdx.x; i < n; i += gridDim.x * blockDim.x) {
        const float4 x4 = x[i];
        const float4 y4 = y[i];
        z[i]            = make_float4(a * x4.x + y4.x, a * x4.y + y4.y, a * x4.z + y4.z, a * x4.w + y4.w);
    }
}

__global__ void init(float4 *x, float4 *y, const float val, const size_t n)
{
    const float4 val4 = make_float4(val, val, val, val);
    for (size_t i = blockIdx.x * blockDim.x + threadIdx.x; i < n; i += gridDim.x * blockDim.x) {
        x[i] = y[i] = val4;
    }
}

void launchSaxpy(const float  a,
                 float4      *x,
                 float4      *y,
                 float4      *z,
                 const size_t n,
                 const float  init_val,
                 const bool   compressibleZbuf)
{
    cudaEvent_t start, stop;
    float       ms;
    int         blockSize;
    int         minGridSize;
    dim3        threads, blocks;

    if (!compressibleZbuf) {
        // We are on config where compressible buffer can only be initialized through cudaMemcpy
        // hence, x & y buffers are allocated as compressible and initialized via cudaMemcpy
        // whereas z buffer is allocated as non-compressible.
        float4 *h_x = (float4 *)malloc(sizeof(float4) * n);
        float4 *h_y = (float4 *)malloc(sizeof(float4) * n);
        for (int i = 0; i < n; i++) {
            h_x[i].x = h_x[i].y = h_x[i].z = h_x[i].w = init_val;
            h_y[i].x = h_y[i].y = h_y[i].z = h_y[i].w = init_val;
        }
        checkCudaErrors(cudaMemcpy(x, h_x, sizeof(float4) * n, cudaMemcpyHostToDevice));
        checkCudaErrors(cudaMemcpy(y, h_y, sizeof(float4) * n, cudaMemcpyHostToDevice));
        free(h_x);
        free(h_y);
    }
    else {
        checkCudaErrors(cudaOccupancyMaxPotentialBlockSize(&minGridSize, &blockSize, (void *)init));
        threads = dim3(blockSize, 1, 1);
        blocks  = dim3(minGridSize, 1, 1);
        init<<<blocks, threads>>>(x, y, init_val, n);
    }

    checkCudaErrors(cudaOccupancyMaxPotentialBlockSize(&minGridSize, &blockSize, (void *)saxpy));
    threads = dim3(blockSize, 1, 1);
    blocks  = dim3(minGridSize, 1, 1);

    checkCudaErrors(cudaEventCreate(&start));
    checkCudaErrors(cudaEventCreate(&stop));
    checkCudaErrors(cudaEventRecord(start));
    saxpy<<<blocks, threads>>>(a, x, y, z, n);
    checkCudaErrors(cudaEventRecord(stop));
    checkCudaErrors(cudaEventSynchronize(stop));
    checkCudaErrors(cudaEventElapsedTime(&ms, start, stop));

    const size_t size = n * sizeof(float4);
    printf("Running saxpy with %d blocks x %d threads = %.3f ms %.3f TB/s\n",
           blocks.x,
           threads.x,
           ms,
           (size * 3) / ms / 1e9);
}

int main(int argc, char **argv)
{
    const size_t n = 10485760;

    if (checkCmdLineFlag(argc, (const char **)argv, "help") || checkCmdLineFlag(argc, (const char **)argv, "?")) {
        printf("Usage -device=n (n >= 0 for deviceID)\n");
        exit(EXIT_SUCCESS);
    }

    findCudaDevice(argc, (const char **)argv);
    CUdevice currentDevice;
    checkCudaErrors(cuCtxGetDevice(&currentDevice));

    // Check that the selected device supports virtual memory management
    int vmm_supported = -1;
    checkCudaErrors(
        cuDeviceGetAttribute(&vmm_supported, CU_DEVICE_ATTRIBUTE_VIRTUAL_ADDRESS_MANAGEMENT_SUPPORTED, currentDevice));
    if (vmm_supported == 0) {
        printf("Device %d doesn't support Virtual Memory Management, waiving the execution.\n", currentDevice);
        exit(EXIT_WAIVED);
    }

    int isCompressionAvailable;
    checkCudaErrors(cuDeviceGetAttribute(
        &isCompressionAvailable, CU_DEVICE_ATTRIBUTE_GENERIC_COMPRESSION_SUPPORTED, currentDevice));
    if (isCompressionAvailable == 0) {
        printf("Device %d doesn't support Generic memory compression, waiving the execution.\n", currentDevice);
        exit(EXIT_WAIVED);
    }

    printf("Generic memory compression support is available\n");

    int major, minor;
    checkCudaErrors(cuDeviceGetAttribute(&major, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR, currentDevice));
    checkCudaErrors(cuDeviceGetAttribute(&minor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR, currentDevice));
    float4      *x, *y, *z;
    const size_t size = n * sizeof(float4);

    // Allocating compressible memory
    checkCudaErrors(allocateCompressible((void **)&x, size, true));
    checkCudaErrors(allocateCompressible((void **)&y, size, true));
    bool compressibleZbuf = 0;
    if ((major == 8 && minor == 0) || (major == 8 && minor == 6)) {
        // On SM 8.0 and 8.6 GPUs compressible buffer can only be initialized
        // through cudaMemcpy.
        printf("allocating non-compressible Z buffer\n");
        checkCudaErrors(allocateCompressible((void **)&z, size, false));
        compressibleZbuf = 0;
    }
    else {
        checkCudaErrors(allocateCompressible((void **)&z, size, true));
        compressibleZbuf = 1;
    }

    printf("Running saxpy on %zu bytes of Compressible memory\n", size);

    const float a        = 1.0f;
    const float init_val = 1.0f;
    launchSaxpy(a, x, y, z, n, init_val, compressibleZbuf);

    checkCudaErrors(freeCompressible(x, size, true));
    checkCudaErrors(freeCompressible(y, size, true));
    checkCudaErrors(freeCompressible(z, size, true));

    printf("Running saxpy on %zu bytes of Non-Compressible memory\n", size);
    // Allocating non-compressible memory
    checkCudaErrors(allocateCompressible((void **)&x, size, false));
    checkCudaErrors(allocateCompressible((void **)&y, size, false));
    checkCudaErrors(allocateCompressible((void **)&z, size, false));

    launchSaxpy(a, x, y, z, n, init_val, compressibleZbuf);

    checkCudaErrors(freeCompressible(x, size, false));
    checkCudaErrors(freeCompressible(y, size, false));
    checkCudaErrors(freeCompressible(z, size, false));

    printf("\nNOTE: The CUDA Samples are not meant for performance measurements. "
           "Results may vary when GPU Boost is enabled.\n");
    return EXIT_SUCCESS;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/cudaCompressibleMemory/saxpy.cu`.

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

- **Total Lines**: 193
- **Approximate Size**: 8035 bytes

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
