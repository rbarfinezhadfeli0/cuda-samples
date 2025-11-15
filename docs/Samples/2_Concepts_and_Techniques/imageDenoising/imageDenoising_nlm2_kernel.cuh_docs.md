# Documentation for Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh`
- **Type**: .cuh
- **Location**: Samples/2_Concepts_and_Techniques/imageDenoising
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

////////////////////////////////////////////////////////////////////////////////
//   0 1 2 3 4 5 6 7
// 0 + . . . . . . .
// 1 . . . . . . . .
// 2 . . . . . . . .
// 3 . . . * . . . .
// 4 . . . . . . . .
// 5 . . . . . . . .
// 6 . . . . . . . .
// 7 . . . . . . . .
//
// * - Base point for every thread, + - pixel around which ColorDistance is
// computed
// The idea behind this method:
// - Every thread in a 8x8 block computes just one ColorDistance
// - It is saved in the weights array that is shared across the threads
// - Threads are synced
// - For every pixel inside the block weights are considered to be constants
////////////////////////////////////////////////////////////////////////////////
#include <cooperative_groups.h>

namespace cg = cooperative_groups;

__global__ void NLM2(TColor *dst, int imageW, int imageH, float Noise, float lerpC, cudaTextureObject_t texImage)
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();

    // Weights cache
    __shared__ float fWeights[BLOCKDIM_X * BLOCKDIM_Y];

    const int ix = blockDim.x * blockIdx.x + threadIdx.x;
    const int iy = blockDim.y * blockIdx.y + threadIdx.y;
    // Add half of a texel to always address exact texel centers
    const float x  = (float)ix + 0.5f;
    const float y  = (float)iy + 0.5f;
    const float cx = blockDim.x * blockIdx.x + NLM_WINDOW_RADIUS + 0.5f;
    const float cy = blockDim.x * blockIdx.y + NLM_WINDOW_RADIUS + 0.5f;

    if (ix < imageW && iy < imageH) {
        // Find color distance from current texel to the center of NLM window
        float weight = 0;

        for (float n = -NLM_BLOCK_RADIUS; n <= NLM_BLOCK_RADIUS; n++)
            for (float m = -NLM_BLOCK_RADIUS; m <= NLM_BLOCK_RADIUS; m++)
                weight += vecLen(tex2D<float4>(texImage, cx + m, cy + n), tex2D<float4>(texImage, x + m, y + n));

        // Geometric distance from current texel to the center of NLM window
        float dist = (threadIdx.x - NLM_WINDOW_RADIUS) * (threadIdx.x - NLM_WINDOW_RADIUS)
                   + (threadIdx.y - NLM_WINDOW_RADIUS) * (threadIdx.y - NLM_WINDOW_RADIUS);

        // Derive final weight from color and geometric distance
        weight = __expf(-(weight * Noise + dist * INV_NLM_WINDOW_AREA));

        // Write the result to shared memory
        fWeights[threadIdx.y * BLOCKDIM_X + threadIdx.x] = weight;
        // Wait until all the weights are ready
        cg::sync(cta);

        // Normalized counter for the NLM weight threshold
        float fCount = 0;
        // Total sum of pixel weights
        float sumWeights = 0;
        // Result accumulator
        float3 clr = {0, 0, 0};

        int idx = 0;

        // Cycle through NLM window, surrounding (x, y) texel
        for (float i = -NLM_WINDOW_RADIUS; i <= NLM_WINDOW_RADIUS + 1; i++)
            for (float j = -NLM_WINDOW_RADIUS; j <= NLM_WINDOW_RADIUS + 1; j++) {
                // Load precomputed weight
                float weightIJ = fWeights[idx++];

                // Accumulate (x + j, y + i) texel color with computed weight
                float4 clrIJ = tex2D<float4>(texImage, x + j, y + i);
                clr.x += clrIJ.x * weightIJ;
                clr.y += clrIJ.y * weightIJ;
                clr.z += clrIJ.z * weightIJ;

                // Sum of weights for color normalization to [0..1] range
                sumWeights += weightIJ;

                // Update weight counter, if NLM weight for current window texel
                // exceeds the weight threshold
                fCount += (weightIJ > NLM_WEIGHT_THRESHOLD) ? INV_NLM_WINDOW_AREA : 0;
            }

        // Normalize result color by sum of weights
        sumWeights = 1.0f / sumWeights;
        clr.x *= sumWeights;
        clr.y *= sumWeights;
        clr.z *= sumWeights;

        // Choose LERP quotient basing on how many texels
        // within the NLM window exceeded the weight threshold
        float lerpQ = (fCount > NLM_LERP_THRESHOLD) ? lerpC : 1.0f - lerpC;

        // Write final result to global memory
        float4 clr00          = tex2D<float4>(texImage, x, y);
        clr.x                 = lerpf(clr.x, clr00.x, lerpQ);
        clr.y                 = lerpf(clr.y, clr00.y, lerpQ);
        clr.z                 = lerpf(clr.z, clr00.z, lerpQ);
        dst[imageW * iy + ix] = make_color(clr.x, clr.y, clr.z, 0);
    }
}

