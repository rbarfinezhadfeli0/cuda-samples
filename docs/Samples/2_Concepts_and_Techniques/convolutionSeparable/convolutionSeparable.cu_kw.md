# Keywords: Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu
---

**Total Keywords**: 13

---

## C

### COLUMNS_BLOCKDIM_X {#columnsblockdimx}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define COLUMNS_BLOCKDIM_X   16
#define COLUMNS_BLOCKDIM_Y   8
#define COLUMNS_RESULT_STEPS 8
#defin`

### COLUMNS_BLOCKDIM_Y {#columnsblockdimy}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define COLUMNS_BLOCKDIM_Y   8
#define COLUMNS_RESULT_STEPS 8
#define COLUMNS_HALO_STEPS   1

__glob`

### COLUMNS_HALO_STEPS {#columnshalosteps}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define COLUMNS_HALO_STEPS   1

__global__ void convolutionColumnsKernel(float *d_Dst, float *d_Src,`

### COLUMNS_RESULT_STEPS {#columnsresultsteps}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define COLUMNS_RESULT_STEPS 8
#define COLUMNS_HALO_STEPS   1

__global__ void convolutionColumnsKer`


## R

### ROWS_BLOCKDIM_X {#rowsblockdimx}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define ROWS_BLOCKDIM_X   16
#define ROWS_BLOCKDIM_Y   4
#define ROWS_RESULT_STEPS 8
#define ROWS_HA`

### ROWS_BLOCKDIM_Y {#rowsblockdimy}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define ROWS_BLOCKDIM_Y   4
#define ROWS_RESULT_STEPS 8
#define ROWS_HALO_STEPS   1

__global__ void`

### ROWS_HALO_STEPS {#rowshalosteps}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define ROWS_HALO_STEPS   1

__global__ void convolutionRowsKernel(float *d_Dst, float *d_Src, int i`

### ROWS_RESULT_STEPS {#rowsresultsteps}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `#define ROWS_RESULT_STEPS 8
#define ROWS_HALO_STEPS   1

__global__ void convolutionRowsKernel(float`


## C

### convolutionColumnsGPU {#convolutioncolumnsgpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `void convolutionColumnsGPU(float *d_Dst, float *d_Src, int imageW, int imageH)
{`

### convolutionColumnsKernel {#convolutioncolumnskernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `__global__ void convolutionColumnsKernel(`

### convolutionRowsGPU {#convolutionrowsgpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `void convolutionRowsGPU(float *d_Dst, float *d_Src, int imageW, int imageH)
{`

### convolutionRowsKernel {#convolutionrowskernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `__global__ void convolutionRowsKernel(`


## S

### setConvolutionKernel {#setconvolutionkernel}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable.cu](./convolutionSeparable.cu_docs.md)
- **Context**: `void setConvolutionKernel(float *h_Kernel)
{`

