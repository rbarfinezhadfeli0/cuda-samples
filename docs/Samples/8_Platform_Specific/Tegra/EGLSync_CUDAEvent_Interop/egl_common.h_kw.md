# Keywords: Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h
---

**Total Keywords**: 13

---

## E

### EXTENSION_LIST {#extensionlist}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define EXTENSION_LIST(T)                                \
    T(PFNEGLCREATEIMAGEKHRPROC, eglCreate`

### EXTLST_DECL {#extlstdecl}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define EXTLST_DECL(tx, x)   tx my_##x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#defin`

### EXTLST_ENTRY {#extlstentry}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_##x, #x},

int eglSetupExtensions(void);
#endif
`

### EXTLST_EXTERN {#extlstextern}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_#`


## _

### _EGL_COMMON_H_ {#eglcommonh}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define _EGL_COMMON_H_

#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.`


## E

### eglClientWaitSyncKHR {#eglclientwaitsynckhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglClientWaitSyncKHR my_eglClientWaitSyncKHR
#define eglGetSyncAttribKHR  my_eglGetSyncAttri`

### eglCreateImageKHR {#eglcreateimagekhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglCreateImageKHR    my_eglCreateImageKHR
#define eglDestroyImageKHR   my_eglDestroyImageKHR`

### eglCreateSync64KHR {#eglcreatesync64khr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglCreateSync64KHR   my_eglCreateSync64KHR
#define eglWaitSyncKHR       my_eglWaitSyncKHR

#`

### eglCreateSyncKHR {#eglcreatesynckhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglCreateSyncKHR     my_eglCreateSyncKHR
#define eglDestroySyncKHR    my_eglDestroySyncKHR
#`

### eglDestroyImageKHR {#egldestroyimagekhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglDestroyImageKHR   my_eglDestroyImageKHR
#define eglCreateSyncKHR     my_eglCreateSyncKHR
`

### eglDestroySyncKHR {#egldestroysynckhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglDestroySyncKHR    my_eglDestroySyncKHR
#define eglClientWaitSyncKHR my_eglClientWaitSyncK`

### eglGetSyncAttribKHR {#eglgetsyncattribkhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglGetSyncAttribKHR  my_eglGetSyncAttribKHR
#define eglCreateSync64KHR   my_eglCreateSync64K`

### eglWaitSyncKHR {#eglwaitsynckhr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/egl_common.h](./egl_common.h_docs.md)
- **Context**: `#define eglWaitSyncKHR       my_eglWaitSyncKHR

#define EXTLST_DECL(tx, x)   tx my_##x = NULL;
#defi`

