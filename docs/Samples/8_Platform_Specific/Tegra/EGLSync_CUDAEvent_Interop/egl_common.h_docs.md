# Documentation: Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h`
- **Filename**: `egl_common.h`
- **Language**: h
- **Size**: 2991 bytes
- **Lines**: 73
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

//
// DESCRIPTION:   Common EGL functions header file
//

#ifndef _EGL_COMMON_H_
#define _EGL_COMMON_H_

#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>

#include "cuda.h"
#include "cudaEGL.h"

EGLImageKHR eglImage;

#define EXTENSION_LIST(T)                                \
    T(PFNEGLCREATEIMAGEKHRPROC, eglCreateImageKHR)       \
    T(PFNEGLDESTROYIMAGEKHRPROC, eglDestroyImageKHR)     \
    T(PFNEGLCREATESYNCKHRPROC, eglCreateSyncKHR)         \
    T(PFNEGLDESTROYSYNCKHRPROC, eglDestroySyncKHR)       \
    T(PFNEGLCLIENTWAITSYNCKHRPROC, eglClientWaitSyncKHR) \
    T(PFNEGLGETSYNCATTRIBKHRPROC, eglGetSyncAttribKHR)   \
    T(PFNEGLCREATESYNC64KHRPROC, eglCreateSync64KHR)     \
    T(PFNEGLWAITSYNCKHRPROC, eglWaitSyncKHR)

#define eglCreateImageKHR    my_eglCreateImageKHR
#define eglDestroyImageKHR   my_eglDestroyImageKHR
#define eglCreateSyncKHR     my_eglCreateSyncKHR
#define eglDestroySyncKHR    my_eglDestroySyncKHR
#define eglClientWaitSyncKHR my_eglClientWaitSyncKHR
#define eglGetSyncAttribKHR  my_eglGetSyncAttribKHR
#define eglCreateSync64KHR   my_eglCreateSync64KHR
#define eglWaitSyncKHR       my_eglWaitSyncKHR

#define EXTLST_DECL(tx, x)   tx my_##x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_##x, #x},

int eglSetupExtensions(void);
#endif

```

---
## High-Level Overview
This file is a h source file with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 9 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `signal.h`
- `stdio.h`
- `stdlib.h`
- `string.h`
- `sys/stat.h`
- `sys/time.h`
- `unistd.h`
- `cuda.h`
- `cudaEGL.h`

### Preprocessor Definitions
- **_EGL_COMMON_H_**: `#include <signal.h>`
- **EXTENSION_LIST**: ``
- **eglCreateImageKHR**: `my_eglCreateImageKHR`
- **eglDestroyImageKHR**: `my_eglDestroyImageKHR`
- **eglCreateSyncKHR**: `my_eglCreateSyncKHR`
- **eglDestroySyncKHR**: `my_eglDestroySyncKHR`
- **eglClientWaitSyncKHR**: `my_eglClientWaitSyncKHR`
- **eglGetSyncAttribKHR**: `my_eglGetSyncAttribKHR`
- **eglCreateSync64KHR**: `my_eglCreateSync64KHR`
- **eglWaitSyncKHR**: `my_eglWaitSyncKHR`
- **EXTLST_DECL**: ``
- **EXTLST_EXTERN**: ``
- **EXTLST_ENTRY**: ``

### Functions
#### `define EXTLST_ENTRY(tx, x)`
- Function in Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h


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

