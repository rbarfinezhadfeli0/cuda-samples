# Documentation: Samples/0_Introduction/simpleTextureDrv/simpleTexture_kernel.cu
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleTextureDrv/simpleTexture_kernel.cu`
- **Filename**: `simpleTexture_kernel.cu`
- **Language**: cuda
- **Size**: 2595 bytes
- **Lines**: 55
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

#ifndef _SIMPLETEXTURE_KERNEL_H_
#define _SIMPLETEXTURE_KERNEL_H_
#include <cuda.h>

////////////////////////////////////////////////////////////////////////////////
//! Transform an image using texture lookups
//! @param g_odata  output data in global memory
////////////////////////////////////////////////////////////////////////////////
extern "C" __global__ void transformKernel(float *g_odata, int width, int height, float theta, CUtexObject tex)
{
    // calculate normalized texture coordinates
    unsigned int x = blockIdx.x * blockDim.x + threadIdx.x;
    unsigned int y = blockIdx.y * blockDim.y + threadIdx.y;

    float u  = (float)x - (float)width / 2;
    float v  = (float)y - (float)height / 2;
    float tu = u * cosf(theta) - v * sinf(theta);
    float tv = v * cosf(theta) + u * sinf(theta);

    tu /= (float)width;
    tv /= (float)height;

    // read from texture and write to global memory
    g_odata[y * width + x] = tex2D<float>(tex, tu + 0.5f, tv + 0.5f);
}

#endif // #ifndef _SIMPLETEXTURE_KERNEL_H_

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda.h`

### Preprocessor Definitions
- **_SIMPLETEXTURE_KERNEL_H_**: `#include <cuda.h>`

### CUDA Kernels
#### `transformKernel`
- **Return Type**: `void`
- **Parameters**: `float *g_odata, int width, int height, float theta, CUtexObject tex`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void transformKernel(float *g_odata, int width, int height, float theta, CUtexObject tex)`
- Function in Samples/0_Introduction/simpleTextureDrv/simpleTexture_kernel.cu


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

