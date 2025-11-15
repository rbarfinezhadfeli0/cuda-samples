# Keywords: Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu
---

**Total Keywords**: 11

---

## I

### IMAD {#imad}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `#define IMAD(a, b, c) (__mul24((a), (b)) + (c))

// Use unrolled innermost convolution loop
#define `


## U

### UNROLL_INNER {#unrollinner}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `#define UNROLL_INNER 1

// Round a / b to nearest higher integer value
inline int iDivUp(int a, int `


## C

### convolutionColumn {#convolutioncolumn}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `float convolutionColumn(float x, float y, cudaTextureObject_t texSrc)
{`

### convolutionColumnsGPU {#convolutioncolumnsgpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `void
convolutionColumnsGPU(float *d_Dst, cudaArray *a_Src, int imageW, int imageH, cudaTextureObject`

### convolutionColumnsKernel {#convolutioncolumnskernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `__global__ void convolutionColumnsKernel(`

### convolutionRow {#convolutionrow}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `float convolutionRow(float x, float y, cudaTextureObject_t texSrc)
{`

### convolutionRowsGPU {#convolutionrowsgpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `void convolutionRowsGPU(float *d_Dst, cudaArray *a_Src, int imageW, int imageH, cudaTextureObject_t `

### convolutionRowsKernel {#convolutionrowskernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `__global__ void convolutionRowsKernel(`


## I

### iAlignUp {#ialignup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `int iAlignUp(int a, int b) {`

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `int iDivUp(int a, int b) {`


## S

### setConvolutionKernel {#setconvolutionkernel}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionTexture/convolutionTexture.cu](./convolutionTexture.cu_docs.md)
- **Context**: `void setConvolutionKernel(float *h_Kernel)
{`

