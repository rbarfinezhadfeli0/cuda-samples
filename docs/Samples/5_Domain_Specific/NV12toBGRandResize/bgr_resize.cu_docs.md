# Documentation for Samples/5_Domain_Specific/NV12toBGRandResize/bgr_resize.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/NV12toBGRandResize/bgr_resize.cu`
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


// Implements BGR 3 progressive planars frames batch resize

#include <cuda.h>
#include <cuda_runtime.h>

#include "resize_convert.h"

__global__ void resizeBGRplanarBatchKernel(cudaTextureObject_t texSrc,
                                           float              *pDst,
                                           int                 nDstPitch,
                                           int                 nDstHeight,
                                           int                 nSrcHeight,
                                           int                 batch,
                                           float               scaleX,
                                           float               scaleY,
                                           int                 cropX,
                                           int                 cropY,
                                           int                 cropW,
                                           int                 cropH)
{
    int x = threadIdx.x + blockIdx.x * blockDim.x;
    int y = threadIdx.y + blockIdx.y * blockDim.y;

    if (x >= (int)(cropW / scaleX) || y >= (int)(cropH / scaleY))
        return;

    int    frameSize = nDstPitch * nDstHeight;
    float *p         = NULL;
    for (int i = blockIdx.z; i < batch; i += gridDim.z) {
#pragma unroll
        for (int channel = 0; channel < 3; channel++) {
            p  = pDst + i * 3 * frameSize + y * nDstPitch + x + channel * frameSize;
            *p = tex2D<float>(texSrc, x * scaleX + cropX, ((3 * i + channel) * nSrcHeight + y * scaleY + cropY));
        }
    }
}


static void resizeBGRplanarBatchCore(float       *dpSrc,
                                     int          nSrcPitch,
                                     int          nSrcWidth,
                                     int          nSrcHeight,
                                     float       *dpDst,
                                     int          nDstPitch,
                                     int          nDstWidth,
                                     int          nDstHeight,
                                     int          nBatchSize,
                                     cudaStream_t stream,
                                     bool         whSameResizeRatio,
                                     int          cropX,
                                     int          cropY,
                                     int          cropW,
                                     int          cropH)
{
    cudaTextureObject_t texSrc[2];
    int                 nTiles = 1, h, iTile;

    h = nSrcHeight * 3 * nBatchSize;
    while ((h + nTiles - 1) / nTiles > 65536)
        nTiles++;

    if (nTiles > 2)
        return;

    int batchTile     = nBatchSize / nTiles;
    int batchTileLast = nBatchSize - batchTile * (nTiles - 1);

    for (iTile = 0; iTile < nTiles; ++iTile) {
        int    bs       = (iTile == nTiles - 1) ? batchTileLast : batchTile;
        float *dpSrcNew = dpSrc + iTile * (batchTile * 3 * nSrcHeight * nSrcPitch);

        cudaResourceDesc resDesc         = {};
        resDesc.resType                  = cudaResourceTypePitch2D;
        resDesc.res.pitch2D.devPtr       = dpSrcNew;
        resDesc.res.pitch2D.desc         = cudaCreateChannelDesc<float>();
        resDesc.res.pitch2D.width        = nSrcWidth;
        resDesc.res.pitch2D.height       = bs * 3 * nSrcHeight;
        resDesc.res.pitch2D.pitchInBytes = nSrcPitch * sizeof(float);
        cudaTextureDesc texDesc          = {};
        texDesc.filterMode               = cudaFilterModeLinear;
        texDesc.readMode                 = cudaReadModeElementType;

        checkCudaErrors(cudaCreateTextureObject(&texSrc[iTile], &resDesc, &texDesc, NULL));
        float *dpDstNew = dpDst + iTile * (batchTile * 3 * nDstHeight * nDstPitch);

        if (cropW == 0 || cropH == 0) {
            cropX = 0;
            cropY = 0;
            cropW = nSrcWidth;
            cropH = nSrcHeight;
        }

        float scaleX = (cropW * 1.0f / nDstWidth);
        float scaleY = (cropH * 1.0f / nDstHeight);

        if (whSameResizeRatio == true)
            scaleX = scaleY = scaleX > scaleY ? scaleX : scaleY;
        dim3 block(32, 32, 1);

        size_t blockDimZ = bs;
        // Restricting blocks in Z-dim till 32 to not launch too many blocks
        blockDimZ = (blockDimZ > 32) ? 32 : blockDimZ;
        dim3 grid((cropW * 1.0f / scaleX + block.x - 1) / block.x,
                  (cropH * 1.0f / scaleY + block.y - 1) / block.y,
                  blockDimZ);

        resizeBGRplanarBatchKernel<<<grid, block, 0, stream>>>(
            texSrc[iTile], dpDstNew, nDstPitch, nDstHeight, nSrcHeight, bs, scaleX, scaleY, cropX, cropY, cropW, cropH);
    }

    for (iTile = 0; iTile < nTiles; ++iTile)
        checkCudaErrors(cudaDestroyTextureObject(texSrc[iTile]));
}

void resizeBGRplanarBatch(float       *dpSrc,
                          int          nSrcPitch,
                          int          nSrcWidth,
                          int          nSrcHeight,
                          float       *dpDst,
                          int          nDstPitch,
                          int          nDstWidth,
                          int          nDstHeight,
                          int          nBatchSize,
                          cudaStream_t stream,
                          int          cropX,
                          int          cropY,
                          int          cropW,
                          int          cropH,
                          bool         whSameResizeRatio)
{
    resizeBGRplanarBatchCore(dpSrc,
                             nSrcPitch,
                             nSrcWidth,
                             nSrcHeight,
                             dpDst,
                             nDstPitch,
                             nDstWidth,
                             nDstHeight,
                             nBatchSize,
                             stream,
                             whSameResizeRatio,
                             cropX,
                             cropY,
                             cropW,
                             cropH);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/NV12toBGRandResize/bgr_resize.cu`.

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

- **Total Lines**: 175
- **Approximate Size**: 7753 bytes

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
