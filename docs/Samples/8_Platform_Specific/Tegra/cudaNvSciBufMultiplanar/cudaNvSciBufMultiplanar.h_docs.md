# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h`
- **Filename**: `cudaNvSciBufMultiplanar.h`
- **Language**: h
- **Size**: 5672 bytes
- **Lines**: 135
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
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

---
## High-Level Overview
This file is a h source file and 2 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 5 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda.h`
- `cuda_runtime.h`
- `helper_cuda.h`
- `nvscibuf.h`
- `vector`

### Preprocessor Definitions
- **CUDA_NVSCIBUF_MULTIPLANAR_H**: `#include <cuda.h>`
- **PLANAR_NUM_PLANES**: `3`
- **PLANAR_CHROMA_WIDTH_ORDER**: `2`
- **PLANAR_CHROMA_HEIGHT_ORDER**: `2`
- **ATTR_SIZE**: `20`
- **DEFAULT_GPU**: `0`
- **checkNvSciErrors**: ``
- **checkCudaDrvErrors**: ``

### Classes / Structures
#### `class Caller`
- Defined in Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h

#### `class cudaNvSciBufMultiplanar`
- Defined in Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


---
## Performance & Security Notes
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

