# Keywords: Samples/2_Concepts_and_Techniques/histogram/histogram_common.h
---

**Total Keywords**: 13

---

## H

### HISTOGRAM256_BIN_COUNT {#histogram256bincount}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define HISTOGRAM256_BIN_COUNT 256
#define UINT_BITS              32
typedef unsigned int  uint;
typ`

### HISTOGRAM256_THREADBLOCK_MEMORY {#histogram256threadblockmemory}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define HISTOGRAM256_THREADBLOCK_MEMORY (WARP_COUNT * HISTOGRAM256_BIN_COUNT)

#define UMUL(a, b)   `

### HISTOGRAM256_THREADBLOCK_SIZE {#histogram256threadblocksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define HISTOGRAM256_THREADBLOCK_SIZE (WARP_COUNT * WARP_SIZE)

// Shared memory per threadblock
#de`

### HISTOGRAM64_BIN_COUNT {#histogram64bincount}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define HISTOGRAM64_BIN_COUNT  64
#define HISTOGRAM256_BIN_COUNT 256
#define UINT_BITS              `

### HISTOGRAM64_THREADBLOCK_SIZE {#histogram64threadblocksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define HISTOGRAM64_THREADBLOCK_SIZE (4 * SHARED_MEMORY_BANKS)

// Warps ==subhistograms per threadb`

### HISTOGRAM_COMMON_H {#histogramcommonh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define HISTOGRAM_COMMON_H

////////////////////////////////////////////////////////////////////////`


## L

### LOG2_WARP_SIZE {#log2warpsize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define LOG2_WARP_SIZE 5U
#define WARP_SIZE      (1U << LOG2_WARP_SIZE)

// May change on future har`


## S

### SHARED_MEMORY_BANKS {#sharedmemorybanks}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define SHARED_MEMORY_BANKS 16

// Threadblock size: must be a multiple of (4 * SHARED_MEMORY_BANKS)`


## U

### UINT_BITS {#uintbits}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define UINT_BITS              32
typedef unsigned int  uint;
typedef unsigned char uchar;

////////`

### UMAD {#umad}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define UMAD(a, b, c) (UMUL((a), (b)) + (c))

//////////////////////////////////////////////////////`

### UMUL {#umul}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define UMUL(a, b)    ((a) * (b))
#define UMAD(a, b, c) (UMUL((a), (b)) + (c))

////////////////////`


## W

### WARP_COUNT {#warpcount}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define WARP_COUNT 6

// Threadblock size
#define HISTOGRAM256_THREADBLOCK_SIZE (WARP_COUNT * WARP_S`

### WARP_SIZE {#warpsize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram_common.h](./histogram_common.h_docs.md)
- **Context**: `#define WARP_SIZE      (1U << LOG2_WARP_SIZE)

// May change on future hardware, so better parametri`

