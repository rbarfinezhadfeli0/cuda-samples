# Keywords: Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h
---

**Total Keywords**: 11

---

## B

### BITONICSORT_LEN {#bitonicsortlen}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `#define BITONICSORT_LEN       1024 // Must be power of 2!
#define QSORT_MAXDEPTH        16   // Will`


## Q

### QSORT_BLOCKSIZE {#qsortblocksize}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `#define QSORT_BLOCKSIZE       (1 << QSORT_BLOCKSIZE_SHIFT)
#define BITONICSORT_LEN       1024 // Mus`

### QSORT_BLOCKSIZE_SHIFT {#qsortblocksizeshift}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `#define QSORT_BLOCKSIZE_SHIFT 9
#define QSORT_BLOCKSIZE       (1 << QSORT_BLOCKSIZE_SHIFT)
#define B`

### QSORT_MAXDEPTH {#qsortmaxdepth}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `#define QSORT_MAXDEPTH        16   // Will force final bitonic stage at depth QSORT_MAXDEPTH+1

////`

### QSORT_STACK_ELEMS {#qsortstackelems}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `#define QSORT_STACK_ELEMS 1 * 1024 * 1024 // One million stack elements is a HUGE number.

__global_`

### QUICKSORT_H {#quicksorth}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `#define QUICKSORT_H

#define QSORT_BLOCKSIZE_SHIFT 9
#define QSORT_BLOCKSIZE       (1 << QSORT_BLOCK`


## _

### __align__ {#align}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `struct __align__`


## B

### big_bitonicsort {#bigbitonicsort}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `__global__ void
big_bitonicsort(`

### bitonicsort {#bitonicsort}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `__global__ void bitonicsort(`


## Q

### qsortRingbuf_t {#qsortringbuft}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `struct qsortRingbuf_t`

### qsort_warp {#qsortwarp}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpAdvancedQuicksort/cdpQuicksort.h](./cdpQuicksort.h_docs.md)
- **Context**: `__global__ void qsort_warp(`

