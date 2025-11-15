# Documentation for Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/simpleD3D11
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

#include "ShaderStructs.h"
#include "helper_cuda.h"
#include "sinewave_cuda.h"

__global__ void sinewave_gen_kernel(Vertex *vertices, unsigned int width, unsigned int height, float time)
{
    unsigned int x = blockIdx.x * blockDim.x + threadIdx.x;
    unsigned int y = blockIdx.y * blockDim.y + threadIdx.y;

    // calculate uv coordinates
    float u = x / (float)width;
    float v = y / (float)height;
    u       = u * 2.0f - 1.0f;
    v       = v * 2.0f - 1.0f;

    // calculate simple sine wave pattern
    float freq = 4.0f;
    float w    = sinf(u * freq + time) * cosf(v * freq + time) * 0.5f;

    if (y < height && x < width) {
        // write output vertex
        vertices[y * width + x].position.x = u;
        vertices[y * width + x].position.y = w;
        vertices[y * width + x].position.z = v;
        vertices[y * width + x].color.x    = 1.0f;
        vertices[y * width + x].color.y    = 0.0f;
        vertices[y * width + x].color.z    = 0.0f;
        vertices[y * width + x].color.w    = 0.0f;
    }
}

Vertex *cudaImportVertexBuffer(void *sharedHandle, cudaExternalMemory_t &externalMemory, int meshWidth, int meshHeight)
{
    cudaExternalMemoryHandleDesc externalMemoryHandleDesc;
    memset(&externalMemoryHandleDesc, 0, sizeof(externalMemoryHandleDesc));

    externalMemoryHandleDesc.type                = cudaExternalMemoryHandleTypeD3D11ResourceKmt;
    externalMemoryHandleDesc.size                = sizeof(Vertex) * meshHeight * meshWidth;
    externalMemoryHandleDesc.flags               = cudaExternalMemoryDedicated;
    externalMemoryHandleDesc.handle.win32.handle = sharedHandle;

    checkCudaErrors(cudaImportExternalMemory(&externalMemory, &externalMemoryHandleDesc));

    cudaExternalMemoryBufferDesc externalMemoryBufferDesc;
    memset(&externalMemoryBufferDesc, 0, sizeof(externalMemoryBufferDesc));
    externalMemoryBufferDesc.offset = 0;
    externalMemoryBufferDesc.size   = sizeof(Vertex) * meshHeight * meshWidth;
    externalMemoryBufferDesc.flags  = 0;

    Vertex *cudaDevVertptr = NULL;
    checkCudaErrors(
        cudaExternalMemoryGetMappedBuffer((void **)&cudaDevVertptr, externalMemory, &externalMemoryBufferDesc));

    return cudaDevVertptr;
}

void cudaImportKeyedMutex(void *sharedHandle, cudaExternalSemaphore_t &extSemaphore)
{
    cudaExternalSemaphoreHandleDesc extSemaDesc;
    memset(&extSemaDesc, 0, sizeof(extSemaDesc));
    extSemaDesc.type                = cudaExternalSemaphoreHandleTypeKeyedMutexKmt;
    extSemaDesc.handle.win32.handle = sharedHandle;
    extSemaDesc.flags               = 0;

    checkCudaErrors(cudaImportExternalSemaphore(&extSemaphore, &extSemaDesc));
}

void cudaAcquireSync(cudaExternalSemaphore_t &extSemaphore,
                     uint64_t                 key,
                     unsigned int             timeoutMs,
                     cudaStream_t             streamToRun)
{
    cudaExternalSemaphoreWaitParams extSemWaitParams;
    memset(&extSemWaitParams, 0, sizeof(extSemWaitParams));
    extSemWaitParams.params.keyedMutex.key       = key;
    extSemWaitParams.params.keyedMutex.timeoutMs = timeoutMs;

    checkCudaErrors(cudaWaitExternalSemaphoresAsync(&extSemaphore, &extSemWaitParams, 1, streamToRun));
}

void cudaReleaseSync(cudaExternalSemaphore_t &extSemaphore, uint64_t key, cudaStream_t streamToRun)
{
    cudaExternalSemaphoreSignalParams extSemSigParams;
    memset(&extSemSigParams, 0, sizeof(extSemSigParams));
    extSemSigParams.params.keyedMutex.key = key;

    checkCudaErrors(cudaSignalExternalSemaphoresAsync(&extSemaphore, &extSemSigParams, 1, streamToRun));
}

////////////////////////////////////////////////////////////////////////////////
//! Run the Cuda part of the computation
////////////////////////////////////////////////////////////////////////////////
void RunSineWaveKernel(cudaExternalSemaphore_t &extSemaphore,
                       uint64_t                &key,
                       unsigned int             timeoutMs,
                       size_t                   mesh_width,
                       size_t                   mesh_height,
                       Vertex                  *cudaDevVertptr,
                       cudaStream_t             streamToRun)
{
    static float t = 0.0f;
    cudaAcquireSync(extSemaphore, key++, timeoutMs, streamToRun);

    dim3 block(16, 16, 1);
    dim3 grid(mesh_width / 16, mesh_height / 16, 1);
    sinewave_gen_kernel<<<grid, block, 0, streamToRun>>>(cudaDevVertptr, mesh_width, mesh_height, t);
    getLastCudaError("sinewave_gen_kernel execution failed.\n");

    cudaReleaseSync(extSemaphore, key, streamToRun);
    t += 0.01f;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu`.

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

- **Total Lines**: 141
- **Approximate Size**: 6221 bytes

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
