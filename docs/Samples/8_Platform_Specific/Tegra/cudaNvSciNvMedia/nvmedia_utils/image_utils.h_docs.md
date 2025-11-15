# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.h
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.h`
- **Filename**: `image_utils.h`
- **Language**: h
- **Size**: 4321 bytes
- **Lines**: 128
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

#ifndef _NVMEDIA_TEST_IMAGE_UTILS_H_
#define _NVMEDIA_TEST_IMAGE_UTILS_H_

#ifdef __cplusplus
extern "C"
{
#endif

#include "cmdline.h"
#include "misc_utils.h"
#include "nvmedia_core.h"
#include "nvmedia_image.h"
#include "nvmedia_surface.h"

#if (NV_IS_SAFETY == 1)
#include "nvmedia_image_internal.h"
#endif

#define PACK_RGBA(R, G, B, A) (((uint32_t)(A) << 24) | ((uint32_t)(B) << 16) | ((uint32_t)(G) << 8) | (uint32_t)(R))
#define DEFAULT_ALPHA         0x80


    //  ReadImage
    //
    //    ReadImage()  Read image from file
    //
    //  Arguments:
    //
    //   filename
    //      (in) Input file name
    //
    //   frameNum
    //      (in) Frame number to read. Use for stream input files.
    //
    //   width
    //      (in) Surface width
    //
    //   height
    //      (in) Surface height
    //
    //   image
    //      (out) Pointer to pre-allocated output surface
    //
    //   uvOrderFlag
    //      (in) Flag for UV order. If true - UV; If false - VU;
    //
    //   bytesPerPixel
    //      (in) Bytes per pixel. Nedded for RAW image types handling.
    //         RAW8 - 1 byte per pixel
    //         RAW10, RAW12, RAW14 - 2 bytes per pixel
    //
    //   pixelAlignment
    //      (in) Alignment of bits in pixel.
    //         0 - LSB Aligned
    //         1 - MSB Aligned

    NvMediaStatus ReadImage(char         *fileName,
                            uint32_t      frameNum,
                            uint32_t      width,
                            uint32_t      height,
                            NvMediaImage *image,
                            NvMediaBool   uvOrderFlag,
                            uint32_t      bytesPerPixel,
                            uint32_t      pixelAlignment);

    //  InitImage
    //
    //    InitImage()  Init image data to zeros
    //
    //  Arguments:
    //
    //   image
    //      (in) image to initialize
    //
    //   width
    //      (in) Surface width
    //
    //   height
    //      (in) Surface height

    NvMediaStatus InitImage(NvMediaImage *image, uint32_t width, uint32_t height);

    NvMediaStatus
    AllocateBufferToWriteImage(Blit2DTest *ctx, NvMediaImage *image, NvMediaBool uvOrderFlag, NvMediaBool appendFlag);

    //  WriteImageToBuffer
    //
    //    WriteImageToBuffer()  Save RGB or YUV image
    //
    NvMediaStatus WriteImageToAllocatedBuffer(Blit2DTest   *ctx,
                                              NvMediaImage *image,
                                              NvMediaBool   uvOrderFlag,
                                              NvMediaBool   appendFlag,
                                              uint32_t      bytesPerPixel);

#ifdef __cplusplus
}
#endif

#endif /* _NVMEDIA_TEST_IMAGE_UTILS_H_ */

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.

**Dependencies**: 6 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cmdline.h`
- `misc_utils.h`
- `nvmedia_core.h`
- `nvmedia_image.h`
- `nvmedia_surface.h`
- `nvmedia_image_internal.h`

### Preprocessor Definitions
- **_NVMEDIA_TEST_IMAGE_UTILS_H_**: `#ifdef __cplusplus`
- **PACK_RGBA**: ``
- **DEFAULT_ALPHA**: `0x80`


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

