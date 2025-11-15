# Documentation: Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu`
- **Filename**: `simpleTexture3D_kernel.cu`
- **Language**: cuda
- **Size**: 5072 bytes
- **Lines**: 139
- **Generated**: 2025-11-15 12:53:53 UTC

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

#ifndef _SIMPLETEXTURE3D_KERNEL_CU_
#define _SIMPLETEXTURE3D_KERNEL_CU_

#include <helper_cuda.h>
#include <helper_math.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef unsigned int  uint;
typedef unsigned char uchar;

cudaArray          *d_volumeArray = 0;
cudaTextureObject_t tex; // 3D texture

__global__ void d_render(uint *d_output, uint imageW, uint imageH, float w, cudaTextureObject_t texObj)
{
    uint x = __umul24(blockIdx.x, blockDim.x) + threadIdx.x;
    uint y = __umul24(blockIdx.y, blockDim.y) + threadIdx.y;

    float u = x / (float)imageW;
    float v = y / (float)imageH;
    // read from 3D texture
    float voxel = tex3D<float>(texObj, u, v, w);

    if ((x < imageW) && (y < imageH)) {
        // write output color
        uint i      = __umul24(y, imageW) + x;
        d_output[i] = voxel * 255;
    }
}

extern "C" void setTextureFilterMode(bool bLinearFilter)
{
    if (tex) {
        checkCudaErrors(cudaDestroyTextureObject(tex));
    }
    cudaResourceDesc texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType         = cudaResourceTypeArray;
    texRes.res.array.array = d_volumeArray;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = true;
    texDescr.filterMode       = bLinearFilter ? cudaFilterModeLinear : cudaFilterModePoint;
    ;
    texDescr.addressMode[0] = cudaAddressModeWrap;
    texDescr.addressMode[1] = cudaAddressModeWrap;
    texDescr.addressMode[2] = cudaAddressModeWrap;
    texDescr.readMode       = cudaReadModeNormalizedFloat;

    checkCudaErrors(cudaCreateTextureObject(&tex, &texRes, &texDescr, NULL));
}

extern "C" void initCuda(const uchar *h_volume, cudaExtent volumeSize)
{
    // create 3D array
    cudaChannelFormatDesc channelDesc = cudaCreateChannelDesc<uchar>();
    checkCudaErrors(cudaMalloc3DArray(&d_volumeArray, &channelDesc, volumeSize));

    // copy data to 3D array
    cudaMemcpy3DParms copyParams = {0};
    copyParams.srcPtr =
        make_cudaPitchedPtr((void *)h_volume, volumeSize.width * sizeof(uchar), volumeSize.width, volumeSize.height);
    copyParams.dstArray = d_volumeArray;
    copyParams.extent   = volumeSize;
    copyParams.kind     = cudaMemcpyHostToDevice;
    checkCudaErrors(cudaMemcpy3D(&copyParams));

    cudaResourceDesc texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType         = cudaResourceTypeArray;
    texRes.res.array.array = d_volumeArray;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    // access with normalized texture coordinates
    texDescr.normalizedCoords = true;
    // linear interpolation
    texDescr.filterMode = cudaFilterModeLinear;
    // wrap texture coordinates
    texDescr.addressMode[0] = cudaAddressModeWrap;
    texDescr.addressMode[1] = cudaAddressModeWrap;
    texDescr.addressMode[2] = cudaAddressModeWrap;
    texDescr.readMode       = cudaReadModeNormalizedFloat;

    checkCudaErrors(cudaCreateTextureObject(&tex, &texRes, &texDescr, NULL));
}

extern "C" void render_kernel(dim3 gridSize, dim3 blockSize, uint *d_output, uint imageW, uint imageH, float w)
{
    d_render<<<gridSize, blockSize>>>(d_output, imageW, imageH, w, tex);
}

void cleanupCuda()
{
    if (tex) {
        checkCudaErrors(cudaDestroyTextureObject(tex));
    }
    if (d_volumeArray) {
        checkCudaErrors(cudaFreeArray(d_volumeArray));
    }
}

#endif // #ifndef _SIMPLETEXTURE3D_KERNEL_CU_

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 5 function(s) in the CUDA Samples repository.

**Dependencies**: 6 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `helper_cuda.h`
- `helper_math.h`
- `math.h`
- `stdio.h`
- `stdlib.h`
- `string.h`

### Preprocessor Definitions
- **_SIMPLETEXTURE3D_KERNEL_CU_**: `#include <helper_cuda.h>`

### CUDA Kernels
#### `d_render`
- **Return Type**: `void`
- **Parameters**: `uint *d_output, uint imageW, uint imageH, float w, cudaTextureObject_t texObj`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void d_render(uint *d_output, uint imageW, uint imageH, float w, cudaTextureObject_t texObj)`
- Function in Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu

#### `void setTextureFilterMode(bool bLinearFilter)`
- Function in Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu

#### `void initCuda(const uchar *h_volume, cudaExtent volumeSize)`
- Function in Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu

#### `void render_kernel(dim3 gridSize, dim3 blockSize, uint *d_output, uint imageW, uint imageH, float w)`
- Function in Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu

#### `void cleanupCuda()`
- Function in Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu


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

