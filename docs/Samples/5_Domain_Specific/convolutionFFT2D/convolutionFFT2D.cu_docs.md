# Documentation for Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/convolutionFFT2D
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

#include <assert.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Other helpers
#include <helper_cuda.h>

// Project includes
#include "convolutionFFT2D.cuh"
#include "convolutionFFT2D_common.h"

////////////////////////////////////////////////////////////////////////////////
/// Position convolution kernel center at (0, 0) in the image
////////////////////////////////////////////////////////////////////////////////
extern "C" void
padKernel(float *d_Dst, float *d_Src, int fftH, int fftW, int kernelH, int kernelW, int kernelY, int kernelX)
{
    assert(d_Src != d_Dst);
    dim3 threads(32, 8);
    dim3 grid(iDivUp(kernelW, threads.x), iDivUp(kernelH, threads.y));

    SET_FLOAT_BASE;
#if (USE_TEXTURE)
    cudaTextureObject_t texFloat;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                = cudaResourceTypeLinear;
    texRes.res.linear.devPtr      = d_Src;
    texRes.res.linear.sizeInBytes = sizeof(float) * kernelH * kernelW;
    texRes.res.linear.desc        = cudaCreateChannelDesc<float>();

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texFloat, &texRes, &texDescr, NULL));
#endif

    padKernel_kernel<<<grid, threads>>>(d_Dst,
                                        d_Src,
                                        fftH,
                                        fftW,
                                        kernelH,
                                        kernelW,
                                        kernelY,
                                        kernelX
#if (USE_TEXTURE)
                                        ,
                                        texFloat
#endif
    );
    getLastCudaError("padKernel_kernel<<<>>> execution failed\n");

#if (USE_TEXTURE)
    checkCudaErrors(cudaDestroyTextureObject(texFloat));
#endif
}

////////////////////////////////////////////////////////////////////////////////
// Prepare data for "pad to border" addressing mode
////////////////////////////////////////////////////////////////////////////////
extern "C" void padDataClampToBorder(float *d_Dst,
                                     float *d_Src,
                                     int    fftH,
                                     int    fftW,
                                     int    dataH,
                                     int    dataW,
                                     int    kernelW,
                                     int    kernelH,
                                     int    kernelY,
                                     int    kernelX)
{
    assert(d_Src != d_Dst);
    dim3 threads(32, 8);
    dim3 grid(iDivUp(fftW, threads.x), iDivUp(fftH, threads.y));

#if (USE_TEXTURE)
    cudaTextureObject_t texFloat;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                = cudaResourceTypeLinear;
    texRes.res.linear.devPtr      = d_Src;
    texRes.res.linear.sizeInBytes = sizeof(float) * dataH * dataW;
    texRes.res.linear.desc        = cudaCreateChannelDesc<float>();

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texFloat, &texRes, &texDescr, NULL));
#endif

    padDataClampToBorder_kernel<<<grid, threads>>>(d_Dst,
                                                   d_Src,
                                                   fftH,
                                                   fftW,
                                                   dataH,
                                                   dataW,
                                                   kernelH,
                                                   kernelW,
                                                   kernelY,
                                                   kernelX
#if (USE_TEXTURE)
                                                   ,
                                                   texFloat
#endif
    );
    getLastCudaError("padDataClampToBorder_kernel<<<>>> execution failed\n");

#if (USE_TEXTURE)
    checkCudaErrors(cudaDestroyTextureObject(texFloat));
#endif
}

////////////////////////////////////////////////////////////////////////////////
// Modulate Fourier image of padded data by Fourier image of padded kernel
// and normalize by FFT size
////////////////////////////////////////////////////////////////////////////////
extern "C" void modulateAndNormalize(fComplex *d_Dst, fComplex *d_Src, int fftH, int fftW, int padding)
{
    assert(fftW % 2 == 0);
    const int dataSize = fftH * (fftW / 2 + padding);

    modulateAndNormalize_kernel<<<iDivUp(dataSize, 256), 256>>>(d_Dst, d_Src, dataSize, 1.0f / (float)(fftW * fftH));
    getLastCudaError("modulateAndNormalize() execution failed\n");
}

////////////////////////////////////////////////////////////////////////////////
// 2D R2C / C2R post/preprocessing kernels
////////////////////////////////////////////////////////////////////////////////
static const double PI       = 3.1415926535897932384626433832795;
static const uint   BLOCKDIM = 256;

