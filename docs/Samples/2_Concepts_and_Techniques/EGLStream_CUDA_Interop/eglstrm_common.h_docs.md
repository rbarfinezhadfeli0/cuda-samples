# Documentation: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h`
- **Filename**: `eglstrm_common.h`
- **Language**: h
- **Size**: 4895 bytes
- **Lines**: 101
- **Generated**: 2025-11-15 12:53:54 UTC

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

//
// DESCRIPTION:   Common EGL stream functions header file
//

#ifndef _EGLSTRM_COMMON_H_
#define _EGLSTRM_COMMON_H_

#include <signal.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>

#include "cuda.h"
#include "cudaEGL.h"
#include "helper_cuda_drvapi.h"

#define EXTENSION_LIST(T)                                                                  \
    T(PFNEGLCREATESTREAMKHRPROC, eglCreateStreamKHR)                                       \
    T(PFNEGLDESTROYSTREAMKHRPROC, eglDestroyStreamKHR)                                     \
    T(PFNEGLQUERYSTREAMKHRPROC, eglQueryStreamKHR)                                         \
    T(PFNEGLQUERYSTREAMU64KHRPROC, eglQueryStreamu64KHR)                                   \
    T(PFNEGLQUERYSTREAMTIMEKHRPROC, eglQueryStreamTimeKHR)                                 \
    T(PFNEGLSTREAMATTRIBKHRPROC, eglStreamAttribKHR)                                       \
    T(PFNEGLSTREAMCONSUMERACQUIREKHRPROC, eglStreamConsumerAcquireKHR)                     \
    T(PFNEGLSTREAMCONSUMERRELEASEKHRPROC, eglStreamConsumerReleaseKHR)                     \
    T(PFNEGLSTREAMCONSUMERGLTEXTUREEXTERNALKHRPROC, eglStreamConsumerGLTextureExternalKHR) \
    T(PFNEGLGETSTREAMFILEDESCRIPTORKHRPROC, eglGetStreamFileDescriptorKHR)                 \
    T(PFNEGLQUERYDEVICESEXTPROC, eglQueryDevicesEXT)                                       \
    T(PFNEGLGETPLATFORMDISPLAYEXTPROC, eglGetPlatformDisplayEXT)                           \
    T(PFNEGLQUERYDEVICEATTRIBEXTPROC, eglQueryDeviceAttribEXT)                             \
    T(PFNEGLCREATESTREAMFROMFILEDESCRIPTORKHRPROC, eglCreateStreamFromFileDescriptorKHR)

#define eglCreateStreamKHR                    my_eglCreateStreamKHR
#define eglDestroyStreamKHR                   my_eglDestroyStreamKHR
#define eglQueryStreamKHR                     my_eglQueryStreamKHR
#define eglQueryStreamu64KHR                  my_eglQueryStreamu64KHR
#define eglQueryStreamTimeKHR                 my_eglQueryStreamTimeKHR
#define eglStreamAttribKHR                    my_eglStreamAttribKHR
#define eglStreamConsumerAcquireKHR           my_eglStreamConsumerAcquireKHR
#define eglStreamConsumerReleaseKHR           my_eglStreamConsumerReleaseKHR
#define eglStreamConsumerGLTextureExternalKHR my_eglStreamConsumerGLTextureExternalKHR
#define eglGetStreamFileDescriptorKHR         my_eglGetStreamFileDescriptorKHR
#define eglCreateStreamFromFileDescriptorKHR  my_eglCreateStreamFromFileDescriptorKHR
#define eglQueryDevicesEXT                    my_eglQueryDevicesEXT
#define eglGetPlatformDisplayEXT              my_eglGetPlatformDisplayEXT
#define eglQueryDeviceAttribEXT               my_eglQueryDeviceAttribEXT

#define EXTLST_DECL(tx, x)   tx my_##x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_##x, #x},

#define MAX_STRING_SIZE 256
#define WIDTH           720
#define HEIGHT          480

typedef struct _TestArgs
{
    char        *infile1;
    char        *infile2;
    bool         isARGB;
    unsigned int inputWidth;
    unsigned int inputHeight;
    bool         pitchLinearOutput;
} TestArgs;

int  eglSetupExtensions(void);
int  EGLStreamInit(int *dev);
void EGLStreamFini(void);
#endif

```

---
## High-Level Overview
This file is a h source file with 1 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 11 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `signal.h`
- `stdbool.h`
- `stdio.h`
- `stdlib.h`
- `string.h`
- `sys/stat.h`
- `sys/time.h`
- `unistd.h`
- `cuda.h`
- `cudaEGL.h`
- `helper_cuda_drvapi.h`

### Preprocessor Definitions
- **_EGLSTRM_COMMON_H_**: `#include <signal.h>`
- **EXTENSION_LIST**: ``
- **eglCreateStreamKHR**: `my_eglCreateStreamKHR`
- **eglDestroyStreamKHR**: `my_eglDestroyStreamKHR`
- **eglQueryStreamKHR**: `my_eglQueryStreamKHR`
- **eglQueryStreamu64KHR**: `my_eglQueryStreamu64KHR`
- **eglQueryStreamTimeKHR**: `my_eglQueryStreamTimeKHR`
- **eglStreamAttribKHR**: `my_eglStreamAttribKHR`
- **eglStreamConsumerAcquireKHR**: `my_eglStreamConsumerAcquireKHR`
- **eglStreamConsumerReleaseKHR**: `my_eglStreamConsumerReleaseKHR`
- **eglStreamConsumerGLTextureExternalKHR**: `my_eglStreamConsumerGLTextureExternalKHR`
- **eglGetStreamFileDescriptorKHR**: `my_eglGetStreamFileDescriptorKHR`
- **eglCreateStreamFromFileDescriptorKHR**: `my_eglCreateStreamFromFileDescriptorKHR`
- **eglQueryDevicesEXT**: `my_eglQueryDevicesEXT`
- **eglGetPlatformDisplayEXT**: `my_eglGetPlatformDisplayEXT`
- **eglQueryDeviceAttribEXT**: `my_eglQueryDeviceAttribEXT`
- **EXTLST_DECL**: ``
- **EXTLST_EXTERN**: ``
- **EXTLST_ENTRY**: ``
- **MAX_STRING_SIZE**: `256`
- **WIDTH**: `720`
- **HEIGHT**: `480`

### Functions
#### `define EXTLST_ENTRY(tx, x)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h

### Classes / Structures
#### `struct _TestArgs`
- Defined in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h


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

