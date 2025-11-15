# Documentation for Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h`
- **Type**: .h
- **Location**: Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```h
/* Copyright (c) 2024, NVIDIA CORPORATION. All rights reserved.
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
#ifndef CUDA_NVSCIBUF_MULTIPLANAR_H
#define CUDA_NVSCIBUF_MULTIPLANAR_H

#include <cuda.h>
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <nvscibuf.h>
#include <vector>

#define PLANAR_NUM_PLANES          3
#define PLANAR_CHROMA_WIDTH_ORDER  2
#define PLANAR_CHROMA_HEIGHT_ORDER 2

#define ATTR_SIZE   20
#define DEFAULT_GPU 0

#define checkNvSciErrors(call)                                   \
    do {                                                         \
        NvSciError _status = call;                               \
        if (NvSciError_Success != _status) {                     \
            printf("NVSCI call in file '%s' in line %i returned" \
                   " %d, expected %d\n",                         \
                   __FILE__,                                     \
                   __LINE__,                                     \
                   _status,                                      \
                   NvSciError_Success);                          \
            fflush(stdout);                                      \
            exit(EXIT_FAILURE);                                  \
        }                                                        \
    } while (0)

#define checkCudaDrvErrors(call)                           \
    do {                                                   \
        CUresult err = call;                               \
        if (CUDA_SUCCESS != err) {                         \
            const char *errorStr = NULL;                   \
            cuGetErrorString(err, &errorStr);              \
            printf("checkCudaDrvErrors() Driver API error" \
                   " = %04d \"%s\" from file <%s>, "       \
                   "line %i.\n",                           \
                   err,                                    \
                   errorStr,                               \
                   __FILE__,                               \
                   __LINE__);                              \
            exit(EXIT_FAILURE);                            \
        }                                                  \
    } while (0)

extern void launchFlipSurfaceBitsKernel(cudaArray_t *levelArray,
                                        int32_t     *multiPlanarWidth,
                                        int32_t     *multiPlanarHeight,
                                        int          numPlanes);

class Caller
{
private:
    NvSciBufAttrList         attrListOut;
    NvSciBufAttrKeyValuePair pairArrayOut[ATTR_SIZE];
    cudaExternalMemory_t     extMem;
    int32_t                  numPlanes;

public:
    NvSciBufAttrList     attrList;
    cudaMipmappedArray_t multiPlanarArray[PLANAR_NUM_PLANES];
    int32_t              multiPlanarWidth[PLANAR_NUM_PLANES];
    int32_t              multiPlanarHeight[PLANAR_NUM_PLANES];

    void init();
    void deinit();
    void copyExtMemToMultiPlanarArrays();
    void copyYUVToCudaArrayAndFlipBits(std::string &image_filename, cudaArray_t *yuvPlanes);
    void copyCudaArrayToYUV(std::string &image_filename, cudaArray_t *yuvPlanes);
    void setAttrListImageMultiPlanes(int imageWidth, int imageHeight);
};


class cudaNvSciBufMultiplanar
{
private:
    size_t           imageWidth;
    size_t           imageHeight;
    int              mCudaDeviceId;
    int              deviceCnt;
    NvSciBufAttrList attrList[2];
    NvSciBufAttrList attrListReconciled;
    NvSciBufAttrList attrListConflict;

public:
    cudaNvSciBufMultiplanar(size_t imageWidth, size_t imageHeight, std::vector<int> &deviceIds);
    void initCuda(int devId);
    void reconcileAttrList(NvSciBufAttrList *attrList1, NvSciBufAttrList *attrList2);
    void runCudaNvSciBufPlanar(std::string &image_filename, std::string &image_filename_out);
    void tearDown(Caller *caller1, Caller *caller2);
};

enum NvSciBufImageAttributes {
    PLANE_SIZE,
    PLANE_ALIGNED_SIZE,
    PLANE_OFFSET,
    PLANE_HEIGHT,
    PLANE_WIDTH,
    PLANE_CHANNEL_COUNT,
    PLANE_BITS_PER_PIXEL,
    PLANE_COUNT,
    PLANE_ATTR_SIZE
};

#endif // CUDA_NVSCIBUF_MULTIPLANAR_H

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h`.

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

- **Total Lines**: 135
- **Approximate Size**: 5672 bytes

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
