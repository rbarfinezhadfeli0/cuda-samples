# Keywords: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h
---

**Total Keywords**: 24

---

## E

### EXTENSION_LIST {#extensionlist}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTENSION_LIST(T)                                                                  \
    T(P`

### EXTLST_DECL {#extlstdecl}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTLST_DECL(tx, x)   tx my_##x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#defin`

### EXTLST_ENTRY {#extlstentry}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_##x, #x},

#define MAX_STRING_SIZE 256
#define W`

### EXTLST_EXTERN {#extlstextern}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_#`


## H

### HEIGHT {#height}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define HEIGHT          480

typedef struct _TestArgs
{
    char        *infile1;
    char        *i`


## M

### MAX_STRING_SIZE {#maxstringsize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define MAX_STRING_SIZE 256
#define WIDTH           720
#define HEIGHT          480

typedef struct `


## T

### TestArgs {#testargs}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `itchLinearOutput;
} TestArgs;

int  eglSetupExte`


## W

### WIDTH {#width}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define WIDTH           720
#define HEIGHT          480

typedef struct _TestArgs
{
    char        `


## _

### _EGLSTRM_COMMON_H_ {#eglstrmcommonh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define _EGLSTRM_COMMON_H_

#include <signal.h>
#include <stdbool.h>
#include <stdio.h>
#include <st`

### _TestArgs {#testargs}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `struct _TestArgs`


## E

### eglCreateStreamFromFileDescriptorKHR {#eglcreatestreamfromfiledescriptorkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglCreateStreamFromFileDescriptorKHR  my_eglCreateStreamFromFileDescriptorKHR
#define eglQue`

### eglCreateStreamKHR {#eglcreatestreamkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglCreateStreamKHR                    my_eglCreateStreamKHR
#define eglDestroyStreamKHR     `

### eglDestroyStreamKHR {#egldestroystreamkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglDestroyStreamKHR                   my_eglDestroyStreamKHR
#define eglQueryStreamKHR      `

### eglGetPlatformDisplayEXT {#eglgetplatformdisplayext}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglGetPlatformDisplayEXT              my_eglGetPlatformDisplayEXT
#define eglQueryDeviceAttr`

### eglGetStreamFileDescriptorKHR {#eglgetstreamfiledescriptorkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglGetStreamFileDescriptorKHR         my_eglGetStreamFileDescriptorKHR
#define eglCreateStre`

### eglQueryDeviceAttribEXT {#eglquerydeviceattribext}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglQueryDeviceAttribEXT               my_eglQueryDeviceAttribEXT

#define EXTLST_DECL(tx, x)`

### eglQueryDevicesEXT {#eglquerydevicesext}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglQueryDevicesEXT                    my_eglQueryDevicesEXT
#define eglGetPlatformDisplayEXT`

### eglQueryStreamKHR {#eglquerystreamkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglQueryStreamKHR                     my_eglQueryStreamKHR
#define eglQueryStreamu64KHR     `

### eglQueryStreamTimeKHR {#eglquerystreamtimekhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglQueryStreamTimeKHR                 my_eglQueryStreamTimeKHR
#define eglStreamAttribKHR   `

### eglQueryStreamu64KHR {#eglquerystreamu64khr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglQueryStreamu64KHR                  my_eglQueryStreamu64KHR
#define eglQueryStreamTimeKHR `

### eglStreamAttribKHR {#eglstreamattribkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglStreamAttribKHR                    my_eglStreamAttribKHR
#define eglStreamConsumerAcquire`

### eglStreamConsumerAcquireKHR {#eglstreamconsumeracquirekhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglStreamConsumerAcquireKHR           my_eglStreamConsumerAcquireKHR
#define eglStreamConsum`

### eglStreamConsumerGLTextureExternalKHR {#eglstreamconsumergltextureexternalkhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglStreamConsumerGLTextureExternalKHR my_eglStreamConsumerGLTextureExternalKHR
#define eglGe`

### eglStreamConsumerReleaseKHR {#eglstreamconsumerreleasekhr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define eglStreamConsumerReleaseKHR           my_eglStreamConsumerReleaseKHR
#define eglStreamConsum`

