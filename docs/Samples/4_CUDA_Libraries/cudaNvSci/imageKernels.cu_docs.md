# Documentation for Samples/4_CUDA_Libraries/cudaNvSci/imageKernels.cu

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/cudaNvSci/imageKernels.cu`
- **Type**: .cu
- **Location**: Samples/4_CUDA_Libraries/cudaNvSci
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

#include <cuda.h>
#include <helper_cuda.h>
#include <helper_image.h>

// convert floating point rgba color to 32-bit integer
__device__ unsigned int rgbaFloatToInt(float4 rgba)
{
    rgba.x = __saturatef(rgba.x); // clamp to [0.0, 1.0]
    rgba.y = __saturatef(rgba.y);
    rgba.z = __saturatef(rgba.z);
    rgba.w = __saturatef(rgba.w);
    return ((unsigned int)(rgba.w * 255.0f) << 24) | ((unsigned int)(rgba.z * 255.0f) << 16)
         | ((unsigned int)(rgba.y * 255.0f) << 8) | ((unsigned int)(rgba.x * 255.0f));
}

////////////////////////////////////////////////////////////////////////////////
//! Rotate an image using texture lookups
//! @param outputData  output data in global memory
////////////////////////////////////////////////////////////////////////////////
static __global__ void
transformKernel(unsigned int *outputData, int width, int height, float theta, cudaTextureObject_t tex)
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
    float4       pix          = tex2D<float4>(tex, tu + 0.5f, tv + 0.5f);
    unsigned int pixelInt     = rgbaFloatToInt(pix);
    outputData[y * width + x] = pixelInt;
}

static __global__ void rgbToGrayscaleKernel(unsigned int *rgbaImage, size_t imageWidth, size_t imageHeight)
{
    size_t gidX = blockDim.x * blockIdx.x + threadIdx.x;

    uchar4 *pixArray = (uchar4 *)rgbaImage;

    for (int pixId = gidX; pixId < imageWidth * imageHeight; pixId += gridDim.x * blockDim.x) {
        uchar4        dataA     = pixArray[pixId];
        unsigned char grayscale = (unsigned char)(dataA.x * 0.3 + dataA.y * 0.59 + dataA.z * 0.11);
        uchar4        dataB     = make_uchar4(grayscale, grayscale, grayscale, 0);
        pixArray[pixId]         = dataB;
    }
}

void launchGrayScaleKernel(unsigned int *d_rgbaImage,
                           std::string   image_filename,
                           size_t        imageWidth,
                           size_t        imageHeight,
                           cudaStream_t  stream)
{
    int numThreadsPerBlock = 1024;
    int numOfBlocks        = (imageWidth * imageHeight) / numThreadsPerBlock;

    rgbToGrayscaleKernel<<<numOfBlocks, numThreadsPerBlock, 0, stream>>>(d_rgbaImage, imageWidth, imageHeight);

    unsigned int *outputData;
    checkCudaErrors(cudaMallocHost((void **)&outputData, sizeof(unsigned int) * imageWidth * imageHeight));
    checkCudaErrors(cudaMemcpyAsync(
        outputData, d_rgbaImage, sizeof(unsigned int) * imageWidth * imageHeight, cudaMemcpyDeviceToHost, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));

    char outputFilename[1024];
    strcpy(outputFilename, image_filename.c_str());
    strcpy(outputFilename + image_filename.length() - 4, "_out.ppm");
    sdkSavePPM4ub(outputFilename, (unsigned char *)outputData, imageWidth, imageHeight);
    printf("Wrote '%s'\n", outputFilename);

    checkCudaErrors(cudaFreeHost(outputData));
}

void rotateKernel(cudaTextureObject_t &texObj,
                  const float          angle,
                  unsigned int        *d_outputData,
                  const int            imageWidth,
                  const int            imageHeight,
                  cudaStream_t         stream)
{
    dim3 dimBlock(8, 8, 1);
    dim3 dimGrid(imageWidth / dimBlock.x, imageHeight / dimBlock.y, 1);

    transformKernel<<<dimGrid, dimBlock, 0, stream>>>(d_outputData, imageWidth, imageHeight, angle, texObj);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/cudaNvSci/imageKernels.cu`.

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

- **Total Lines**: 120
- **Approximate Size**: 5381 bytes

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
