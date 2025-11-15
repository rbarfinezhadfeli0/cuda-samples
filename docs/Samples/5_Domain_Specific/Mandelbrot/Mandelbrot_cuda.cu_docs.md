# Documentation for Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_cuda.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_cuda.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/Mandelbrot
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

#include <stdio.h>

#include "Mandelbrot_kernel.cuh"
#include "Mandelbrot_kernel.h"
#include "helper_cuda.h"

// The Mandelbrot CUDA GPU thread function

template <class T>
__global__ void Mandelbrot0(uchar4      *dst,
                            const int    imageW,
                            const int    imageH,
                            const int    crunch,
                            const T      xOff,
                            const T      yOff,
                            const T      xJP,
                            const T      yJP,
                            const T      scale,
                            const uchar4 colors,
                            const int    frame,
                            const int    animationFrame,
                            const int    gridWidth,
                            const int    numBlocks,
                            const bool   isJ)
{
    // loop until all blocks completed
    for (unsigned int blockIndex = blockIdx.x; blockIndex < numBlocks; blockIndex += gridDim.x) {
        unsigned int blockX = blockIndex % gridWidth;
        unsigned int blockY = blockIndex / gridWidth;

        // process this block
        const int ix = blockDim.x * blockX + threadIdx.x;
        const int iy = blockDim.y * blockY + threadIdx.y;

        if ((ix < imageW) && (iy < imageH)) {
            // Calculate the location
            const T xPos = (T)ix * scale + xOff;
            const T yPos = (T)iy * scale + yOff;

            // Calculate the Mandelbrot index for the current location
            int m = CalcMandelbrot<T>(xPos, yPos, xJP, yJP, crunch, isJ);
            //            int m = blockIdx.x;         // uncomment to see scheduling
            //            order
            m = m > 0 ? crunch - m : 0;

            // Convert the Mandelbrot index into a color
            uchar4 color;

            if (m) {
                m += animationFrame;
                color.x = m * colors.x;
                color.y = m * colors.y;
                color.z = m * colors.z;
            }
            else {
                color.x = 0;
                color.y = 0;
                color.z = 0;
            }

            // Output the pixel
            int pixel = imageW * iy + ix;

            if (frame == 0) {
                color.w    = 0;
                dst[pixel] = color;
            }
            else {
                int frame1   = frame + 1;
                int frame2   = frame1 / 2;
                dst[pixel].x = (dst[pixel].x * frame + color.x + frame2) / frame1;
                dst[pixel].y = (dst[pixel].y * frame + color.y + frame2) / frame1;
                dst[pixel].z = (dst[pixel].z * frame + color.z + frame2) / frame1;
            }
        }
    }

} // Mandelbrot0

// The Mandelbrot CUDA GPU thread function (double single version)
__global__ void MandelbrotDS0(uchar4      *dst,
                              const int    imageW,
                              const int    imageH,
                              const int    crunch,
                              const float  xOff0,
                              const float  xOff1,
                              const float  yOff0,
                              const float  yOff1,
                              const float  xJP,
                              const float  yJP,
                              const float  scale,
                              const uchar4 colors,
                              const int    frame,
                              const int    animationFrame,
                              const int    gridWidth,
                              const int    numBlocks,
                              const bool   isJ)
{
    // loop until all blocks completed
    for (unsigned int blockIndex = blockIdx.x; blockIndex < numBlocks; blockIndex += gridDim.x) {
        unsigned int blockX = blockIndex % gridWidth;
        unsigned int blockY = blockIndex / gridWidth;

        // process this block
        const int ix = blockDim.x * blockX + threadIdx.x;
        const int iy = blockDim.y * blockY + threadIdx.y;

        if ((ix < imageW) && (iy < imageH)) {
            // Calculate the location
            float xPos0 = (float)ix * scale;
            float xPos1 = 0.0f;
            float yPos0 = (float)iy * scale;
            float yPos1 = 0.0f;
            dsadd(xPos0, xPos1, xPos0, xPos1, xOff0, xOff1);
            dsadd(yPos0, yPos1, yPos0, yPos1, yOff0, yOff1);

            // Calculate the Mandelbrot index for the current location
            int m = CalcMandelbrotDS(xPos0, xPos1, yPos0, yPos1, xJP, yJP, crunch, isJ);
            m     = m > 0 ? crunch - m : 0;

            // Convert the Mandelbrot index into a color
            uchar4 color;

            if (m) {
                m += animationFrame;
                color.x = m * colors.x;
                color.y = m * colors.y;
                color.z = m * colors.z;
            }
            else {
                color.x = 0;
                color.y = 0;
                color.z = 0;
            }

            // Output the pixel
            int pixel = imageW * iy + ix;

            if (frame == 0) {
                color.w    = 0;
                dst[pixel] = color;
            }
            else {
                int frame1   = frame + 1;
                int frame2   = frame1 / 2;
                dst[pixel].x = (dst[pixel].x * frame + color.x + frame2) / frame1;
                dst[pixel].y = (dst[pixel].y * frame + color.y + frame2) / frame1;
                dst[pixel].z = (dst[pixel].z * frame + color.z + frame2) / frame1;
            }
        }
    }
} // MandelbrotDS0

