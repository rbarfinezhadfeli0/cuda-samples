# Documentation: Common/UtilNPP/ImagesCPU.h
---
## File Metadata
- **Path**: `Common/UtilNPP/ImagesCPU.h`
- **Filename**: `ImagesCPU.h`
- **Language**: h
- **Size**: 4549 bytes
- **Lines**: 122
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

#ifndef NV_UTIL_NPP_IMAGES_CPU_H
#define NV_UTIL_NPP_IMAGES_CPU_H

#include "ImagePacked.h"

#include "ImageAllocatorsCPU.h"
#include "Exceptions.h"

#include <npp.h>


namespace npp
{

    template<typename D, unsigned int N, class A>
    class ImageCPU: public npp::ImagePacked<D, N, A>
    {
        public:

            ImageCPU()
            {
                ;
            }

            ImageCPU(unsigned int nWidth, unsigned int nHeight): ImagePacked<D, N, A>(nWidth, nHeight)
            {
                ;
            }

            explicit
            ImageCPU(const npp::Image::Size &rSize): ImagePacked<D, N, A>(rSize)
            {
                ;
            }

            ImageCPU(const ImageCPU<D, N, A> &rImage): Image(rImage)
            {
                ;
            }

            virtual
            ~ImageCPU()
            {
                ;
            }

            ImageCPU &
            operator= (const ImageCPU<D, N, A> &rImage)
            {
                ImagePacked<D, N, A>::operator= (rImage);

                return *this;
            }

            npp::Pixel<D, N> &
            operator()(unsigned int iX, unsigned int iY)
            {
                return *ImagePacked<D, N, A>::pixels(iX, iY);
            }

            npp::Pixel<D, N>
            operator()(unsigned int iX, unsigned int iY)
            const
            {
                return *ImagePacked<D, N, A>::pixels(iX, iY);
            }

    };


    typedef ImageCPU<Npp8u,  1, npp::ImageAllocatorCPU<Npp8u,      1>  >   ImageCPU_8u_C1;
    typedef ImageCPU<Npp8u,  2, npp::ImageAllocatorCPU<Npp8u,      2>  >   ImageCPU_8u_C2;
    typedef ImageCPU<Npp8u,  3, npp::ImageAllocatorCPU<Npp8u,      3>  >   ImageCPU_8u_C3;
    typedef ImageCPU<Npp8u,  4, npp::ImageAllocatorCPU<Npp8u,      4>  >   ImageCPU_8u_C4;

    typedef ImageCPU<Npp16u, 1, npp::ImageAllocatorCPU<Npp16u,     1>  >   ImageCPU_16u_C1;
    typedef ImageCPU<Npp16u, 3, npp::ImageAllocatorCPU<Npp16u,     3>  >   ImageCPU_16u_C3;
    typedef ImageCPU<Npp16u, 4, npp::ImageAllocatorCPU<Npp16u,     4>  >   ImageCPU_16u_C4;

    typedef ImageCPU<Npp16s, 1, npp::ImageAllocatorCPU<Npp16s,     1>  >   ImageCPU_16s_C1;
    typedef ImageCPU<Npp16s, 3, npp::ImageAllocatorCPU<Npp16s,     3>  >   ImageCPU_16s_C3;
    typedef ImageCPU<Npp16s, 4, npp::ImageAllocatorCPU<Npp16s,     4>  >   ImageCPU_16s_C4;

    typedef ImageCPU<Npp32s, 1, npp::ImageAllocatorCPU<Npp32s,     1>  >   ImageCPU_32s_C1;
    typedef ImageCPU<Npp32s, 3, npp::ImageAllocatorCPU<Npp32s,     3>  >   ImageCPU_32s_C3;
    typedef ImageCPU<Npp32s, 4, npp::ImageAllocatorCPU<Npp32s,     4>  >   ImageCPU_32s_C4;

    typedef ImageCPU<Npp32f, 1, npp::ImageAllocatorCPU<Npp32f,     1>  >   ImageCPU_32f_C1;
    typedef ImageCPU<Npp32f, 3, npp::ImageAllocatorCPU<Npp32f,     3>  >   ImageCPU_32f_C3;
    typedef ImageCPU<Npp32f, 4, npp::ImageAllocatorCPU<Npp32f,     4>  >   ImageCPU_32f_C4;

} // npp namespace

#endif // NV_IMAGE_IPP_H

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `ImagePacked.h`
- `ImageAllocatorsCPU.h`
- `Exceptions.h`
- `npp.h`

### Preprocessor Definitions
- **NV_UTIL_NPP_IMAGES_CPU_H**: `#include "ImagePacked.h"`


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

