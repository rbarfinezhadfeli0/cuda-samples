# Documentation for Samples/0_Introduction/simpleAttributes/simpleAttributes.cu

## File Metadata

- **Path**: `Samples/0_Introduction/simpleAttributes/simpleAttributes.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/simpleAttributes
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

// includes, system
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes CUDA
#include <cuda_runtime.h>

// includes, project
#include <helper_cuda.h>
#include <helper_functions.h> // helper functions for SDK examples

////////////////////////////////////////////////////////////////////////////////
// declaration, forward
void runTest(int argc, char **argv);

cudaAccessPolicyWindow initAccessPolicyWindow(void)
{
    cudaAccessPolicyWindow accessPolicyWindow = {0};
    accessPolicyWindow.base_ptr               = (void *)0;
    accessPolicyWindow.num_bytes              = 0;
    accessPolicyWindow.hitRatio               = 0.f;
    accessPolicyWindow.hitProp                = cudaAccessPropertyNormal;
    accessPolicyWindow.missProp               = cudaAccessPropertyStreaming;
    return accessPolicyWindow;
}

////////////////////////////////////////////////////////////////////////////////
//! Simple test kernel for device functionality
//! @param data  input data in global memory
//! @param dataSize  input data size
//! @param bigData  input bigData in global memory
//! @param bigDataSize  input bigData size
//! @param hitcount how many data access are done within block
////////////////////////////////////////////////////////////////////////////////
static __global__ void kernCacheSegmentTest(int *data, int dataSize, int *trash, int bigDataSize, int hitCount)
{
    __shared__ unsigned int hit;
    int                     row    = blockIdx.y * blockDim.y + threadIdx.y;
    int                     col    = blockIdx.x * blockDim.x + threadIdx.x;
    int                     tID    = row * blockDim.y + col;
    uint32_t                psRand = tID;

    atomicExch(&hit, 0);
    __syncthreads();
    while (hit < hitCount) {
        psRand ^= psRand << 13;
        psRand ^= psRand >> 17;
        psRand ^= psRand << 5;

        int idx = tID - psRand;
        if (idx < 0) {
            idx = -idx;
        }

        if ((tID % 2) == 0) {
            data[psRand % dataSize] = data[psRand % dataSize] + data[idx % dataSize];
        }
        else {
            trash[psRand % bigDataSize] = trash[psRand % bigDataSize] + trash[idx % bigDataSize];
        }

        atomicAdd(&hit, 1);
    }
}
////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv) { runTest(argc, argv); }

////////////////////////////////////////////////////////////////////////////////
//! Run a simple test for CUDA
////////////////////////////////////////////////////////////////////////////////
void runTest(int argc, char **argv)
{
    bool                   bTestResult = true;
    cudaAccessPolicyWindow accessPolicyWindow;
    cudaDeviceProp         deviceProp;
    cudaStreamAttrValue    streamAttrValue;
    cudaStream_t           stream;
    cudaStreamAttrID       streamAttrID;
    dim3                   threads(32, 32);
    int                   *dataDevicePointer;
    int                   *dataHostPointer;
    int                    dataSize;
    int                   *bigDataDevicePointer;
    int                   *bigDataHostPointer;
    int                    bigDataSize;
    StopWatchInterface    *timer = 0;

    printf("%s Starting...\n\n", argv[0]);

    // use command-line specified CUDA device, otherwise use device with highest
    // Gflops/s
    int devID = findCudaDevice(argc, (const char **)argv);
    sdkCreateTimer(&timer);
    sdkStartTimer(&timer);
    // Get device properties
    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, devID));
    dim3 blocks(deviceProp.maxGridSize[1], 1);

    // Make sure device the l2 optimization
    if (deviceProp.persistingL2CacheMaxSize == 0) {
        printf("Waiving execution as device %d does not support persisting L2 "
               "Caching\n",
               devID);
        exit(EXIT_WAIVED);
    }

    // Create stream to assiocate with window
    checkCudaErrors(cudaStreamCreate(&stream));

    // Set the amount of l2 cache that will be persisting to maximum the device
    // can support
    checkCudaErrors(cudaDeviceSetLimit(cudaLimitPersistingL2CacheSize, deviceProp.persistingL2CacheMaxSize));

    // Stream attribute to set
    streamAttrID = cudaStreamAttributeAccessPolicyWindow;

    // Default window
    streamAttrValue.accessPolicyWindow = initAccessPolicyWindow();
    accessPolicyWindow                 = initAccessPolicyWindow();

    // Allocate size of both buffers
    bigDataSize = (deviceProp.l2CacheSize * 4) / sizeof(int);
    dataSize    = (deviceProp.l2CacheSize / 4) / sizeof(int);

    // Allocate data
    checkCudaErrors(cudaMallocHost(&dataHostPointer, dataSize * sizeof(int)));
    checkCudaErrors(cudaMallocHost(&bigDataHostPointer, bigDataSize * sizeof(int)));

    for (int i = 0; i < bigDataSize; ++i) {
        if (i < dataSize) {
            dataHostPointer[i] = i;
        }

        bigDataHostPointer[bigDataSize - i - 1] = i;
    }

    checkCudaErrors(cudaMalloc((void **)&dataDevicePointer, dataSize * sizeof(int)));
    checkCudaErrors(cudaMalloc((void **)&bigDataDevicePointer, bigDataSize * sizeof(int)));
    checkCudaErrors(
        cudaMemcpyAsync(dataDevicePointer, dataHostPointer, dataSize * sizeof(int), cudaMemcpyHostToDevice, stream));
    checkCudaErrors(cudaMemcpyAsync(
        bigDataDevicePointer, bigDataHostPointer, bigDataSize * sizeof(int), cudaMemcpyHostToDevice, stream));

    // Make a window for the buffer of interest
    accessPolicyWindow.base_ptr        = (void *)dataDevicePointer;
    accessPolicyWindow.num_bytes       = dataSize * sizeof(int);
    accessPolicyWindow.hitRatio        = 1.f;
    accessPolicyWindow.hitProp         = cudaAccessPropertyPersisting;
    accessPolicyWindow.missProp        = cudaAccessPropertyNormal;
    streamAttrValue.accessPolicyWindow = accessPolicyWindow;

    // Assign window to stream
    checkCudaErrors(cudaStreamSetAttribute(stream, streamAttrID, &streamAttrValue));

    // Demote any previous persisting lines
    checkCudaErrors(cudaCtxResetPersistingL2Cache());

    checkCudaErrors(cudaStreamSynchronize(stream));
    kernCacheSegmentTest<<<blocks, threads, 0, stream>>>(
        dataDevicePointer, dataSize, bigDataDevicePointer, bigDataSize, 0xAFFFF);

    checkCudaErrors(cudaStreamSynchronize(stream));
    // check if kernel execution generated and error
    getLastCudaError("Kernel execution failed");

    // Free memory
    checkCudaErrors(cudaFreeHost(dataHostPointer));
    checkCudaErrors(cudaFreeHost(bigDataHostPointer));
    checkCudaErrors(cudaFree(dataDevicePointer));
    checkCudaErrors(cudaFree(bigDataDevicePointer));

    sdkStopTimer(&timer);
    printf("Processing time: %f (ms)\n", sdkGetTimerValue(&timer));
    sdkDeleteTimer(&timer);

    exit(bTestResult ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/simpleAttributes/simpleAttributes.cu`.

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

- **Total Lines**: 209
- **Approximate Size**: 8485 bytes

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
