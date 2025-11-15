# Keywords: Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp
---

**Total Keywords**: 23

---

## C

### CLEANUP_ON_ERROR {#cleanuponerror}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define CLEANUP_ON_ERROR(dptr, hcuModule, hcuContext, status) \
    if (dptr)                       `

### CreateThread {#createthread}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `eads[ThreadIndex] = CreateThread(NULL,
             `


## E

### ENTERCRITICALSECTION {#entercriticalsection}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define ENTERCRITICALSECTION pthread_mutex_lock(&g_mutex);
#define LEAVECRITICALSECTION pthread_mute`

### EnterCriticalSection {#entercriticalsection}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `NTERCRITICALSECTION EnterCriticalSection(&g_cs);
#define LEA`


## F

### FATBIN_FILE {#fatbinfile}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define FATBIN_FILE "threadMigration_kernel64.fatbin"
#endif

bool gbAutoQuit = false;

////////////`

### FinalErrorCheck {#finalerrorcheck}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `bool FinalErrorCheck(CUDAContext *pContext, int NumThreads, int deviceCount)
{`


## I

### InitCUDAContext {#initcudacontext}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `CUresult InitCUDAContext(CUDAContext *pContext, CUdevice hcuDevice, int deviceID, char **argv)
{`

### InitializeCriticalSection {#initializecriticalsection}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `defined(_WIN64)
    InitializeCriticalSection(&g_cs);
#else
    p`


## L

### LEAVECRITICALSECTION {#leavecriticalsection}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define LEAVECRITICALSECTION pthread_mutex_unlock(&g_mutex);
#define STRICMP              strcasecmp`

### LeaveCriticalSection {#leavecriticalsection}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `EAVECRITICALSECTION LeaveCriticalSection(&g_cs);
#define STR`


## M

### MAXTHREADS {#maxthreads}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define MAXTHREADS 256
#define NUM_INTS   32

#if defined(WIN32) || defined(_WIN32) || defined(WIN64`


## N

### NUM_INTS {#numints}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define NUM_INTS   32

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
//`

### NumThreads {#numthreads}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `namespace std;

int NumThreads;
int ThreadLaunchCo`


## S

### STRICMP {#stricmp}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define STRICMP              strcasecmp
#endif

#include <cstring>
#include <cuda.h>
#include <cuda_`


## T

### THREAD_QUIT {#threadquit}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `#define THREAD_QUIT    \
    printf("Error\n"); \
    return 0;

// This sample uses the Driver API `

### ThreadIndex {#threadindex}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `ead    = 0;
    int ThreadIndex = 0;

    CUDAConte`

### ThreadLaunchCount {#threadlaunchcount}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `int NumThreads;
int ThreadLaunchCount;

typedef struct _C`

### ThreadLaunchCounts {#threadlaunchcounts}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `cted=%d, Actual=%d> ThreadLaunchCounts(s)\n", NumThreads *`

### ThreadProc {#threadproc}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `CUDA_SUCCESS;
}

// ThreadProc launches the CUDA k`


## W

### WaitForMultipleObjects {#waitformultipleobjects}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `defined(_WIN64)
    WaitForMultipleObjects(ThreadIndex, rghThr`


## _

### _CUDAContext_st {#cudacontextst}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `struct _CUDAContext_st`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadMigration/threadMigration.cpp](./threadMigration.cpp_docs.md)
- **Context**: `bool runTest(int argc, char **argv)
{`

