# Keywords: Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh
---

**Total Keywords**: 5

---

## C

### ColorDistance {#colordistance}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh](./imageDenoising_nlm2_kernel.cuh_docs.md)
- **Context**: ` pixel around which ColorDistance is
// computed
// T`


## N

### NLM2 {#nlm2}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh](./imageDenoising_nlm2_kernel.cuh_docs.md)
- **Context**: `__global__ void NLM2(`

### NLM2diag {#nlm2diag}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh](./imageDenoising_nlm2_kernel.cuh_docs.md)
- **Context**: `__global__ void NLM2diag(`


## C

### cuda_NLM2 {#cudanlm2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh](./imageDenoising_nlm2_kernel.cuh_docs.md)
- **Context**: `void cuda_NLM2(TColor *d_dst, int imageW, int imageH, float Noise, float LerpC, cudaTextureObject_t `

### cuda_NLM2diag {#cudanlm2diag}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising_nlm2_kernel.cuh](./imageDenoising_nlm2_kernel.cuh_docs.md)
- **Context**: `void
cuda_NLM2diag(TColor *d_dst, int imageW, int imageH, float Noise, float LerpC, cudaTextureObjec`

