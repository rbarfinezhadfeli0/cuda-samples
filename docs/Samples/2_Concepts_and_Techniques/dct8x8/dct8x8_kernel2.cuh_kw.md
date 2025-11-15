# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh
---

**Total Keywords**: 20

---

## C

### CUDAkernel2DCT {#cudakernel2dct}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `__global__ void CUDAkernel2DCT(`

### CUDAkernel2IDCT {#cudakernel2idct}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `__global__ void CUDAkernel2IDCT(`

### CUDAsubroutineInplaceDCTvector {#cudasubroutineinplacedctvector}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `void CUDAsubroutineInplaceDCTvector(float *Vect0, int Step)
{`

### CUDAsubroutineInplaceIDCTvector {#cudasubroutineinplaceidctvector}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `void CUDAsubroutineInplaceIDCTvector(float *Vect0, int Step)
{`

### C_a {#ca}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_a 1.387039845322148f //!< a = (2^0.5) * cos(    pi / 16);
#define C_b 1.306562964876377f /`

### C_b {#cb}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_b 1.306562964876377f //!< b = (2^0.5) * cos(    pi /  8);
#define C_c 1.175875602419359f /`

### C_c {#cc}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_c 1.175875602419359f //!< c = (2^0.5) * cos(3 * pi / 16);
#define C_d 0.785694958387102f /`

### C_d {#cd}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_d 0.785694958387102f //!< d = (2^0.5) * cos(5 * pi / 16);
#define C_e 0.541196100146197f /`

### C_e {#ce}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_e 0.541196100146197f //!< e = (2^0.5) * cos(3 * pi /  8);
#define C_f 0.275899379282943f /`

### C_f {#cf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_f 0.275899379282943f //!< f = (2^0.5) * cos(7 * pi / 16);

/**
 *  Normalization constant `

### C_norm {#cnorm}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define C_norm 0.3535533905932737f // 1 / (8^0.5)

/**
 *  Width of data block (2nd kernel)
 */
#def`


## I

### ImgStride {#imgstride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `ents plane
* \param ImgStride                  [I`


## K

### KER2_BH_LOG2 {#ker2bhlog2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define KER2_BH_LOG2 4

/**
 *  Stride of shared memory buffer (2nd kernel)
 */
#define KER2_SMEMBLO`

### KER2_BLOCK_HEIGHT {#ker2blockheight}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define KER2_BLOCK_HEIGHT 16

/**
 *  LOG2 of width of data block (2nd kernel)
 */
#define KER2_BW_L`

### KER2_BLOCK_WIDTH {#ker2blockwidth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define KER2_BLOCK_WIDTH 32

/**
 *  Height of data block (2nd kernel)
 */
#define KER2_BLOCK_HEIGHT`

### KER2_BW_LOG2 {#ker2bwlog2}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define KER2_BW_LOG2 5

/**
 *  LOG2 of height of data block (2nd kernel)
 */
#define KER2_BH_LOG2 4`

### KER2_SMEMBLOCK_STRIDE {#ker2smemblockstride}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `#define KER2_SMEMBLOCK_STRIDE (KER2_BLOCK_WIDTH + 1)

/**
******************************************`


## O

### OffsThreadInCol {#offsthreadincol}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `hreadIdx.x;
    int OffsThreadInCol = threadIdx.z * BLO`

### OffsThreadInRow {#offsthreadinrow}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `K_STRIDE];

    int OffsThreadInRow = threadIdx.y * BLO`


## S

### SrcDst {#srcdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8_kernel2.cuh](./dct8x8_kernel2.cuh_docs.md)
- **Context**: `lock8x8.
*
* \param SrcDst                    `

