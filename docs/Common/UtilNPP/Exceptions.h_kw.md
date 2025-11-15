# Keywords: Common/UtilNPP/Exceptions.h
---

**Total Keywords**: 13

---

## E

### Exception {#exception}

- **Type**: type
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `class Exception`


## N

### NPP_ASSERT {#nppassert}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_ASSERT(C) do {if (!(C)) throw npp::Exception(#C " assertion faild!", __FILE__, __LINE__)`

### NPP_ASSERT_MSG {#nppassertmsg}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_ASSERT_MSG(C, M) do {if (!(C)) throw npp::Exception(#C " assertion faild! Message: " M, `

### NPP_ASSERT_NOT_NULL {#nppassertnotnull}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_ASSERT_NOT_NULL(P) do {if ((P) == 0) throw npp::Exception(#P " not null assertion faild!`

### NPP_CHECK_CUDA {#nppcheckcuda}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_CHECK_CUDA(S) do {cudaError_t eCUDAResult; \
        eCUDAResult = S; \
        if (eCUD`

### NPP_CHECK_CUFFT {#nppcheckcufft}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_CHECK_CUFFT(S) do {cufftResult eCUFFTResult; \
        eCUFFTResult = S; \
        if (e`

### NPP_CHECK_NPP {#nppchecknpp}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_CHECK_NPP(S) do {NppStatus eStatusNPP; \
        eStatusNPP = S; \
        if (eStatusNP`

### NPP_DEBUG_ASSERT {#nppdebugassert}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_DEBUG_ASSERT(C)
#endif

    /// ASSERT for null-pointer test.
    /// It is safe to put `

### NPP_NOT_IMPLEMENTED {#nppnotimplemented}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NPP_NOT_IMPLEMENTED() do {throw npp::Exception("Implementation missing!", __FILE__, __LINE__`

### NV_UTIL_NPP_EXCEPTIONS_H {#nvutilnppexceptionsh}

- **Type**: macro
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `#define NV_UTIL_NPP_EXCEPTIONS_H


#include <string>
#include <sstream>
#include <iostream>

/// All`

### NppStatus {#nppstatus}

- **Type**: identifier
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `PP_CHECK_NPP(S) do {NppStatus eStatusNPP; \
     `


## C

### can {#can}

- **Type**: type
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `class can`


## W

### will {#will}

- **Type**: type
- **File**: [Common/UtilNPP/Exceptions.h](./Exceptions.h_docs.md)
- **Context**: `class will`

