# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.h
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.h`
- **Filename**: `nvmedia_producer.h`
- **Language**: h
- **Size**: 2614 bytes
- **Lines**: 55
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

#ifndef __NVMEDIA_PRODUCER_H__
#define __NVMEDIA_PRODUCER_H__
#include "nvmedia_2d.h"
#include "nvmedia_image.h"
#include "nvmedia_image_nvscibuf.h"
#include "nvmedia_surface.h"
#include "nvmedia_utils/cmdline.h"
#include "nvmedia_utils/image_utils.h"
#include "nvscisync.h"

void runNvMediaBlit2D(TestArgs       *args,
                      Blit2DTest     *ctx,
                      NvSciSyncObj   &syncObj,
                      NvSciSyncFence *preSyncFence,
                      NvSciSyncFence *fence);
void runNvMediaBlit2D(TestArgs *args, Blit2DTest *ctx);
void setupNvMedia(TestArgs     *args,
                  Blit2DTest   *ctx,
                  NvSciBufObj  &srcNvSciBufobj,
                  NvSciBufObj  &dstNvSciBufobj,
                  NvSciSyncObj &syncObj,
                  NvSciSyncObj &preSyncObj,
                  int           cudaDeviceId);
void setupNvMedia(TestArgs *args, Blit2DTest *ctx);
void cleanupNvMedia(Blit2DTest *ctx, NvSciSyncObj &syncObj, NvSciSyncObj &preSyncObj);
void cleanupNvMedia(Blit2DTest *ctx);
#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.

**Dependencies**: 7 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `nvmedia_2d.h`
- `nvmedia_image.h`
- `nvmedia_image_nvscibuf.h`
- `nvmedia_surface.h`
- `nvmedia_utils/cmdline.h`
- `nvmedia_utils/image_utils.h`
- `nvscisync.h`

### Preprocessor Definitions
- **__NVMEDIA_PRODUCER_H__**: `#include "nvmedia_2d.h"`


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

