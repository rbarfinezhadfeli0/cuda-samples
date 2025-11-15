# Keywords: Common/nvrtc_helper.h
---

**Total Keywords**: 5

---

## C

### COMMON_NVRTC_HELPER_H_ {#commonnvrtchelperh}

- **Type**: macro
- **File**: [Common/nvrtc_helper.h](./nvrtc_helper.h_docs.md)
- **Context**: `#define COMMON_NVRTC_HELPER_H_ 1

#include <cuda.h>
#include <helper_cuda_drvapi.h>
#include <nvrtc.`


## H

### HeaderNames {#headernames}

- **Type**: identifier
- **File**: [Common/nvrtc_helper.h](./nvrtc_helper.h_docs.md)
- **Context**: `leOptions;
    char HeaderNames[256];
#if defined(W`


## N

### NVRTC_SAFE_CALL {#nvrtcsafecall}

- **Type**: macro
- **File**: [Common/nvrtc_helper.h](./nvrtc_helper.h_docs.md)
- **Context**: `#define NVRTC_SAFE_CALL(Name, x)                                \
  do {                            `


## C

### compileFileToCUBIN {#compilefiletocubin}

- **Type**: function
- **File**: [Common/nvrtc_helper.h](./nvrtc_helper.h_docs.md)
- **Context**: `void compileFileToCUBIN(char *filename, int argc, char **argv, char **cubinResult,
                 `


## L

### loadCUBIN {#loadcubin}

- **Type**: function
- **File**: [Common/nvrtc_helper.h](./nvrtc_helper.h_docs.md)
- **Context**: `CUmodule loadCUBIN(char *cubin, int argc, char **argv) {`

