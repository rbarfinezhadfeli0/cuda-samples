# Keywords: Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c
---

**Total Keywords**: 23

---

## C

### CHECKED_CALL {#checkedcall}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define CHECKED_CALL(call)            \
    do {                              \
        CUresult res`

### CUDA_INIT_D3D10 {#cudainitd3d10}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define CUDA_INIT_D3D10
// #define CUDA_INIT_D3D11
// #define CUDA_INIT_OPENGL

#include "cuda_drvap`

### CUDA_INIT_D3D11 {#cudainitd3d11}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define CUDA_INIT_D3D11
// #define CUDA_INIT_OPENGL

#include "cuda_drvapi_dynlink.h"

#include <std`

### CUDA_INIT_D3D9 {#cudainitd3d9}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define CUDA_INIT_D3D9
// #define CUDA_INIT_D3D10
// #define CUDA_INIT_D3D11
// #define CUDA_INIT_OP`

### CUDA_INIT_OPENGL {#cudainitopengl}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define CUDA_INIT_OPENGL

#include "cuda_drvapi_dynlink.h"

#include <stdio.h>

tcuInit             `

### CudaDrvLib {#cudadrvlib}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `me *)GetProcAddress(CudaDrvLib, #name);           `


## G

### GET_DRIVER_HANDLE {#getdriverhandle}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `CUresult GET_DRIVER_HANDLE(CUDADRIVER *pInstance)
{`

### GET_PROC {#getproc}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC(name)          GET_PROC_REQUIRED(name)
#define GET_PROC_V2(name)       GET_PROC_EX_`

### GET_PROC_ERROR_FUNCTIONS {#getprocerrorfunctions}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_ERROR_FUNCTIONS(name, alias, required)                               \
    alias = `

### GET_PROC_EX {#getprocex}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_EX(name, alias, required)                                               \
    alias`

### GET_PROC_EX_V2 {#getprocexv2}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_EX_V2(name, alias, required)                                                       `

### GET_PROC_EX_V3 {#getprocexv3}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_EX_V3(name, alias, required)                                                       `

### GET_PROC_OPTIONAL {#getprocoptional}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_OPTIONAL(name) GET_PROC_EX(name, name, 0)
#define GET_PROC(name)          GET_PROC_`

### GET_PROC_REQUIRED {#getprocrequired}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_REQUIRED(name) GET_PROC_EX(name, name, 1)
#define GET_PROC_OPTIONAL(name) GET_PROC_`

### GET_PROC_V2 {#getprocv2}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_V2(name)       GET_PROC_EX_V2(name, name, 1)
#define GET_PROC_V3(name)       GET_PR`

### GET_PROC_V3 {#getprocv3}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define GET_PROC_V3(name)       GET_PROC_EX_V3(name, name, 1)

CUresult INIT_ERROR_FUNCTIONS(void)
{`

### GetModuleHandle {#getmodulehandle}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `
{
    *pInstance = GetModuleHandle(__CudaLibName);
   `

### GetProcAddress {#getprocaddress}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: ` alias = (t##name *)GetProcAddress(CudaDrvLib, #name);`


## I

### INIT_ERROR_FUNCTIONS {#initerrorfunctions}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `CUresult INIT_ERROR_FUNCTIONS(void)
{`


## L

### LOAD_LIBRARY {#loadlibrary}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `CUresult LOAD_LIBRARY(CUDADRIVER *pInstance)
{`

### LoadLibrary {#loadlibrary}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `
{
    *pInstance = LoadLibrary(__CudaLibName);

  `


## S

### STRINGIFY {#stringify}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `#define STRINGIFY(X) #X

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
`


## C

### cuInit {#cuinit}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink.c](./cuda_drvapi_dynlink.c_docs.md)
- **Context**: `CUDAAPI cuInit(unsigned int Flags, int cudaVersion)
{`

