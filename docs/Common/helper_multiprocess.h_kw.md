# Keywords: Common/helper_multiprocess.h
---

**Total Keywords**: 7

---

## H

### HELPER_MULTIPROCESS_H {#helpermultiprocessh}

- **Type**: macro
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `#define HELPER_MULTIPROCESS_H

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_W`


## S

### SOCK_FOLDER {#sockfolder}

- **Type**: macro
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `#define SOCK_FOLDER ""
#endif

typedef struct sharedMemoryInfo_st {
    void *addr;
    size_t size;`

### ShareableHandle {#shareablehandle}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `ame;
};
typedef int ShareableHandle;
#elif defined(WIN3`


## W

### WIN32_LEAN_AND_MEAN {#win32leanandmean}

- **Type**: macro
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `#define WIN32_LEAN_AND_MEAN
#endif
#include <windows.h>
#include <iostream>
#include <stdio.h>
#incl`


## C

### checkIpcErrors {#checkipcerrors}

- **Type**: macro
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `#define checkIpcErrors(ipcFuncResult) \
    if (ipcFuncResult == -1) { fprintf(stderr, "Failure at %`


## I

### ipcHandle_st {#ipchandlest}

- **Type**: type
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `struct ipcHandle_st`


## S

### sharedMemoryInfo_st {#sharedmemoryinfost}

- **Type**: type
- **File**: [Common/helper_multiprocess.h](./helper_multiprocess.h_docs.md)
- **Context**: `struct sharedMemoryInfo_st`

