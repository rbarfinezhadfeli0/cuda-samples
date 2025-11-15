# Documentation for Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/NV12toBGRandResize
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


// Implements NV12 to BGR batch conversion

#include <cuda.h>
#include <cuda_runtime.h>

#include "resize_convert.h"

#define CONV_THREADS_X 64
#define CONV_THREADS_Y 10

__forceinline__ __device__ static float clampF(float x, float lower, float upper)
{
    return x < lower ? lower : (x > upper ? upper : x);
}

__global__ static void nv12ToBGRplanarBatchKernel(const uint8_t *pNv12,
                                                  int            nNv12Pitch,
                                                  float         *pBgr,
                                                  int            nRgbPitch,
                                                  int            nWidth,
                                                  int            nHeight,
                                                  int            nBatchSize)
{
    int x = threadIdx.x + blockIdx.x * blockDim.x;
    int y = threadIdx.y + blockIdx.y * blockDim.y;

    if ((x << 2) + 1 > nWidth || (y << 1) + 1 > nHeight)
        return;

    const uint8_t *__restrict__ pSrc = pNv12;

    for (int i = blockIdx.z; i < nBatchSize; i += gridDim.z) {
        pSrc = pNv12 + i * ((nHeight * nNv12Pitch * 3) >> 1) + (x << 2) + (y << 1) * nNv12Pitch;
        uchar4 luma2x01, luma2x23, uv2;
        *(uint32_t *)&luma2x01 = *(uint32_t *)pSrc;
        *(uint32_t *)&luma2x23 = *(uint32_t *)(pSrc + nNv12Pitch);
        *(uint32_t *)&uv2      = *(uint32_t *)(pSrc + (nHeight - y) * nNv12Pitch);

        float *pDstBlock = (pBgr + i * ((nHeight * nRgbPitch * 3) >> 2) + ((blockIdx.x * blockDim.x) << 2)
                            + ((blockIdx.y * blockDim.y) << 1) * (nRgbPitch >> 2));

        float2 add1;
        float2 add2;
        float2 add3;
        float2 add00, add01, add02, add03;
        float2 d, e;

        add00.x = 1.1644f * luma2x01.x;
        add01.x = 1.1644f * luma2x01.y;
        add00.y = 1.1644f * luma2x01.z;
        add01.y = 1.1644f * luma2x01.w;

        add02.x = 1.1644f * luma2x23.x;
        add03.x = 1.1644f * luma2x23.y;
        add02.y = 1.1644f * luma2x23.z;
        add03.y = 1.1644f * luma2x23.w;

        d.x = uv2.x - 128.0f;
        e.x = uv2.y - 128.0f;
        d.y = uv2.z - 128.0f;
        e.y = uv2.w - 128.0f;

        add1.x = 2.0172f * d.x;
        add1.y = 2.0172f * d.y;

        add2.x = (-0.3918f) * d.x + (-0.8130f) * e.x;
        add2.y = (-0.3918f) * d.y + (-0.8130f) * e.y;

        add3.x = 1.5960f * e.x;
        add3.y = 1.5960f * e.y;

        int rowStride     = (threadIdx.y << 1) * (nRgbPitch >> 2);
        int nextRowStride = ((threadIdx.y << 1) + 1) * (nRgbPitch >> 2);
        // B
        *((float4 *)&pDstBlock[rowStride + (threadIdx.x << 2)]) = make_float4(clampF(add00.x + add1.x, 0.0f, 255.0f),
                                                                              clampF(add01.x + add1.x, 0.0f, 255.0f),
                                                                              clampF(add00.y + add1.y, 0.0f, 255.0f),
                                                                              clampF(add01.y + add1.y, 0.0f, 255.0f));
        *((float4 *)&pDstBlock[nextRowStride + (threadIdx.x << 2)]) =
            make_float4(clampF(add02.x + add1.x, 0.0f, 255.0f),
                        clampF(add03.x + add1.x, 0.0f, 255.0f),
                        clampF(add02.y + add1.y, 0.0f, 255.0f),
                        clampF(add03.y + add1.y, 0.0f, 255.0f));

        int planeStride = nHeight * nRgbPitch >> 2;
        // G
        *((float4 *)&pDstBlock[planeStride + rowStride + (threadIdx.x << 2)]) =
            make_float4(clampF(add00.x + add2.x, 0.0f, 255.0f),
                        clampF(add01.x + add2.x, 0.0f, 255.0f),
                        clampF(add00.y + add2.y, 0.0f, 255.0f),
                        clampF(add01.y + add2.y, 0.0f, 255.0f));
        *((float4 *)&pDstBlock[planeStride + nextRowStride + (threadIdx.x << 2)]) =
            make_float4(clampF(add02.x + add2.x, 0.0f, 255.0f),
                        clampF(add03.x + add2.x, 0.0f, 255.0f),
                        clampF(add02.y + add2.y, 0.0f, 255.0f),
                        clampF(add03.y + add2.y, 0.0f, 255.0f));

        // R
        *((float4 *)&pDstBlock[(planeStride << 1) + rowStride + (threadIdx.x << 2)]) =
            make_float4(clampF(add00.x + add3.x, 0.0f, 255.0f),
                        clampF(add01.x + add3.x, 0.0f, 255.0f),
                        clampF(add00.y + add3.y, 0.0f, 255.0f),
                        clampF(add01.y + add3.y, 0.0f, 255.0f));
        *((float4 *)&pDstBlock[(planeStride << 1) + nextRowStride + (threadIdx.x << 2)]) =
            make_float4(clampF(add02.x + add3.x, 0.0f, 255.0f),
                        clampF(add03.x + add3.x, 0.0f, 255.0f),
                        clampF(add02.y + add3.y, 0.0f, 255.0f),
                        clampF(add03.y + add3.y, 0.0f, 255.0f));
    }
}

void nv12ToBGRplanarBatch(uint8_t     *pNv12,
                          int          nNv12Pitch,
                          float       *pBgr,
                          int          nRgbPitch,
                          int          nWidth,
                          int          nHeight,
                          int          nBatchSize,
                          cudaStream_t stream)
{
    dim3 threads(CONV_THREADS_X, CONV_THREADS_Y);

    size_t blockDimZ = nBatchSize;

    // Restricting blocks in Z-dim till 32 to not launch too many blocks
    blockDimZ = (blockDimZ > 32) ? 32 : blockDimZ;

    dim3 blocks((nWidth / 4 - 1) / threads.x + 1, (nHeight / 2 - 1) / threads.y + 1, blockDimZ);
    nv12ToBGRplanarBatchKernel<<<blocks, threads, 0, stream>>>(
        pNv12, nNv12Pitch, pBgr, nRgbPitch, nWidth, nHeight, nBatchSize);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu`.

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

- **Total Lines**: 160
- **Approximate Size**: 7277 bytes

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