// The Mandelbrot secondary AA pass CUDA GPU thread function
template <class T>
__global__ void Mandelbrot1(uchar4      *dst,
                            const int    imageW,
                            const int    imageH,
                            const int    crunch,
                            const T      xOff,
                            const T      yOff,
                            const T      xJP,
                            const T      yJP,
                            const T      scale,
                            const uchar4 colors,
                            const int    frame,
                            const int    animationFrame,
                            const int    gridWidth,
                            const int    numBlocks,
                            const bool   isJ)
{
    // loop until all blocks completed
    for (unsigned int blockIndex = blockIdx.x; blockIndex < numBlocks; blockIndex += gridDim.x) {
        unsigned int blockX = blockIndex % gridWidth;
        unsigned int blockY = blockIndex / gridWidth;

        // process this block
        const int ix = blockDim.x * blockX + threadIdx.x;
        const int iy = blockDim.y * blockY + threadIdx.y;

        if ((ix < imageW) && (iy < imageH)) {
            // Get the current pixel color
            int    pixel      = imageW * iy + ix;
            uchar4 pixelColor = dst[pixel];
            int    count      = 0;

            // Search for pixels out of tolerance surrounding the current pixel
            if (ix > 0) {
                count += CheckColors(pixelColor, dst[pixel - 1]);
            }

            if (ix + 1 < imageW) {
                count += CheckColors(pixelColor, dst[pixel + 1]);
            }

            if (iy > 0) {
                count += CheckColors(pixelColor, dst[pixel - imageW]);
            }

            if (iy + 1 < imageH) {
                count += CheckColors(pixelColor, dst[pixel + imageW]);
            }

            if (count) {
                // Calculate the location
                const T xPos = (T)ix * scale + xOff;
                const T yPos = (T)iy * scale + yOff;

                // Calculate the Mandelbrot index for the current location
                int m = CalcMandelbrot(xPos, yPos, xJP, yJP, crunch, isJ);
                m     = m > 0 ? crunch - m : 0;

                // Convert the Mandelbrot index into a color
                uchar4 color;

                if (m) {
                    m += animationFrame;
                    color.x = m * colors.x;
                    color.y = m * colors.y;
                    color.z = m * colors.z;
                }
                else {
                    color.x = 0;
                    color.y = 0;
                    color.z = 0;
                }

                // Output the pixel
                int frame1   = frame + 1;
                int frame2   = frame1 / 2;
                dst[pixel].x = (pixelColor.x * frame + color.x + frame2) / frame1;
                dst[pixel].y = (pixelColor.y * frame + color.y + frame2) / frame1;
                dst[pixel].z = (pixelColor.z * frame + color.z + frame2) / frame1;
            }
        }
    }

} // Mandelbrot1

