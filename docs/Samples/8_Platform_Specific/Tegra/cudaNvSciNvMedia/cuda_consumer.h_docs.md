# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/cuda_consumer.h
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/cuda_consumer.h`
- **Filename**: `cuda_consumer.h`
- **Language**: h
- **Size**: 3289 bytes
- **Lines**: 80
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```h
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

#ifndef __CUDA_BUFIMPORT_KERNEL_H__
#define __CUDA_BUFIMPORT_KERNEL_H__

#include <cuda_runtime.h>

#include "helper_cuda.h"
#include "nvmedia_image_nvscibuf.h"
#include "nvmedia_utils/cmdline.h"
#include "nvscisync.h"

struct cudaExternalResInterop
{
    cudaMipmappedArray_t   *d_mipmapArray;
    cudaArray_t            *d_mipLevelArray;
    cudaSurfaceObject_t    *cudaSurfaceNvmediaBuf;
    cudaStream_t            stream;
    cudaExternalMemory_t    extMemImageBuf;
    cudaExternalSemaphore_t waitSem;
    cudaExternalSemaphore_t signalSem;

    int32_t       planeCount;
    uint64_t     *planeOffset;
    int32_t      *imageWidth;
    int32_t      *imageHeight;
    unsigned int *d_outputImage;
};

struct cudaResources
{
    cudaArray_t         *d_yuvArray;
    cudaStream_t         stream;
    cudaSurfaceObject_t *cudaSurfaceNvmediaBuf;
    unsigned int        *d_outputImage;
};

void runCudaOperation(cudaExternalResInterop &cudaExtResObj,
                      NvSciSyncFence         *fence,
                      NvSciSyncFence         *cudaSignalfence,
                      int                     deviceId,
                      int                     iterations);
void runCudaOperation(Blit2DTest *ctx, cudaResources &cudaResObj, int deviceId);

void setupCuda(cudaExternalResInterop &cudaExtResObj,
               NvSciBufObj            &inputBufObj,
               NvSciSyncObj           &syncObj,
               NvSciSyncObj           &cudaSignalerSyncObj,
               int                     deviceId);
void setupCuda(Blit2DTest *ctx, cudaResources &cudaResObj, int deviceId);
void cleanupCuda(cudaExternalResInterop &cudaObjs);
void cleanupCuda(Blit2DTest *ctx, cudaResources &cudaResObj);

#endif

```

---
## High-Level Overview
This file is a h source file and 2 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 5 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda_runtime.h`
- `helper_cuda.h`
- `nvmedia_image_nvscibuf.h`
- `nvmedia_utils/cmdline.h`
- `nvscisync.h`

### Preprocessor Definitions
- **__CUDA_BUFIMPORT_KERNEL_H__**: `#include <cuda_runtime.h>`

### Classes / Structures
#### `struct cudaExternalResInterop`
- Defined in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/cuda_consumer.h

#### `struct cudaResources`
- Defined in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/cuda_consumer.h


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

