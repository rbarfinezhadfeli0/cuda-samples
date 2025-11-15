# Documentation: Samples/5_Domain_Specific/simpleD3D11Texture/texture_2d.cu
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/simpleD3D11Texture/texture_2d.cu`
- **Filename**: `texture_2d.cu`
- **Language**: cuda
- **Size**: 3419 bytes
- **Lines**: 79
- **Generated**: 2025-11-15 12:53:51 UTC

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

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define PI 3.1415926536f

/*
 * Paint a 2D texture with a moving red/green hatch pattern on a
 * strobing blue background.  Note that this kernel reads to and
 * writes from the texture, hence why this texture was not mapped
 * as WriteDiscard.
 */
__global__ void cuda_kernel_texture_2d(unsigned char *surface, int width, int height, size_t pitch, float t)
{
    int    x = blockIdx.x * blockDim.x + threadIdx.x;
    int    y = blockIdx.y * blockDim.y + threadIdx.y;
    float *pixel;

    // in the case where, due to quantization into grids, we have
    // more threads than pixels, skip the threads which don't
    // correspond to valid pixels
    if (x >= width || y >= height)
        return;

    // get a pointer to the pixel at (x,y)
    pixel = (float *)(surface + y * pitch) + 4 * x;

    // populate it
    float value_x = 0.5f + 0.5f * cos(t + 10.0f * ((2.0f * x) / width - 1.0f));
    float value_y = 0.5f + 0.5f * cos(t + 10.0f * ((2.0f * y) / height - 1.0f));
    pixel[0]      = 0.5 * pixel[0] + 0.5 * pow(value_x, 3.0f); // red
    pixel[1]      = 0.5 * pixel[1] + 0.5 * pow(value_y, 3.0f); // green
    pixel[2]      = 0.5f + 0.5f * cos(t);                      // blue
    pixel[3]      = 1;                                         // alpha
}

extern "C" void cuda_texture_2d(void *surface, int width, int height, size_t pitch, float t)
{
    cudaError_t error = cudaSuccess;

    dim3 Db = dim3(16, 16); // block dimensions are fixed to be 256 threads
    dim3 Dg = dim3((width + Db.x - 1) / Db.x, (height + Db.y - 1) / Db.y);

    cuda_kernel_texture_2d<<<Dg, Db>>>((unsigned char *)surface, width, height, pitch, t);

    error = cudaGetLastError();

    if (error != cudaSuccess) {
        printf("cuda_kernel_texture_2d() failed to launch error = %d\n", error);
    }
}

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 3 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `stdio.h`
- `stdlib.h`
- `string.h`

### Preprocessor Definitions
- **PI**: `3.1415926536f`

### CUDA Kernels
#### `cuda_kernel_texture_2d`
- **Return Type**: `void`
- **Parameters**: `unsigned char *surface, int width, int height, size_t pitch, float t`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void cuda_kernel_texture_2d(unsigned char *surface, int width, int height, size_t pitch, float t)`
- Function in Samples/5_Domain_Specific/simpleD3D11Texture/texture_2d.cu

#### `void cuda_texture_2d(void *surface, int width, int height, size_t pitch, float t)`
- Function in Samples/5_Domain_Specific/simpleD3D11Texture/texture_2d.cu


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

