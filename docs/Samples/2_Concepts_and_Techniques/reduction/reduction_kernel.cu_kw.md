# Keywords: Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu
---

**Total Keywords**: 19

---

## B

### BlockSize {#blocksize}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `te <class T, size_t BlockSize, size_t MultiWarpGr`


## M

### MultiWarpGroupSize {#multiwarpgroupsize}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `t BlockSize, size_t MultiWarpGroupSize>
__global__ void mu`


## S

### SharedMemory {#sharedmemory}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `struct SharedMemory`


## _

### _REDUCE_KERNEL_H_ {#reducekernelh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `#define _REDUCE_KERNEL_H_

#include <cooperative_groups.h>
#include <cooperative_groups/reduce.h>
#i`


## C

### cg_reduce {#cgreduce}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void cg_reduce(`

### cg_reduce_n {#cgreducen}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `T cg_reduce_n(T in, Group &threads)
{`


## M

### multi_warp_cg_reduce {#multiwarpcgreduce}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void multi_warp_cg_reduce(`


## R

### reduce {#reduce}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `void reduce(int size, int threads, int blocks, int whichKernel, T *d_idata, T *d_odata)
{`

### reduce0 {#reduce0}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce0(`

### reduce1 {#reduce1}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce1(`

### reduce2 {#reduce2}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce2(`

### reduce3 {#reduce3}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce3(`

### reduce4 {#reduce4}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce4(`

### reduce5 {#reduce5}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce5(`

### reduce6 {#reduce6}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce6(`

### reduce7 {#reduce7}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `__global__ void reduce7(`


## S

### switch {#switch}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `launch
    switch (whichKernel) {`


## U

### used {#used}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `class used`


## W

### warpReduceSum {#warpreducesum}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction_kernel.cu](./reduction_kernel.cu_docs.md)
- **Context**: `T warpReduceSum(unsigned int mask, T mySum)
{`

