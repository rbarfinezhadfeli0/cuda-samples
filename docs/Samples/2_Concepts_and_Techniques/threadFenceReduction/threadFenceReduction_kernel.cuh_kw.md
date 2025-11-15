# Keywords: Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh
---

**Total Keywords**: 8

---

## _

### _REDUCE_KERNEL_H_ {#reducekernelh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `#define _REDUCE_KERNEL_H_

#include <cooperative_groups.h>
#include <cuda_runtime_api.h>

namespace `


## I

### isPow2 {#ispow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `bool isPow2(unsigned int x) {`


## R

### reduce {#reduce}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `void reduce(int size, int threads, int blocks, float *d_idata, float *d_odata)
{`

### reduceBlock {#reduceblock}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `void reduceBlock(volatile float *sdata, float mySum, const unsigned int tid, cg::thread_block cta)
{`

### reduceBlocks {#reduceblocks}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `void reduceBlocks(const float *g_idata, float *g_odata, unsigned int n, cg::thread_block cta)
{`

### reduceMultiPass {#reducemultipass}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `__global__ void reduceMultiPass(`

### reduceSinglePass {#reducesinglepass}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `__global__ void reduceSinglePass(`


## S

### setRetirementCount {#setretirementcount}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction_kernel.cuh](./threadFenceReduction_kernel.cuh_docs.md)
- **Context**: `cudaError_t setRetirementCount(int retCnt)
{`

