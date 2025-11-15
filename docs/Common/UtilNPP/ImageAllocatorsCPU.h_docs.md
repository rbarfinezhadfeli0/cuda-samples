# Documentation: Common/UtilNPP/ImageAllocatorsCPU.h
---
## File Metadata
- **Path**: `Common/UtilNPP/ImageAllocatorsCPU.h`
- **Filename**: `ImageAllocatorsCPU.h`
- **Language**: h
- **Size**: 2942 bytes
- **Lines**: 81
- **Generated**: 2025-11-15 12:53:55 UTC

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

#ifndef NV_UTIL_NPP_IMAGE_ALLOCATORS_CPU_H
#define NV_UTIL_NPP_IMAGE_ALLOCATORS_CPU_H

#include "Exceptions.h"

namespace npp
{

    template <typename D, size_t N>
    class ImageAllocatorCPU
    {
        public:
            static
            D *
            Malloc2D(unsigned int nWidth, unsigned int nHeight, unsigned int *pPitch)
            {
                NPP_ASSERT(nWidth * nHeight > 0);

                D *pResult = new D[nWidth * N * nHeight];
                *pPitch = nWidth * sizeof(D) * N;

                return pResult;
            };

            static
            void
            Free2D(D *pPixels)
            {
                delete[] pPixels;
            };

            static
            void
            Copy2D(D *pDst, size_t nDstPitch, const D *pSrc, size_t nSrcPitch, size_t nWidth, size_t nHeight)
            {
                const void *pSrcLine = pSrc;
                void        *pDstLine = pDst;

                for (size_t iLine = 0; iLine < nHeight; ++iLine)
                {
                    // copy one line worth of data
                    memcpy(pDst, pSrc, nWidth * N * sizeof(D));
                    // move data pointers to next line
                    pDst += nDstPitch;
                    pSrc += nSrcPitch;
                }
            };

    };

} // npp namespace

#endif // NV_UTIL_NPP_IMAGE_ALLOCATORS_CPU_H

```

---
## High-Level Overview
This file is a h source file with 2 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `Exceptions.h`

### Preprocessor Definitions
- **NV_UTIL_NPP_IMAGE_ALLOCATORS_CPU_H**: `#include "Exceptions.h"`

### Functions
#### `void Free2D(D *pPixels)`
- Function in Common/UtilNPP/ImageAllocatorsCPU.h

#### `void Copy2D(D *pDst, size_t nDstPitch, const D *pSrc, size_t nSrcPitch, size_t nWidth, size_t nHeight)`
- Function in Common/UtilNPP/ImageAllocatorsCPU.h

### Classes / Structures
#### `class ImageAllocatorCPU`
- Defined in Common/UtilNPP/ImageAllocatorsCPU.h


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

