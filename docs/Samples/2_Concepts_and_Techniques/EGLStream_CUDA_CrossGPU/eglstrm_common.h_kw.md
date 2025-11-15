# Keywords: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h
---

**Total Keywords**: 18

---

## C

### CONS_DATA {#consdata}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define CONS_DATA       0x04

#define SOCK_PATH "/tmp/tegra_sw_egl_socket"

typedef struct _TestArgs`


## E

### EXTENSION_LIST {#extensionlist}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTENSION_LIST(T)                                                                  \
    T(P`

### EXTLST_DECL {#extlstdecl}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTLST_DECL(tx, x)   tx x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx x;
#define EXTLST_E`

### EXTLST_ENTRY {#extlstentry}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&x, #x},

#define MAX_STRING_SIZE 256
#define INIT_D`

### EXTLST_EXTERN {#extlstextern}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define EXTLST_EXTERN(tx, x) extern tx x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&x, #x},

`


## I

### INIT_DATA {#initdata}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define INIT_DATA       0x01
#define PROD_DATA       0x07
#define CONS_DATA       0x04

#define SOCK`


## M

### MAX_STRING_SIZE {#maxstringsize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define MAX_STRING_SIZE 256
#define INIT_DATA       0x01
#define PROD_DATA       0x07
#define CONS_D`


## P

### PROD_DATA {#proddata}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define PROD_DATA       0x07
#define CONS_DATA       0x04

#define SOCK_PATH "/tmp/tegra_sw_egl_sock`


## S

### SOCK_PATH {#sockpath}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define SOCK_PATH "/tmp/tegra_sw_egl_socket"

typedef struct _TestArgs
{
    unsigned int charCnt;
 `


## T

### TIME_DIFF {#timediff}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define TIME_DIFF(end, start) (getMicrosecond(end) - getMicrosecond(start))

extern EGLStreamKHR g_p`

### TestArgs {#testargs}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `      isProducer;
} TestArgs;

extern int WIDTH,`


## U

### UnixSocketConnect {#unixsocketconnect}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `KHR eglStream);
int UnixSocketConnect(const char *socket_`

### UnixSocketCreate {#unixsocketcreate}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `nt fd_to_send);
int UnixSocketCreate(const char *socket_`


## _

### _EGLSTRM_COMMON_H_ {#eglstrmcommonh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `#define _EGLSTRM_COMMON_H_

#include <signal.h>
#include <stdbool.h>
#include <stdio.h>
#include <st`

### _TestArgs {#testargs}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `struct _TestArgs`


## G

### getMicrosecond {#getmicrosecond}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `double    getMicrosecond(struct timespec t) {`

### getTime {#gettime}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `void getTime(struct timespec *t) {`


## T

### timespec {#timespec}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/eglstrm_common.h](./eglstrm_common.h_docs.md)
- **Context**: `struct timespec`

