# Documentation: Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu`
- **Filename**: `convolutionSeparable.cu`
- **Language**: cuda
- **Size**: 7564 bytes
- **Lines**: 193
- **Generated**: 2025-11-15 12:53:54 UTC

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

#include <assert.h>
#include <cooperative_groups.h>
#include <helper_cuda.h>

namespace cg = cooperative_groups;
#include "convolutionSeparable_common.h"

////////////////////////////////////////////////////////////////////////////////
// Convolution kernel storage
////////////////////////////////////////////////////////////////////////////////
__constant__ float c_Kernel[KERNEL_LENGTH];

extern "C" void setConvolutionKernel(float *h_Kernel)
{
    cudaMemcpyToSymbol(c_Kernel, h_Kernel, KERNEL_LENGTH * sizeof(float));
}

////////////////////////////////////////////////////////////////////////////////
// Row convolution filter
////////////////////////////////////////////////////////////////////////////////
#define ROWS_BLOCKDIM_X   16
#define ROWS_BLOCKDIM_Y   4
#define ROWS_RESULT_STEPS 8
#define ROWS_HALO_STEPS   1

__global__ void convolutionRowsKernel(float *d_Dst, float *d_Src, int imageW, int imageH, int pitch)
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();
    __shared__ float s_Data[ROWS_BLOCKDIM_Y][(ROWS_RESULT_STEPS + 2 * ROWS_HALO_STEPS) * ROWS_BLOCKDIM_X];

    // Offset to the left halo edge
    const int baseX = (blockIdx.x * ROWS_RESULT_STEPS - ROWS_HALO_STEPS) * ROWS_BLOCKDIM_X + threadIdx.x;
    const int baseY = blockIdx.y * ROWS_BLOCKDIM_Y + threadIdx.y;

    d_Src += baseY * pitch + baseX;
    d_Dst += baseY * pitch + baseX;

// Load main data
#pragma unroll

    for (int i = ROWS_HALO_STEPS; i < ROWS_HALO_STEPS + ROWS_RESULT_STEPS; i++) {
        s_Data[threadIdx.y][threadIdx.x + i * ROWS_BLOCKDIM_X] = d_Src[i * ROWS_BLOCKDIM_X];
    }

// Load left halo
#pragma unroll

    for (int i = 0; i < ROWS_HALO_STEPS; i++) {
        s_Data[threadIdx.y][threadIdx.x + i * ROWS_BLOCKDIM_X] =
            (baseX >= -i * ROWS_BLOCKDIM_X) ? d_Src[i * ROWS_BLOCKDIM_X] : 0;
    }

// Load right halo
#pragma unroll

    for (int i = ROWS_HALO_STEPS + ROWS_RESULT_STEPS; i < ROWS_HALO_STEPS + ROWS_RESULT_STEPS + ROWS_HALO_STEPS; i++) {
        s_Data[threadIdx.y][threadIdx.x + i * ROWS_BLOCKDIM_X] =
            (imageW - baseX > i * ROWS_BLOCKDIM_X) ? d_Src[i * ROWS_BLOCKDIM_X] : 0;
    }

    // Compute and store results
    cg::sync(cta);
#pragma unroll

    for (int i = ROWS_HALO_STEPS; i < ROWS_HALO_STEPS + ROWS_RESULT_STEPS; i++) {
        float sum = 0;

#pragma unroll

        for (int j = -KERNEL_RADIUS; j <= KERNEL_RADIUS; j++) {
            sum += c_Kernel[KERNEL_RADIUS - j] * s_Data[threadIdx.y][threadIdx.x + i * ROWS_BLOCKDIM_X + j];
        }

        d_Dst[i * ROWS_BLOCKDIM_X] = sum;
    }
}

extern "C" void convolutionRowsGPU(float *d_Dst, float *d_Src, int imageW, int imageH)
{
    assert(ROWS_BLOCKDIM_X * ROWS_HALO_STEPS >= KERNEL_RADIUS);
    assert(imageW % (ROWS_RESULT_STEPS * ROWS_BLOCKDIM_X) == 0);
    assert(imageH % ROWS_BLOCKDIM_Y == 0);

    dim3 blocks(imageW / (ROWS_RESULT_STEPS * ROWS_BLOCKDIM_X), imageH / ROWS_BLOCKDIM_Y);
    dim3 threads(ROWS_BLOCKDIM_X, ROWS_BLOCKDIM_Y);

    convolutionRowsKernel<<<blocks, threads>>>(d_Dst, d_Src, imageW, imageH, imageW);
    getLastCudaError("convolutionRowsKernel() execution failed\n");
}

////////////////////////////////////////////////////////////////////////////////
// Column convolution filter
////////////////////////////////////////////////////////////////////////////////
#define COLUMNS_BLOCKDIM_X   16
#define COLUMNS_BLOCKDIM_Y   8
#define COLUMNS_RESULT_STEPS 8
#define COLUMNS_HALO_STEPS   1

__global__ void convolutionColumnsKernel(float *d_Dst, float *d_Src, int imageW, int imageH, int pitch)
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();
    __shared__ float s_Data[COLUMNS_BLOCKDIM_X]
                           [(COLUMNS_RESULT_STEPS + 2 * COLUMNS_HALO_STEPS) * COLUMNS_BLOCKDIM_Y + 1];

    // Offset to the upper halo edge
    const int baseX = blockIdx.x * COLUMNS_BLOCKDIM_X + threadIdx.x;
    const int baseY = (blockIdx.y * COLUMNS_RESULT_STEPS - COLUMNS_HALO_STEPS) * COLUMNS_BLOCKDIM_Y + threadIdx.y;
    d_Src += baseY * pitch + baseX;
    d_Dst += baseY * pitch + baseX;

