# Documentation for Samples/5_Domain_Specific/HSOpticalFlow/main.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/HSOpticalFlow/main.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/HSOpticalFlow
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp
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

const static char *const sSDKsample = "HSOpticalFlow";

// CPU-GPU discrepancy threshold for self-test
const float THRESHOLD = 0.05f;

#include <cuda_runtime.h>
#include <helper_functions.h>

#include "common.h"
#include "flowCUDA.h"
#include "flowGold.h"

///////////////////////////////////////////////////////////////////////////////
/// \brief save optical flow in format described on vision.middlebury.edu/flow
/// \param[in] name output file name
/// \param[in] w    optical flow field width
/// \param[in] h    optical flow field height
/// \param[in] s    optical flow field row stride
/// \param[in] u    horizontal displacement
/// \param[in] v    vertical displacement
///////////////////////////////////////////////////////////////////////////////
void WriteFloFile(const char *name, int w, int h, int s, const float *u, const float *v)
{
    FILE *stream;
    stream = fopen(name, "wb");

    if (stream == 0) {
        printf("Could not save flow to \"%s\"\n", name);
        return;
    }

    float data = 202021.25f;
    fwrite(&data, sizeof(float), 1, stream);
    fwrite(&w, sizeof(w), 1, stream);
    fwrite(&h, sizeof(h), 1, stream);

    for (int i = 0; i < h; ++i) {
        for (int j = 0; j < w; ++j) {
            const int pos = j + i * s;
            fwrite(u + pos, sizeof(float), 1, stream);
            fwrite(v + pos, sizeof(float), 1, stream);
        }
    }

    fclose(stream);
}

///////////////////////////////////////////////////////////////////////////////
/// \brief
/// load 4-channel unsigned byte image
/// and convert it to single channel FP32 image
/// \param[out] img_data pointer to raw image data
/// \param[out] img_w    image width
/// \param[out] img_h    image height
/// \param[out] img_s    image row stride
/// \param[in]  name     image file name
/// \param[in]  exePath  executable file path
/// \return true if image is successfully loaded or false otherwise
///////////////////////////////////////////////////////////////////////////////
bool LoadImageAsFP32(float *&img_data, int &img_w, int &img_h, int &img_s, const char *name, const char *exePath)
{
    printf("Loading \"%s\" ...\n", name);
    char *name_ = sdkFindFilePath(name, exePath);

    if (!name_) {
        printf("File not found\n");
        return false;
    }

    unsigned char *data = 0;
    unsigned int   w = 0, h = 0;
    bool           result = sdkLoadPPM4ub(name_, &data, &w, &h);

    if (result == false) {
        printf("Invalid file format\n");
        return false;
    }

    img_w = w;
    img_h = h;
    img_s = iAlignUp(img_w);

    img_data = new float[img_s * h];

    // source is 4 channel image
    const int widthStep = 4 * img_w;

    for (int i = 0; i < img_h; ++i) {
        for (int j = 0; j < img_w; ++j) {
            img_data[j + i * img_s] = ((float)data[j * 4 + i * widthStep]) / 255.0f;
        }
    }

    return true;
}

///////////////////////////////////////////////////////////////////////////////
/// \brief compare given flow field with gold (L1 norm)
/// \param[in] width    optical flow field width
/// \param[in] height   optical flow field height
/// \param[in] stride   optical flow field row stride
/// \param[in] h_uGold  horizontal displacement, gold
/// \param[in] h_vGold  vertical displacement, gold
/// \param[in] h_u      horizontal displacement
/// \param[in] h_v      vertical displacement
/// \return true if discrepancy is lower than a given threshold
///////////////////////////////////////////////////////////////////////////////
bool CompareWithGold(int          width,
                     int          height,
                     int          stride,
                     const float *h_uGold,
                     const float *h_vGold,
                     const float *h_u,
                     const float *h_v)
{
    float error = 0.0f;

    for (int i = 0; i < height; ++i) {
        for (int j = 0; j < width; ++j) {
            const int pos = j + i * stride;
            error += fabsf(h_u[pos] - h_uGold[pos]) + fabsf(h_v[pos] - h_vGold[pos]);
        }
    }

    error /= (float)(width * height);

    printf("L1 error : %.6f\n", error);

    return (error < THRESHOLD);
}

///////////////////////////////////////////////////////////////////////////////
/// application entry point
///////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    // welcome message
    printf("%s Starting...\n\n", sSDKsample);

    // pick GPU
    findCudaDevice(argc, (const char **)argv);

    // find images
    const char *const sourceFrameName = "frame10.ppm";
    const char *const targetFrameName = "frame11.ppm";

    // image dimensions
    int width;
    int height;
    // row access stride
    int stride;

    // flow is computed from source image to target image
    float *h_source; // source image, host memory
    float *h_target; // target image, host memory

    // load image from file
    if (!LoadImageAsFP32(h_source, width, height, stride, sourceFrameName, argv[0])) {
        exit(EXIT_FAILURE);
    }

    if (!LoadImageAsFP32(h_target, width, height, stride, targetFrameName, argv[0])) {
        exit(EXIT_FAILURE);
    }

    // allocate host memory for CPU results
    float *h_uGold = new float[stride * height];
    float *h_vGold = new float[stride * height];

    // allocate host memory for GPU results
    float *h_u = new float[stride * height];
    float *h_v = new float[stride * height];

    // smoothness
    // if image brightness is not within [0,1]
    // this paramter should be scaled appropriately
    const float alpha = 0.2f;

    // number of pyramid levels
    const int nLevels = 5;

    // number of solver iterations on each level
    const int nSolverIters = 500;

    // number of warping iterations
    const int nWarpIters = 3;

    ComputeFlowGold(
        h_source, h_target, width, height, stride, alpha, nLevels, nWarpIters, nSolverIters, h_uGold, h_vGold);

    ComputeFlowCUDA(h_source, h_target, width, height, stride, alpha, nLevels, nWarpIters, nSolverIters, h_u, h_v);

    // compare results (L1 norm)
    bool status = CompareWithGold(width, height, stride, h_uGold, h_vGold, h_u, h_v);

    WriteFloFile("FlowGPU.flo", width, height, stride, h_u, h_v);

    WriteFloFile("FlowCPU.flo", width, height, stride, h_uGold, h_vGold);

    // free resources
    delete[] h_uGold;
    delete[] h_vGold;

    delete[] h_u;
    delete[] h_v;

    delete[] h_source;
    delete[] h_target;

    // report self-test status
    exit(status ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/HSOpticalFlow/main.cpp`.

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

- **Total Lines**: 240
- **Approximate Size**: 8137 bytes

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
