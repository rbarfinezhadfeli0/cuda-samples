# Keywords: Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu
---

**Total Keywords**: 13

---

## C

### CUDA_DRIVER_API {#cudadriverapi}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `#define CUDA_DRIVER_API 1
#include "helper_cuda.h"
#include "helper_cuda_drvapi.h"
#include "helper_`

### CanAccessPeer {#canaccesspeer}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `remove devices from CanAccessPeer
            for (in`


## D

### DATA_SIZE {#datasize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `#define DATA_SIZE   (64ULL << 20ULL) // 64MB

#if defined(__linux__)
#define cpu_atomic_add32(a, x) `


## I

### InterlockedAdd {#interlockedadd}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `_atomic_add32(a, x) InterlockedAdd((volatile LONG *)a,`


## M

### MAX_DEVICES {#maxdevices}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `#define MAX_DEVICES (32)
#define DATA_SIZE   (64ULL << 20ULL) // 64MB

#if defined(__linux__)
#defin`


## S

### ShareableHandle {#shareablehandle}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `nt.
    std::vector<ShareableHandle> shHandle(shm->npro`


## B

### barrierWait {#barrierwait}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `void barrierWait(volatile int *barrier, volatile int *sense, unsigned int n)
{`


## C

### childProcess {#childprocess}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `void childProcess(int id)
{`

### cpu_atomic_add32 {#cpuatomicadd32}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `#define cpu_atomic_add32(a, x) InterlockedAdd((volatile LONG *)a, x)
#else
#error Unsupported system`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## P

### parentProcess {#parentprocess}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `void parentProcess(char *app)
{`


## S

### shmStruct_st {#shmstructst}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `struct shmStruct_st`

### simpleKernel {#simplekernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/streamOrderedAllocationIPC/streamOrderedAllocationIPC.cu](./streamOrderedAllocationIPC.cu_docs.md)
- **Context**: `__global__ void simpleKernel(`

