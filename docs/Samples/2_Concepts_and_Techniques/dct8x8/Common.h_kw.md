# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/Common.h
---

**Total Keywords**: 7

---

## B

### BLOCK_SIZE {#blocksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define BLOCK_SIZE 8

/**
 *  Square of dimension of pixels block
 */
#define BLOCK_SIZE2 64

/**
 *`

### BLOCK_SIZE2 {#blocksize2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define BLOCK_SIZE2 64

/**
 *  log_2{BLOCK_SIZE), used for quick multiplication or division by the
`

### BLOCK_SIZE2_LOG2 {#blocksize2log2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define BLOCK_SIZE2_LOG2 6

/**
 *  This macro states that __mul24 operation is performed faster tha`

### BLOCK_SIZE_LOG2 {#blocksizelog2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define BLOCK_SIZE_LOG2 3

/**
 *  log_2{BLOCK_SIZE*BLOCK_SIZE), used for quick multiplication or di`


## F

### FMUL {#fmul}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define FMUL(x, y) ((x) * (y))
#endif

/**
 *  This macro allows using aligned memory management
 */`


## _

### __ALLOW_ALIGNED_MEMORY_MANAGEMENT {#allowalignedmemorymanagement}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define __ALLOW_ALIGNED_MEMORY_MANAGEMENT
`

### __MUL24_FASTER_THAN_ASTERIX {#mul24fasterthanasterix}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/Common.h](./Common.h_docs.md)
- **Context**: `#define __MUL24_FASTER_THAN_ASTERIX

/**
 *  Wrapper to the fastest integer multiplication function `