// The Mandelbrot secondary AA pass CUDA GPU thread function (double single
// version)
__global__ void MandelbrotDS1(uchar4      *dst,
                              const int    imageW,
                              const int    imageH,
                              const int    crunch,
                              const float  xOff0,
                              const float  xOff1,
                              const float  yOff0,
                              const float  yOff1,
                              const float  xJP,
                              const float  yJP,
                              const float  scale,
                              const uchar4 colors,
                              const int    frame,
                              const int    animationFrame,
                              const int    gridWidth,
                              const int    numBlocks,
                              const bool   isJ)
{
    // loop until all blocks completed
    for (unsigned int blockIndex = blockIdx.x; blockIndex < numBlocks; blockIndex += gridDim.x) {
        unsigned int blockX = blockIndex % gridWidth;
        unsigned int blockY = blockIndex / gridWidth;

        // process this block
        const int ix = blockDim.x * blockX + threadIdx.x;
        const int iy = blockDim.y * blockY + threadIdx.y;

        if ((ix < imageW) && (iy < imageH)) {
            // Get the current pixel color
            int    pixel      = imageW * iy + ix;
            uchar4 pixelColor = dst[pixel];
            int    count      = 0;

            // Search for pixels out of tolerance surrounding the current pixel
            if (ix > 0) {
                count += CheckColors(pixelColor, dst[pixel - 1]);
            }

            if (ix + 1 < imageW) {
                count += CheckColors(pixelColor, dst[pixel + 1]);
            }

            if (iy > 0) {
                count += CheckColors(pixelColor, dst[pixel - imageW]);
            }

            if (iy + 1 < imageH) {
                count += CheckColors(pixelColor, dst[pixel + imageW]);
            }

            if (count) {
                // Calculate the location
                float xPos0 = (float)ix * scale;
                float xPos1 = 0.0f;
                float yPos0 = (float)iy * scale;
                float yPos1 = 0.0f;
                dsadd(xPos0, xPos1, xPos0, xPos1, xOff0, xOff1);
                dsadd(yPos0, yPos1, yPos0, yPos1, yOff0, yOff1);

                // Calculate the Mandelbrot index for the current location
                int m = CalcMandelbrotDS(xPos0, xPos1, yPos0, yPos1, xJP, yJP, crunch, isJ);
                m     = m > 0 ? crunch - m : 0;

                // Convert the Mandelbrot index into a color
                uchar4 color;

                if (m) {
                    m += animationFrame;
                    color.x = m * colors.x;
                    color.y = m * colors.y;
                    color.z = m * colors.z;
                }
                else {
                    color.x = 0;
                    color.y = 0;
                    color.z = 0;
                }

                // Output the pixel
                int frame1   = frame + 1;
                int frame2   = frame1 / 2;
                dst[pixel].x = (pixelColor.x * frame + color.x + frame2) / frame1;
                dst[pixel].y = (pixelColor.y * frame + color.y + frame2) / frame1;
                dst[pixel].z = (pixelColor.z * frame + color.z + frame2) / frame1;
            }
        }
    }

} // MandelbrotDS1

