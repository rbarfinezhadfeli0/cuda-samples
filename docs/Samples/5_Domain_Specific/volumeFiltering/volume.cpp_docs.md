# Documentation: Samples/5_Domain_Specific/volumeFiltering/volume.cpp
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/volumeFiltering/volume.cpp`
- **Filename**: `volume.cpp`
- **Language**: cpp
- **Size**: 3700 bytes
- **Lines**: 91
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
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

// CUDA Runtime
#include <cuda_runtime.h>

// Helper functions
#include <helper_cuda.h>
#include <helper_math.h>

#include "volume.h"

void Volume_init(Volume *vol, cudaExtent dataSize, void *h_data, int allowStore)
{
    // create 3D array
    vol->channelDesc = cudaCreateChannelDesc<VolumeType>();
    checkCudaErrors(
        cudaMalloc3DArray(&vol->content, &vol->channelDesc, dataSize, allowStore ? cudaArraySurfaceLoadStore : 0));
    vol->size = dataSize;

    if (h_data) {
        // copy data to 3D array
        cudaMemcpy3DParms copyParams = {0};
        copyParams.srcPtr =
            make_cudaPitchedPtr(h_data, dataSize.width * sizeof(VolumeType), dataSize.width, dataSize.height);
        copyParams.dstArray = vol->content;
        copyParams.extent   = dataSize;
        copyParams.kind     = cudaMemcpyHostToDevice;
        checkCudaErrors(cudaMemcpy3D(&copyParams));
    }

    if (allowStore) {
        cudaResourceDesc surfRes;
        memset(&surfRes, 0, sizeof(cudaResourceDesc));
        surfRes.resType         = cudaResourceTypeArray;
        surfRes.res.array.array = vol->content;

        checkCudaErrors(cudaCreateSurfaceObject(&vol->volumeSurf, &surfRes));
    }

    cudaResourceDesc texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType         = cudaResourceTypeArray;
    texRes.res.array.array = vol->content;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = true;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.addressMode[1]   = cudaAddressModeWrap;
    texDescr.addressMode[2]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeNormalizedFloat; // VolumeTypeInfo<VolumeType>::readMode;

    checkCudaErrors(cudaCreateTextureObject(&vol->volumeTex, &texRes, &texDescr, NULL));
}

void Volume_deinit(Volume *vol)
{
    checkCudaErrors(cudaDestroyTextureObject(vol->volumeTex));
    checkCudaErrors(cudaDestroySurfaceObject(vol->volumeSurf));
    checkCudaErrors(cudaFreeArray(vol->content));
    vol->content = 0;
}

```

---
## High-Level Overview
This file is a cpp source file with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda_runtime.h`
- `helper_cuda.h`
- `helper_math.h`
- `volume.h`

### Functions
#### `void Volume_init(Volume *vol, cudaExtent dataSize, void *h_data, int allowStore)`
- Function in Samples/5_Domain_Specific/volumeFiltering/volume.cpp

#### `void Volume_deinit(Volume *vol)`
- Function in Samples/5_Domain_Specific/volumeFiltering/volume.cpp


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

