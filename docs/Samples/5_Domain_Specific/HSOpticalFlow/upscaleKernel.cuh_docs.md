# Documentation: Samples/5_Domain_Specific/HSOpticalFlow/upscaleKernel.cuh
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/HSOpticalFlow/upscaleKernel.cuh`
- **Filename**: `upscaleKernel.cuh`
- **Language**: cuda
- **Size**: 4717 bytes
- **Lines**: 103
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```cuda
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
/// \brief upscale one component of a displacement field, CUDA kernel
/// \param[in]  width   field width
/// \param[in]  height  field height
/// \param[in]  stride  field stride
/// \param[in]  scale   scale factor (multiplier)
/// \param[out] out     result
///////////////////////////////////////////////////////////////////////////////
__global__ void UpscaleKernel(int width, int height, int stride, float scale, float *out, cudaTextureObject_t texCoarse)
{
    const int ix = threadIdx.x + blockIdx.x * blockDim.x;
    const int iy = threadIdx.y + blockIdx.y * blockDim.y;

    if (ix >= width || iy >= height)
        return;

    float x = ((float)ix + 0.5f) / (float)width;
    float y = ((float)iy + 0.5f) / (float)height;

    // exploit hardware interpolation
    // and scale interpolated vector to match next pyramid level resolution
    out[ix + iy * stride] = tex2D<float>(texCoarse, x, y) * scale;
}

///////////////////////////////////////////////////////////////////////////////
/// \brief upscale one component of a displacement field, kernel wrapper
/// \param[in]  src         field component to upscale
/// \param[in]  width       field current width
/// \param[in]  height      field current height
/// \param[in]  stride      field current stride
/// \param[in]  newWidth    field new width
/// \param[in]  newHeight   field new height
/// \param[in]  newStride   field new stride
/// \param[in]  scale       value scale factor (multiplier)
/// \param[out] out         upscaled field component
///////////////////////////////////////////////////////////////////////////////
static void Upscale(const float *src,
                    int          width,
                    int          height,
                    int          stride,
                    int          newWidth,
                    int          newHeight,
                    int          newStride,
                    float        scale,
                    float       *out)
{
    dim3 threads(32, 8);
    dim3 blocks(iDivUp(newWidth, threads.x), iDivUp(newHeight, threads.y));

    cudaTextureObject_t texCoarse;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                  = cudaResourceTypePitch2D;
    texRes.res.pitch2D.devPtr       = (void *)src;
    texRes.res.pitch2D.desc         = cudaCreateChannelDesc<float>();
    texRes.res.pitch2D.width        = width;
    texRes.res.pitch2D.height       = height;
    texRes.res.pitch2D.pitchInBytes = stride * sizeof(float);

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = true;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeMirror;
    texDescr.addressMode[1]   = cudaAddressModeMirror;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texCoarse, &texRes, &texDescr, NULL));

    UpscaleKernel<<<blocks, threads>>>(newWidth, newHeight, newStride, scale, out, texCoarse);
}

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `common.h`

### CUDA Kernels
#### `UpscaleKernel`
- **Return Type**: `void`
- **Parameters**: `int width, int height, int stride, float scale, float *out, cudaTextureObject_t texCoarse`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void UpscaleKernel(int width, int height, int stride, float scale, float *out, cudaTextureObject_t texCoarse)`
- Function in Samples/5_Domain_Specific/HSOpticalFlow/upscaleKernel.cuh

#### `void Upscale(const float *src,
                    int          width,
                    int          height,
                    int          stride,
                    int          newWidth,
                    int          newHeight,
                    int          newStride,
                    float        scale,
                    float       *out)`
- Function in Samples/5_Domain_Specific/HSOpticalFlow/upscaleKernel.cuh


---
## Usage Examples
This is a CUDA source file. Typical usage involves:
1. Compiling with nvcc (NVIDIA CUDA Compiler)
2. Linking with CUDA runtime libraries
3. Executing on NVIDIA GPU hardware


---
## Performance & Security Notes
### Performance Considerations
- CUDA kernel execution is asynchronous
- Memory transfers between host and device can be a bottleneck
- Thread block and grid dimensions affect performance

### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