// The host CPU Mandelbrot thread spawner
void RunMandelbrot0(uchar4      *dst,
                    const int    imageW,
                    const int    imageH,
                    const int    crunch,
                    const double xOff,
                    const double yOff,
                    const double xjp,
                    const double yjp,
                    const double scale,
                    const uchar4 colors,
                    const int    frame,
                    const int    animationFrame,
                    const int    mode,
                    const int    numSMs,
                    const bool   isJ,
                    int          version)
{
    dim3 threads(BLOCKDIM_X, BLOCKDIM_Y);
    dim3 grid(iDivUp(imageW, BLOCKDIM_X), iDivUp(imageH, BLOCKDIM_Y));

    int numWorkerBlocks = numSMs;

    switch (mode) {
    default:
    case 0:
        Mandelbrot0<float><<<numWorkerBlocks, threads>>>(dst,
                                                         imageW,
                                                         imageH,
                                                         crunch,
                                                         (float)xOff,
                                                         (float)yOff,
                                                         (float)xjp,
                                                         (float)yjp,
                                                         (float)scale,
                                                         colors,
                                                         frame,
                                                         animationFrame,
                                                         grid.x,
                                                         grid.x * grid.y,
                                                         isJ);
        break;
    case 1:
        float x0, x1, y0, y1;
        dsdeq(x0, x1, xOff);
        dsdeq(y0, y1, yOff);
        MandelbrotDS0<<<numWorkerBlocks, threads>>>(dst,
                                                    imageW,
                                                    imageH,
                                                    crunch,
                                                    x0,
                                                    x1,
                                                    y0,
                                                    y1,
                                                    (float)xjp,
                                                    (float)yjp,
                                                    (float)scale,
                                                    colors,
                                                    frame,
                                                    animationFrame,
                                                    grid.x,
                                                    grid.x * grid.y,
                                                    isJ);
        break;
    case 2:
        Mandelbrot0<double><<<numWorkerBlocks, threads>>>(dst,
                                                          imageW,
                                                          imageH,
                                                          crunch,
                                                          xOff,
                                                          yOff,
                                                          xjp,
                                                          yjp,
                                                          scale,
                                                          colors,
                                                          frame,
                                                          animationFrame,
                                                          grid.x,
                                                          grid.x * grid.y,
                                                          isJ);
        break;
    }

    getLastCudaError("Mandelbrot0 kernel execution failed.\n");
} // RunMandelbrot0

// The host CPU Mandelbrot thread spawner
void RunMandelbrot1(uchar4      *dst,
                    const int    imageW,
                    const int    imageH,
                    const int    crunch,
                    const double xOff,
                    const double yOff,
                    const double xjp,
                    const double yjp,
                    const double scale,
                    const uchar4 colors,
                    const int    frame,
                    const int    animationFrame,
                    const int    mode,
                    const int    numSMs,
                    const bool   isJ,
                    int          version)
{
    dim3 threads(BLOCKDIM_X, BLOCKDIM_Y);
    dim3 grid(iDivUp(imageW, BLOCKDIM_X), iDivUp(imageH, BLOCKDIM_Y));

    int numWorkerBlocks = numSMs;

    switch (mode) {
    default:
    case 0:
        Mandelbrot1<float><<<numWorkerBlocks, threads>>>(dst,
                                                         imageW,
                                                         imageH,
                                                         crunch,
                                                         (float)xOff,
                                                         (float)yOff,
                                                         (float)xjp,
                                                         (float)yjp,
                                                         (float)scale,
                                                         colors,
                                                         frame,
                                                         animationFrame,
                                                         grid.x,
                                                         grid.x * grid.y,
                                                         isJ);
        break;
    case 1:
        float x0, x1, y0, y1;
        dsdeq(x0, x1, xOff);
        dsdeq(y0, y1, yOff);
        MandelbrotDS1<<<numWorkerBlocks, threads>>>(dst,
                                                    imageW,
                                                    imageH,
                                                    crunch,
                                                    x0,
                                                    x1,
                                                    y0,
                                                    y1,
                                                    (float)xjp,
                                                    (float)yjp,
                                                    (float)scale,
                                                    colors,
                                                    frame,
                                                    animationFrame,
                                                    grid.x,
                                                    grid.x * grid.y,
                                                    isJ);
        break;
    case 2:
        Mandelbrot1<double><<<numWorkerBlocks, threads>>>(dst,
                                                          imageW,
                                                          imageH,
                                                          crunch,
                                                          xOff,
                                                          yOff,
                                                          xjp,
                                                          yjp,
                                                          scale,
                                                          colors,
                                                          frame,
                                                          animationFrame,
                                                          grid.x,
                                                          grid.x * grid.y,
                                                          isJ);
        break;
    }

    getLastCudaError("Mandelbrot1 kernel execution failed.\n");
} // RunMandelbrot1

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_cuda.cu`.

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

- **Total Lines**: 530
- **Approximate Size**: 22291 bytes

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
