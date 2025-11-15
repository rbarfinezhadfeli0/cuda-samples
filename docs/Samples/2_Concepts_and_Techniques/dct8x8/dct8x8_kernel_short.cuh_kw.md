# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh
---

**Total Keywords**: 35

---

## C

### COS_1_4 {#cos14}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define COS_1_4 0x5A82
#define SIN_1_8 0x30FC
#define COS_1_8 0x7642

#define OSIN_1_16 0x063E
#defi`

### COS_1_8 {#cos18}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define COS_1_8 0x7642

#define OSIN_1_16 0x063E
#define OSIN_3_16 0x11C7
#define OSIN_5_16 0x1A9B
#`

### CUDAkernelShortDCT {#cudakernelshortdct}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `__global__ void CUDAkernelShortDCT(`

### CUDAkernelShortIDCT {#cudakernelshortidct}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `__global__ void CUDAkernelShortIDCT(`

### CUDAshortInplaceDCT {#cudashortinplacedct}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `void CUDAshortInplaceDCT(unsigned int *V8)
{`

### CUDAshortInplaceIDCT {#cudashortinplaceidct}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `void CUDAshortInplaceIDCT(unsigned int *V8)
{`


## D

### DoubleStride {#doublestride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `25, tmp26;

    int DoubleStride = Stride << 1;

   `

### DstPtr {#dstptr}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `e << 1;

    short *DstPtr = SrcDst;
    in0  `


## I

### IMAD {#imad}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define IMAD(a, b, c) (((a) * (b)) + (c))
#define IMUL(a, b)    ((a) * (b))

__global__ void CUDAker`

### IMUL {#imul}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define IMUL(a, b)    ((a) * (b))

__global__ void CUDAkernelShortDCT(short *SrcDst, int ImgStride)
`

### ImgStride {#imgstride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `ents plane
* \param ImgStride                  [I`


## K

### KERS_BH_LOG2 {#kersbhlog2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define KERS_BH_LOG2 5

/**
 *  Stride of shared memory buffer (short kernel)
 */
#define KERS_SMEMB`

### KERS_BLOCK_HEIGHT {#kersblockheight}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define KERS_BLOCK_HEIGHT 32

/**
 *  LOG2 of width of data block (short kernel)
 */
#define KERS_BW`

### KERS_BLOCK_WIDTH {#kersblockwidth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define KERS_BLOCK_WIDTH 32

/**
 *  Height of data block (short kernel)
 */
#define KERS_BLOCK_HEIG`

### KERS_BLOCK_WIDTH_HALF {#kersblockwidthhalf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define KERS_BLOCK_WIDTH_HALF (KERS_BLOCK_WIDTH / 2)

#define SIN_1_4 0x5A82
#define COS_1_4 0x5A82
`

### KERS_BW_LOG2 {#kersbwlog2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define KERS_BW_LOG2 5

/**
 *  LOG2 of height of data block (short kernel)
 */
#define KERS_BH_LOG2`

### KERS_SMEMBLOCK_STRIDE {#kerssmemblockstride}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define KERS_SMEMBLOCK_STRIDE (KERS_BLOCK_WIDTH + 2)

/**
 *  Half of data block width (short kernel`


## O

### OCOS_1_16 {#ocos116}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OCOS_1_16 0x1F63
#define OCOS_3_16 0x1A9B
#define OCOS_5_16 0x11C7
#define OCOS_7_16 0x063E
`

### OCOS_3_16 {#ocos316}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OCOS_3_16 0x1A9B
#define OCOS_5_16 0x11C7
#define OCOS_7_16 0x063E

/**
 *  Package of 2 sho`

### OCOS_5_16 {#ocos516}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OCOS_5_16 0x11C7
#define OCOS_7_16 0x063E

/**
 *  Package of 2 shorts into 1 int - designed`

### OCOS_7_16 {#ocos716}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OCOS_7_16 0x063E

/**
 *  Package of 2 shorts into 1 int - designed to perform i/o by intege`

### OSIN_1_16 {#osin116}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OSIN_1_16 0x063E
#define OSIN_3_16 0x11C7
#define OSIN_5_16 0x1A9B
#define OSIN_7_16 0x1F63
`

### OSIN_3_16 {#osin316}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OSIN_3_16 0x11C7
#define OSIN_5_16 0x1A9B
#define OSIN_7_16 0x1F63

#define OCOS_1_16 0x1F63`

### OSIN_5_16 {#osin516}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OSIN_5_16 0x1A9B
#define OSIN_7_16 0x1F63

#define OCOS_1_16 0x1F63
#define OCOS_3_16 0x1A9B`

### OSIN_7_16 {#osin716}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define OSIN_7_16 0x1F63

#define OCOS_1_16 0x1F63
#define OCOS_3_16 0x1A9B
#define OCOS_5_16 0x11C7`

### OffsThrRowPermuted {#offsthrrowpermuted}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `   int              OffsThrRowPermuted =
        (OffsThre`

### OffsThreadInCol {#offsthreadincol}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `   int              OffsThreadInCol = FMUL(threadIdx.z,`

### OffsThreadInRow {#offsthreadinrow}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `   int              OffsThreadInRow = FMUL(threadIdx.y,`


## P

### PackedShorts {#packedshorts}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `conflicts
 */
union PackedShorts
{
    struct __alig`


## S

### SIN_1_4 {#sin14}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define SIN_1_4 0x5A82
#define COS_1_4 0x5A82
#define SIN_1_8 0x30FC
#define COS_1_8 0x7642

#define`

### SIN_1_8 {#sin18}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `#define SIN_1_8 0x30FC
#define COS_1_8 0x7642

#define OSIN_1_16 0x063E
#define OSIN_3_16 0x11C7
#de`

### SrcDst {#srcdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `memory).
*
* \param SrcDst         [IN/OUT] - `


## _

### __align__ {#align}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `struct __align__`


## U

### unfixh {#unfixh}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `short unfixh(int x) {`

### unfixo {#unfixo}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel_short.cuh](./dct8x8_kernel_short.cuh_docs.md)
- **Context**: `int unfixo(int x) {`

