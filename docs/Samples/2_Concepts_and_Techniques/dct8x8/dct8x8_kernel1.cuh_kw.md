# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh
---

**Total Keywords**: 10

---

## C

### CUDAkernel1DCT {#cudakernel1dct}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `__global__ void
CUDAkernel1DCT(`

### CUDAkernel1IDCT {#cudakernel1idct}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `__global__ void
CUDAkernel1IDCT(`

### CurBlockLocal1 {#curblocklocal1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `ks
__shared__ float CurBlockLocal1[BLOCK_SIZE2];
__sha`

### CurBlockLocal1Index {#curblocklocal1index}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `IZE + ty;
    int   CurBlockLocal1Index = 0 * BLOCK_SIZE + `

### CurBlockLocal2 {#curblocklocal2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `];
__shared__ float CurBlockLocal2[BLOCK_SIZE2];

/**
`

### CurBlockLocal2Index {#curblocklocal2index}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `       = 0;
    int CurBlockLocal2Index = (ty << BLOCK_SIZE`


## I

### ImgWidth {#imgwidth}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `ents plane
* \param ImgWidth       [IN] - Stride`


## O

### OffsetXBlocks {#offsetxblocks}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `ide of Dst
* \param OffsetXBlocks  [IN] - Offset alon`

### OffsetYBlocks {#offsetyblocks}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `processing
* \param OffsetYBlocks  [IN] - Offset alon`


## T

### TexSrc {#texsrc}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel1.cuh](./dct8x8_kernel1.cuh_docs.md)
- **Context**: `cudaTextureObject_t TexSrc)
{
    // Handle to`

