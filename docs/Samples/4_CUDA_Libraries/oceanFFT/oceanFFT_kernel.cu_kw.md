# Keywords: Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu
---

**Total Keywords**: 12

---

## C

### calculateSlopeKernel {#calculateslopekernel}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `__global__ void calculateSlopeKernel(`

### complex_add {#complexadd}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `float2 complex_add(float2 a, float2 b) {`

### complex_exp {#complexexp}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `float2 complex_exp(float arg) {`

### complex_mult {#complexmult}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `float2 complex_mult(float2 ab, float2 cd)
{`

### conjugate {#conjugate}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `float2 conjugate(float2 arg) {`

### cudaCalculateSlopeKernel {#cudacalculateslopekernel}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `void cudaCalculateSlopeKernel(float *hptr, float2 *slopeOut, unsigned int width, unsigned int height`

### cudaGenerateSpectrumKernel {#cudageneratespectrumkernel}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `void cudaGenerateSpectrumKernel(float2      *d_h0,
                                           float2`

### cudaUpdateHeightmapKernel {#cudaupdateheightmapkernel}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `void
cudaUpdateHeightmapKernel(float *d_heightMap, float2 *d_ht, unsigned int width, unsigned int he`

### cuda_iDivUp {#cudaidivup}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `int cuda_iDivUp(int a, int b) {`


## G

### generateSpectrumKernel {#generatespectrumkernel}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `__global__ void generateSpectrumKernel(`


## U

### updateHeightmapKernel {#updateheightmapkernel}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `__global__ void updateHeightmapKernel(`

### updateHeightmapKernel_y {#updateheightmapkernely}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT_kernel.cu](./oceanFFT_kernel.cu_docs.md)
- **Context**: `__global__ void updateHeightmapKernel_y(`