extern "C" void spPostprocess2D(void *d_Dst, void *d_Src, uint DY, uint DX, uint padding, int dir)
{
    assert(d_Src != d_Dst);
    assert(DX % 2 == 0);

#if (POWER_OF_TWO)
    uint log2DX, log2DY;
    uint factorizationRemX = factorRadix2(log2DX, DX);
    uint factorizationRemY = factorRadix2(log2DY, DY);
    assert(factorizationRemX == 1 && factorizationRemY == 1);
#endif

    const uint   threadCount = DY * (DX / 2);
    const double phaseBase   = dir * PI / (double)DX;

#if (USE_TEXTURE)
    cudaTextureObject_t texComplex;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                = cudaResourceTypeLinear;
    texRes.res.linear.devPtr      = d_Src;
    texRes.res.linear.sizeInBytes = sizeof(fComplex) * DY * (DX + padding);
    texRes.res.linear.desc        = cudaCreateChannelDesc<fComplex>();

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texComplex, &texRes, &texDescr, NULL));
#endif

    spPostprocess2D_kernel<<<iDivUp(threadCount, BLOCKDIM), BLOCKDIM>>>((fComplex *)d_Dst,
                                                                        (fComplex *)d_Src,
                                                                        DY,
                                                                        DX,
                                                                        threadCount,
                                                                        padding,
                                                                        (float)phaseBase
#if (USE_TEXTURE)
                                                                        ,
                                                                        texComplex
#endif
    );
    getLastCudaError("spPostprocess2D_kernel<<<>>> execution failed\n");

#if (USE_TEXTURE)
    checkCudaErrors(cudaDestroyTextureObject(texComplex));
#endif
}

extern "C" void spPreprocess2D(void *d_Dst, void *d_Src, uint DY, uint DX, uint padding, int dir)
{
    assert(d_Src != d_Dst);
    assert(DX % 2 == 0);

#if (POWER_OF_TWO)
    uint log2DX, log2DY;
    uint factorizationRemX = factorRadix2(log2DX, DX);
    uint factorizationRemY = factorRadix2(log2DY, DY);
    assert(factorizationRemX == 1 && factorizationRemY == 1);
#endif

    const uint   threadCount = DY * (DX / 2);
    const double phaseBase   = -dir * PI / (double)DX;

#if (USE_TEXTURE)
    cudaTextureObject_t texComplex;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                = cudaResourceTypeLinear;
    texRes.res.linear.devPtr      = d_Src;
    texRes.res.linear.sizeInBytes = sizeof(fComplex) * DY * (DX + padding);
    texRes.res.linear.desc        = cudaCreateChannelDesc<fComplex>();

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texComplex, &texRes, &texDescr, NULL));
#endif
    spPreprocess2D_kernel<<<iDivUp(threadCount, BLOCKDIM), BLOCKDIM>>>((fComplex *)d_Dst,
                                                                       (fComplex *)d_Src,
                                                                       DY,
                                                                       DX,
                                                                       threadCount,
                                                                       padding,
                                                                       (float)phaseBase
#if (USE_TEXTURE)
                                                                       ,
                                                                       texComplex
#endif
    );
    getLastCudaError("spPreprocess2D_kernel<<<>>> execution failed\n");

#if (USE_TEXTURE)
    checkCudaErrors(cudaDestroyTextureObject(texComplex));
#endif
}

////////////////////////////////////////////////////////////////////////////////
// Combined spPostprocess2D + modulateAndNormalize + spPreprocess2D
////////////////////////////////////////////////////////////////////////////////
extern "C" void spProcess2D(void *d_Dst, void *d_SrcA, void *d_SrcB, uint DY, uint DX, int dir)
{
    assert(DY % 2 == 0);

#if (POWER_OF_TWO)
    uint log2DX, log2DY;
    uint factorizationRemX = factorRadix2(log2DX, DX);
    uint factorizationRemY = factorRadix2(log2DY, DY);
    assert(factorizationRemX == 1 && factorizationRemY == 1);
#endif

    const uint   threadCount = (DY / 2) * DX;
    const double phaseBase   = dir * PI / (double)DX;

#if (USE_TEXTURE)
    cudaTextureObject_t texComplexA, texComplexB;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                = cudaResourceTypeLinear;
    texRes.res.linear.devPtr      = d_SrcA;
    texRes.res.linear.sizeInBytes = sizeof(fComplex) * DY * DX;
    texRes.res.linear.desc        = cudaCreateChannelDesc<fComplex>();

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texComplexA, &texRes, &texDescr, NULL));

    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                = cudaResourceTypeLinear;
    texRes.res.linear.devPtr      = d_SrcB;
    texRes.res.linear.sizeInBytes = sizeof(fComplex) * DY * DX;
    texRes.res.linear.desc        = cudaCreateChannelDesc<fComplex>();

    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texComplexB, &texRes, &texDescr, NULL));
#endif
    spProcess2D_kernel<<<iDivUp(threadCount, BLOCKDIM), BLOCKDIM>>>((fComplex *)d_Dst,
                                                                    (fComplex *)d_SrcA,
                                                                    (fComplex *)d_SrcB,
                                                                    DY,
                                                                    DX,
                                                                    threadCount,
                                                                    (float)phaseBase,
                                                                    0.5f / (float)(DY * DX)
#if (USE_TEXTURE)
                                                                        ,
                                                                    texComplexA,
                                                                    texComplexB
#endif
    );
    getLastCudaError("spProcess2D_kernel<<<>>> execution failed\n");

#if (USE_TEXTURE)
    checkCudaErrors(cudaDestroyTextureObject(texComplexA));
    checkCudaErrors(cudaDestroyTextureObject(texComplexB));
#endif
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu`.

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

- **Total Lines**: 355
- **Approximate Size**: 14725 bytes

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
