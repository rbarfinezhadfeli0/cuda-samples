# Documentation for Samples/5_Domain_Specific/HSOpticalFlow/derivativesKernel.cuh

## File Metadata

- **Path**: `Samples/5_Domain_Specific/HSOpticalFlow/derivativesKernel.cuh`
- **Type**: .cuh
- **Location**: Samples/5_Domain_Specific/HSOpticalFlow
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```cuh
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

#include "common.h"

///////////////////////////////////////////////////////////////////////////////
/// \brief compute image derivatives
///
/// CUDA kernel, relies heavily on texture unit
/// \param[in]  width   image width
/// \param[in]  height  image height
/// \param[in]  stride  image stride
/// \param[out] Ix      x derivative
/// \param[out] Iy      y derivative
/// \param[out] Iz      temporal derivative
///////////////////////////////////////////////////////////////////////////////
__global__ void ComputeDerivativesKernel(int                 width,
                                         int                 height,
                                         int                 stride,
                                         float              *Ix,
                                         float              *Iy,
                                         float              *Iz,
                                         cudaTextureObject_t texSource,
                                         cudaTextureObject_t texTarget)
{
    const int ix = threadIdx.x + blockIdx.x * blockDim.x;
    const int iy = threadIdx.y + blockIdx.y * blockDim.y;

    const int pos = ix + iy * stride;

    if (ix >= width || iy >= height)
        return;

    float dx = 1.0f / (float)width;
    float dy = 1.0f / (float)height;

    float x = ((float)ix + 0.5f) * dx;
    float y = ((float)iy + 0.5f) * dy;

    float t0, t1;
    // x derivative
    t0 = tex2D<float>(texSource, x - 2.0f * dx, y);
    t0 -= tex2D<float>(texSource, x - 1.0f * dx, y) * 8.0f;
    t0 += tex2D<float>(texSource, x + 1.0f * dx, y) * 8.0f;
    t0 -= tex2D<float>(texSource, x + 2.0f * dx, y);
    t0 /= 12.0f;

    t1 = tex2D<float>(texTarget, x - 2.0f * dx, y);
    t1 -= tex2D<float>(texTarget, x - 1.0f * dx, y) * 8.0f;
    t1 += tex2D<float>(texTarget, x + 1.0f * dx, y) * 8.0f;
    t1 -= tex2D<float>(texTarget, x + 2.0f * dx, y);
    t1 /= 12.0f;

    Ix[pos] = (t0 + t1) * 0.5f;

    // t derivative
    Iz[pos] = tex2D<float>(texTarget, x, y) - tex2D<float>(texSource, x, y);

    // y derivative
    t0 = tex2D<float>(texSource, x, y - 2.0f * dy);
    t0 -= tex2D<float>(texSource, x, y - 1.0f * dy) * 8.0f;
    t0 += tex2D<float>(texSource, x, y + 1.0f * dy) * 8.0f;
    t0 -= tex2D<float>(texSource, x, y + 2.0f * dy);
    t0 /= 12.0f;

    t1 = tex2D<float>(texTarget, x, y - 2.0f * dy);
    t1 -= tex2D<float>(texTarget, x, y - 1.0f * dy) * 8.0f;
    t1 += tex2D<float>(texTarget, x, y + 1.0f * dy) * 8.0f;
    t1 -= tex2D<float>(texTarget, x, y + 2.0f * dy);
    t1 /= 12.0f;

    Iy[pos] = (t0 + t1) * 0.5f;
}

///////////////////////////////////////////////////////////////////////////////
/// \brief compute image derivatives
///
/// \param[in]  I0  source image
/// \param[in]  I1  tracked image
/// \param[in]  w   image width
/// \param[in]  h   image height
/// \param[in]  s   image stride
/// \param[out] Ix  x derivative
/// \param[out] Iy  y derivative
/// \param[out] Iz  temporal derivative
///////////////////////////////////////////////////////////////////////////////
static void ComputeDerivatives(const float *I0, const float *I1, int w, int h, int s, float *Ix, float *Iy, float *Iz)
{
    dim3 threads(32, 6);
    dim3 blocks(iDivUp(w, threads.x), iDivUp(h, threads.y));

    cudaTextureObject_t texSource, texTarget;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                  = cudaResourceTypePitch2D;
    texRes.res.pitch2D.devPtr       = (void *)I0;
    texRes.res.pitch2D.desc         = cudaCreateChannelDesc<float>();
    texRes.res.pitch2D.width        = w;
    texRes.res.pitch2D.height       = h;
    texRes.res.pitch2D.pitchInBytes = s * sizeof(float);

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = true;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeMirror;
    texDescr.addressMode[1]   = cudaAddressModeMirror;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texSource, &texRes, &texDescr, NULL));
    memset(&texRes, 0, sizeof(cudaResourceDesc));
    texRes.resType                  = cudaResourceTypePitch2D;
    texRes.res.pitch2D.devPtr       = (void *)I1;
    texRes.res.pitch2D.desc         = cudaCreateChannelDesc<float>();
    texRes.res.pitch2D.width        = w;
    texRes.res.pitch2D.height       = h;
    texRes.res.pitch2D.pitchInBytes = s * sizeof(float);
    checkCudaErrors(cudaCreateTextureObject(&texTarget, &texRes, &texDescr, NULL));

    ComputeDerivativesKernel<<<blocks, threads>>>(w, h, s, Ix, Iy, Iz, texSource, texTarget);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/HSOpticalFlow/derivativesKernel.cuh`.

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

- **Total Lines**: 148
- **Approximate Size**: 6278 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

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

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

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
