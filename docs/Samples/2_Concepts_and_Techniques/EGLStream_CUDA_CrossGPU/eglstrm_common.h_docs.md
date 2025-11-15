# Documentation: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h`
- **Filename**: `eglstrm_common.h`
- **Language**: h
- **Size**: 4821 bytes
- **Lines**: 110
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
#include <sys/socket.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <sys/types.h>
#include <sys/un.h>
#include <time.h>
#include <unistd.h>

#include "cuda.h"
#include "cudaEGL.h"
#define TIME_DIFF(end, start) (getMicrosecond(end) - getMicrosecond(start))

extern EGLStreamKHR g_producerEglStream;
extern EGLStreamKHR g_consumerEglStream;
extern EGLDisplay   g_producerEglDisplay;
extern EGLDisplay   g_consumerEglDisplay;
extern int          cudaDevIndexCons;
extern int          cudaDevIndexProd;
extern bool         verbose;

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
    T(PFNEGLQUERYDEVICESEXTPROC, eglQueryDevicesEXT)                                       \
    T(PFNEGLGETPLATFORMDISPLAYEXTPROC, eglGetPlatformDisplayEXT)                           \
    T(PFNEGLGETSTREAMFILEDESCRIPTORKHRPROC, eglGetStreamFileDescriptorKHR)                 \
    T(PFNEGLQUERYDEVICEATTRIBEXTPROC, eglQueryDeviceAttribEXT)                             \
    T(PFNEGLCREATESTREAMFROMFILEDESCRIPTORKHRPROC, eglCreateStreamFromFileDescriptorKHR)

#define EXTLST_DECL(tx, x)   tx x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&x, #x},

#define MAX_STRING_SIZE 256
#define INIT_DATA       0x01
#define PROD_DATA       0x07
#define CONS_DATA       0x04

#define SOCK_PATH "/tmp/tegra_sw_egl_socket"

typedef struct _TestArgs
{
    unsigned int charCnt;
    bool         isProducer;
} TestArgs;

extern int WIDTH, HEIGHT;

int  eglSetupExtensions(bool is_dgpu);
int  EGLStreamInit(bool isCrossDevice, int isConsumer, EGLNativeFileDescriptorKHR fileDesc);
void EGLStreamFini(void);

int EGLStreamSetAttr(EGLDisplay display, EGLStreamKHR eglStream);
int UnixSocketConnect(const char *socket_name);
int EGLStreamSendfd(int send_fd, int fd_to_send);
int UnixSocketCreate(const char *socket_name);
int EGLStreamReceivefd(int connect_fd);

static clockid_t clock_id = CLOCK_MONOTONIC; // CLOCK_PROCESS_CPUTIME_ID;
static double    getMicrosecond(struct timespec t) { return ((t.tv_sec) * 1000000.0 + (t.tv_nsec) / 1.0e3); }

static inline void getTime(struct timespec *t) { clock_gettime(clock_id, t); }
#endif

```

---
## High-Level Overview
This file is a h source file with 3 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 14 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `signal.h`
- `stdbool.h`
- `stdio.h`
- `stdlib.h`
- `string.h`
- `sys/socket.h`
- `sys/stat.h`
- `sys/time.h`
- `sys/types.h`
- `sys/un.h`
- `time.h`
- `unistd.h`
- `cuda.h`
- `cudaEGL.h`

### Preprocessor Definitions
- **_EGLSTRM_COMMON_H_**: `#include <signal.h>`
- **TIME_DIFF**: ``
- **EXTENSION_LIST**: ``
- **EXTLST_DECL**: ``
- **EXTLST_EXTERN**: ``
- **EXTLST_ENTRY**: ``
- **MAX_STRING_SIZE**: `256`
- **INIT_DATA**: `0x01`
- **PROD_DATA**: `0x07`
- **CONS_DATA**: `0x04`
- **SOCK_PATH**: `"/tmp/tegra_sw_egl_socket"`

### Functions
#### `define EXTLST_ENTRY(tx, x)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h

#### `double getMicrosecond(struct timespec t)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h

#### `void getTime(struct timespec *t)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h

### Classes / Structures
#### `struct _TestArgs`
- Defined in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h


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

