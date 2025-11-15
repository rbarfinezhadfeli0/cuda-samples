# Keywords: Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h
---

**Total Keywords**: 28

---

## C

### CUBLASTEST_FAILED {#cublastestfailed}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUBLASTEST_FAILED 1
#define CUBLASTEST_WAIVED 2

//=========================================`

### CUBLASTEST_PASSED {#cublastestpassed}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUBLASTEST_PASSED 0
#define CUBLASTEST_FAILED 1
#define CUBLASTEST_WAIVED 2

//=============`

### CUBLASTEST_WAIVED {#cublastestwaived}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUBLASTEST_WAIVED 2

//=====================================================================`

### CUDA_CONG {#cudacong}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUDA_CONG (cuda_jcong = 69069 * cuda_jcong + 1234567)
#define KISS      ((CUDA_MWC ^ CUDA_CO`

### CUDA_MWC {#cudamwc}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUDA_MWC  ((CUDA_ZNEW << 16) + CUDA_WNEW)
#define CUDA_SHR3                            \
   `

### CUDA_SHR3 {#cudashr3}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUDA_SHR3                            \
    (cuda_jsr = cuda_jsr ^ (cuda_jsr << 17), \
     c`

### CUDA_WNEW {#cudawnew}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUDA_WNEW (cuda_w = 18000 * (cuda_w & 65535) + (cuda_w >> 16))
#define CUDA_MWC  ((CUDA_ZNEW`

### CUDA_ZNEW {#cudaznew}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define CUDA_ZNEW (cuda_z = 36969 * (cuda_z & 65535) + (cuda_z >> 16))
#define CUDA_WNEW (cuda_w = 1`


## D

### DEV_VER_ALL_SUPPORT {#devverallsupport}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define DEV_VER_ALL_SUPPORT (999)

/* Errors Tests to be returned by all the Cublas test */
#define `

### DEV_VER_DBL_SUPPORT {#devverdblsupport}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define DEV_VER_DBL_SUPPORT (130)
#define DEV_VER_ALL_SUPPORT (999)

/* Errors Tests to be returned `


## G

### GetTickCount {#gettickcount}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `     return (double)GetTickCount() / 1000.0;
    }
}`


## K

### KISS {#kiss}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define KISS      ((CUDA_MWC ^ CUDA_CONG) + CUDA_SHR3)
    static unsigned int cuda_z = 362436069, c`


## Q

### QuadPart {#quadpart}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `  = 1.0 / (double)t.QuadPart;
        checkedFor`

### QueryPerformanceCounter {#queryperformancecounter}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `ResTimer) {
        QueryPerformanceCounter(&t);
        return`

### QueryPerformanceFrequency {#queryperformancefrequency}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `ghResTimer        = QueryPerformanceFrequency(&t);
        oofreq`


## R

### REFFUNC {#reffunc}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define REFFUNC(funcname)    ref_##funcname
#define TESTGEN(funcname)    get_##funcname##_params
#de`


## S

### SWITCH_CHAR {#switchchar}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define SWITCH_CHAR '-'

#define REFFUNC(funcname)    ref_##funcname
#define TESTGEN(funcname)    ge`


## T

### TESTGEN {#testgen}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define TESTGEN(funcname)    get_##funcname##_params
#define TESTPARAMS(funcname) funcname##TestPara`

### TESTPARAMS {#testparams}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define TESTPARAMS(funcname) funcname##TestParams

#define DEV_VER_DBL_SUPPORT (130)
#define DEV_VER`

### TestParams {#testparams}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `funcname) funcname##TestParams

#define DEV_VER_DB`


## W

### WIN32_LEAN_AND_MEAN {#win32leanandmean}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `#define WIN32_LEAN_AND_MEAN
#endif
#include <windows.h>
static __inline__ double second(void)
{
    `


## C

### cuEqual {#cuequal}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `bool                       cuEqual(double x, double y) {`

### cuRand {#curand}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `unsigned cuRand(void)
{`


## D

### doubleAsULL {#doubleasull}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `long doubleAsULL(double x)
{`


## F

### floatAsUInt {#floatasuint}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `unsigned floatAsUInt(float x)
{`


## I

### imax {#imax}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `int imax(int x, int y) {`


## S

### second {#second}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `double second(void)
{`


## T

### timeval {#timeval}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.h](./batchCUBLAS.h_docs.md)
- **Context**: `struct timeval`

