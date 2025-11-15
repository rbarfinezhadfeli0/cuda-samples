# Keywords: Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h
---

**Total Keywords**: 12

---

## C

### CUDANVSCI_H {#cudanvscih}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `#define CUDANVSCI_H

#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <nvscibuf.h>
#inclu`


## N

### NvSciBufAttrList {#nvscibufattrlist}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `cConflictList;

    NvSciBufAttrList rawBufUnreconciledL`

### NvSciBufModule {#nvscibufmodule}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `e   syncModule;
    NvSciBufModule    buffModule;
    `

### NvSciBufObj {#nvscibufobj}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `Obj    syncObj;
    NvSciBufObj     rawBufObj;
    `

### NvSciError {#nvscierror}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `          \
        NvSciError _status = call;    `

### NvSciError_Success {#nvscierrorsuccess}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `      \
        if (NvSciError_Success != _status) {      `

### NvSciSyncAttrList {#nvscisyncattrlist}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `    buffModule;
    NvSciSyncAttrList syncUnreconciledLis`

### NvSciSyncFence {#nvscisyncfence}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `   imageBufObj;
    NvSciSyncFence *fence;

    cudaNv`

### NvSciSyncModule {#nvscisyncmodule}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `eight;

public:
    NvSciSyncModule   syncModule;
    N`

### NvSciSyncObj {#nvscisyncobj}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `ffAttrListOut;

    NvSciSyncObj    syncObj;
    NvS`


## C

### checkNvSciErrors {#checknvscierrors}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `#define checkNvSciErrors(call)                                   \
    do {                         `

### cudaNvSci {#cudanvsci}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.h](./cudaNvSci.h_docs.md)
- **Context**: `class cudaNvSci`

