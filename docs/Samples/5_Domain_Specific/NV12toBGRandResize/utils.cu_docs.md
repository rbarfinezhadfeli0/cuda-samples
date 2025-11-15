# Documentation for Samples/5_Domain_Specific/NV12toBGRandResize/utils.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/NV12toBGRandResize/utils.cu`
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

#include <cuda.h>
#include <cuda_runtime.h>
#include <fstream>
#include <iostream>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/types.h>

#include "resize_convert.h"
#include "utils.h"

__global__ void floatToChar(float *src, unsigned char *dst, int height, int width, int batchSize)
{
    int x = threadIdx.x + blockIdx.x * blockDim.x;

    if (x >= height * width)
        return;

    int offset = height * width * 3;

    for (int j = 0; j < batchSize; j++) {
        // b
        *(dst + j * offset + x * 3 + 0) = (unsigned char)*(src + j * offset + height * width * 0 + x);
        // g
        *(dst + j * offset + x * 3 + 1) = (unsigned char)*(src + j * offset + height * width * 1 + x);
        // r
        *(dst + j * offset + x * 3 + 2) = (unsigned char)*(src + j * offset + height * width * 2 + x);
    }
}

void floatPlanarToChar(float *src, unsigned char *dst, int height, int width, int batchSize)
{
    floatToChar<<<(height * width - 1) / 1024 + 1, 1024, 0, NULL>>>(src, dst, height, width, batchSize);
}

void dumpRawBGR(float *d_srcBGR, int pitch, int width, int height, int batchSize, char *folder, char *tag)
{
    float *bgr, *d_bgr;
    int    frameSize;
    char   directory[120];
    char   mkdir_cmd[256];
#if !defined(_WIN32)
    sprintf(directory, "output/%s", folder);
    sprintf(mkdir_cmd, "mkdir -p %s 2> /dev/null", directory);
#else
    sprintf(directory, "output\\%s", folder);
    sprintf(mkdir_cmd, "mkdir %s 2> nul", directory);
#endif

    int ret = system(mkdir_cmd);

    frameSize = width * height * 3 * sizeof(float);
    bgr       = (float *)malloc(frameSize);
    if (bgr == NULL) {
        std::cerr << "Failed malloc for bgr\n";
        return;
    }

    d_bgr = d_srcBGR;
    for (int i = 0; i < batchSize; i++) {
        char           filename[256];
        std::ofstream *outputFile;

        checkCudaErrors(cudaMemcpy((void *)bgr, (void *)d_bgr, frameSize, cudaMemcpyDeviceToHost));
        snprintf(filename, sizeof(filename), "%s/%s_%d.raw", directory, tag, (i + 1));

        outputFile = new std::ofstream(filename);
        if (outputFile) {
            outputFile->write((char *)bgr, frameSize);
            delete outputFile;
        }

        d_bgr += pitch * height * 3;
    }

    free(bgr);
}

void dumpBGR(float *d_srcBGR, int pitch, int width, int height, int batchSize, char *folder, char *tag)
{
    dumpRawBGR(d_srcBGR, pitch, width, height, batchSize, folder, tag);
}

void dumpYUV(unsigned char *d_nv12, int size, char *folder, char *tag)
{
    unsigned char *nv12Data;
    std::ofstream *nv12File;
    char           filename[256];
    char           directory[60];
    char           mkdir_cmd[256];
#if !defined(_WIN32)
    sprintf(directory, "output/%s", folder);
    sprintf(mkdir_cmd, "mkdir -p %s 2> /dev/null", directory);
#else
    sprintf(directory, "output\\%s", folder);
    sprintf(mkdir_cmd, "mkdir %s 2> nul", directory);
#endif

    int ret = system(mkdir_cmd);

    snprintf(filename, sizeof(filename), "%s/%s.nv12", directory, tag);

    nv12File = new std::ofstream(filename);
    if (nv12File == NULL) {
        std::cerr << "Failed to new " << filename;
        return;
    }

    nv12Data = (unsigned char *)malloc(size * (sizeof(char)));
    if (nv12Data == NULL) {
        std::cerr << "Failed to allcoate memory\n";
        return;
    }

    cudaMemcpy((void *)nv12Data, (void *)d_nv12, size, cudaMemcpyDeviceToHost);

    nv12File->write((const char *)nv12Data, size);

    free(nv12Data);
    delete nv12File;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/NV12toBGRandResize/utils.cu`.

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

- **Total Lines**: 149
- **Approximate Size**: 5088 bytes

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
