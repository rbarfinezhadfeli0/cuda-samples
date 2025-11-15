# Keywords: Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh
---

**Total Keywords**: 8

---

## _

### _BISECT_KERNEL_LARGE_H_ {#bisectkernellargeh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `#define _BISECT_KERNEL_LARGE_H_
#include <cooperative_groups.h>

namespace cg = cooperative_groups;
`


## B

### bisectKernelLarge {#bisectkernellarge}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `__global__ void bisectKernelLarge(`


## C

### compactStreamsFinal {#compactstreamsfinal}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `void compactStreamsFinal(const unsigned int tid,
                                    const unsigned `


## S

### scanCompactBlocksStartAddress {#scancompactblocksstartaddress}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `void scanCompactBlocksStartAddress(const unsigned int tid,
                                         `

### scanInitial {#scaninitial}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `void scanInitial(const unsigned int tid,
                            const unsigned int tid_2,
     `

### scanSumBlocks {#scansumblocks}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `void scanSumBlocks(const unsigned int tid,
                              const unsigned int tid_2,
 `

### storeNonEmptyIntervalsLarge {#storenonemptyintervalslarge}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `void storeNonEmptyIntervalsLarge(unsigned int         addr,
                                        `


## W

### writeToGmem {#writetogmem}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large.cuh](./bisect_kernel_large.cuh_docs.md)
- **Context**: `void writeToGmem(const unsigned int tid,
                            const unsigned int tid_2,
     `