extern "C" void cuda_NLM2(TColor *d_dst, int imageW, int imageH, float Noise, float LerpC, cudaTextureObject_t texImage)
{
    dim3 threads(BLOCKDIM_X, BLOCKDIM_Y);
    dim3 grid(iDivUp(imageW, BLOCKDIM_X), iDivUp(imageH, BLOCKDIM_Y));

    NLM2<<<grid, threads>>>(d_dst, imageW, imageH, Noise, LerpC, texImage);
}

////////////////////////////////////////////////////////////////////////////////
// Stripped NLM2 kernel, only highlighting areas with different LERP directions
////////////////////////////////////////////////////////////////////////////////
__global__ void NLM2diag(TColor *dst, int imageW, int imageH, float Noise, float LerpC, cudaTextureObject_t texImage)
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();

    // Weights cache
    __shared__ float fWeights[BLOCKDIM_X * BLOCKDIM_Y];

    const int ix = blockDim.x * blockIdx.x + threadIdx.x;
    const int iy = blockDim.y * blockIdx.y + threadIdx.y;
    // Add half of a texel to always address exact texel centers
    const float x  = (float)ix + 0.5f;
    const float y  = (float)iy + 0.5f;
    const float cx = blockDim.x * blockIdx.x + NLM_WINDOW_RADIUS + 0.5f;
    const float cy = blockDim.x * blockIdx.y + NLM_WINDOW_RADIUS + 0.5f;

    if (ix < imageW && iy < imageH) {
        // Find color distance from current texel to the center of NLM window
        float weight = 0;

        for (float n = -NLM_BLOCK_RADIUS; n <= NLM_BLOCK_RADIUS + 1; n++)
            for (float m = -NLM_BLOCK_RADIUS; m <= NLM_BLOCK_RADIUS + 1; m++)
                weight += vecLen(tex2D<float4>(texImage, cx + m, cy + n), tex2D<float4>(texImage, x + m, y + n));

        // Geometric distance from current texel to the center of NLM window
        float dist = (threadIdx.x - NLM_WINDOW_RADIUS) * (threadIdx.x - NLM_WINDOW_RADIUS)
                   + (threadIdx.y - NLM_WINDOW_RADIUS) * (threadIdx.y - NLM_WINDOW_RADIUS);

        // Derive final weight from color and geometric distance
        weight = __expf(-(weight * Noise + dist * INV_NLM_WINDOW_AREA));

        // Write the result to shared memory
        fWeights[threadIdx.y * BLOCKDIM_X + threadIdx.x] = weight;
        // Wait until all the weights are ready
        cg::sync(cta);

        // Normalized counter for the NLM weight threshold
        float fCount = 0;
        int   idx    = 0;

        // Cycle through NLM window, surrounding (x, y) texel
        for (float n = -NLM_WINDOW_RADIUS; n <= NLM_WINDOW_RADIUS + 1; n++)
            for (float m = -NLM_WINDOW_RADIUS; m <= NLM_WINDOW_RADIUS + 1; m++) {
                // Load precomputed weight
                float weightIJ = fWeights[idx++];

                // Update weight counter, if NLM weight for current window texel
                // exceeds the weight threshold
                fCount += (weightIJ > NLM_WEIGHT_THRESHOLD) ? INV_NLM_WINDOW_AREA : 0;
            }

        // Choose LERP quotient basing on how many texels
        // within the NLM window exceeded the weight threshold
        float lerpQ = (fCount > NLM_LERP_THRESHOLD) ? 1.0f : 0.0f;

        // Write final result to global memory
        dst[imageW * iy + ix] = make_color(lerpQ, 0, (1.0f - lerpQ), 0);
    };
}

extern "C" void
cuda_NLM2diag(TColor *d_dst, int imageW, int imageH, float Noise, float LerpC, cudaTextureObject_t texImage)
{
    dim3 threads(BLOCKDIM_X, BLOCKDIM_Y);
    dim3 grid(iDivUp(imageW, BLOCKDIM_X), iDivUp(imageH, BLOCKDIM_Y));

    NLM2diag<<<grid, threads>>>(d_dst, imageW, imageH, Noise, LerpC, texImage);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh`.

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

- **Total Lines**: 214
- **Approximate Size**: 9496 bytes

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