// Main data
#pragma unroll

    for (int i = COLUMNS_HALO_STEPS; i < COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS; i++) {
        s_Data[threadIdx.x][threadIdx.y + i * COLUMNS_BLOCKDIM_Y] = d_Src[i * COLUMNS_BLOCKDIM_Y * pitch];
    }

// Upper halo
#pragma unroll

    for (int i = 0; i < COLUMNS_HALO_STEPS; i++) {
        s_Data[threadIdx.x][threadIdx.y + i * COLUMNS_BLOCKDIM_Y] =
            (baseY >= -i * COLUMNS_BLOCKDIM_Y) ? d_Src[i * COLUMNS_BLOCKDIM_Y * pitch] : 0;
    }

// Lower halo
#pragma unroll

    for (int i = COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS;
         i < COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS + COLUMNS_HALO_STEPS;
         i++) {
        s_Data[threadIdx.x][threadIdx.y + i * COLUMNS_BLOCKDIM_Y] =
            (imageH - baseY > i * COLUMNS_BLOCKDIM_Y) ? d_Src[i * COLUMNS_BLOCKDIM_Y * pitch] : 0;
    }

    // Compute and store results
    cg::sync(cta);
#pragma unroll

    for (int i = COLUMNS_HALO_STEPS; i < COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS; i++) {
        float sum = 0;
#pragma unroll

        for (int j = -KERNEL_RADIUS; j <= KERNEL_RADIUS; j++) {
            sum += c_Kernel[KERNEL_RADIUS - j] * s_Data[threadIdx.x][threadIdx.y + i * COLUMNS_BLOCKDIM_Y + j];
        }

        d_Dst[i * COLUMNS_BLOCKDIM_Y * pitch] = sum;
    }
}

extern "C" void convolutionColumnsGPU(float *d_Dst, float *d_Src, int imageW, int imageH)
{
    assert(COLUMNS_BLOCKDIM_Y * COLUMNS_HALO_STEPS >= KERNEL_RADIUS);
    assert(imageW % COLUMNS_BLOCKDIM_X == 0);
    assert(imageH % (COLUMNS_RESULT_STEPS * COLUMNS_BLOCKDIM_Y) == 0);

    dim3 blocks(imageW / COLUMNS_BLOCKDIM_X, imageH / (COLUMNS_RESULT_STEPS * COLUMNS_BLOCKDIM_Y));
    dim3 threads(COLUMNS_BLOCKDIM_X, COLUMNS_BLOCKDIM_Y);

    convolutionColumnsKernel<<<blocks, threads>>>(d_Dst, d_Src, imageW, imageH, imageW);
    getLastCudaError("convolutionColumnsKernel() execution failed\n");
}

```

---
## High-Level Overview
This file is a cuda source file containing 2 CUDA kernel(s) with 15 function(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `assert.h`
- `cooperative_groups.h`
- `helper_cuda.h`
- `convolutionSeparable_common.h`

### Preprocessor Definitions
- **ROWS_BLOCKDIM_X**: `16`
- **ROWS_BLOCKDIM_Y**: `4`
- **ROWS_RESULT_STEPS**: `8`
- **ROWS_HALO_STEPS**: `1`
- **COLUMNS_BLOCKDIM_X**: `16`
- **COLUMNS_BLOCKDIM_Y**: `8`
- **COLUMNS_RESULT_STEPS**: `8`
- **COLUMNS_HALO_STEPS**: `1`

### CUDA Kernels
#### `convolutionRowsKernel`
- **Return Type**: `void`
- **Parameters**: `float *d_Dst, float *d_Src, int imageW, int imageH, int pitch`
- **Description**: CUDA kernel function for GPU execution

#### `convolutionColumnsKernel`
- **Return Type**: `void`
- **Parameters**: `float *d_Dst, float *d_Src, int imageW, int imageH, int pitch`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void setConvolutionKernel(float *h_Kernel)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `void convolutionRowsKernel(float *d_Dst, float *d_Src, int imageW, int imageH, int pitch)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = ROWS_HALO_STEPS; i < ROWS_HALO_STEPS + ROWS_RESULT_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = 0; i < ROWS_HALO_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = ROWS_HALO_STEPS + ROWS_RESULT_STEPS; i < ROWS_HALO_STEPS + ROWS_RESULT_STEPS + ROWS_HALO_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = ROWS_HALO_STEPS; i < ROWS_HALO_STEPS + ROWS_RESULT_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int j = -KERNEL_RADIUS; j <= KERNEL_RADIUS; j++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `void convolutionRowsGPU(float *d_Dst, float *d_Src, int imageW, int imageH)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `void convolutionColumnsKernel(float *d_Dst, float *d_Src, int imageW, int imageH, int pitch)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = COLUMNS_HALO_STEPS; i < COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = 0; i < COLUMNS_HALO_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS;
         i < COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS + COLUMNS_HALO_STEPS;
         i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int i = COLUMNS_HALO_STEPS; i < COLUMNS_HALO_STEPS + COLUMNS_RESULT_STEPS; i++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `unroll for(int j = -KERNEL_RADIUS; j <= KERNEL_RADIUS; j++)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu

#### `void convolutionColumnsGPU(float *d_Dst, float *d_Src, int imageW, int imageH)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu


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

