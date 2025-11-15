# Keywords: Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu
---

**Total Keywords**: 16

---

## E

### ENABLE_CPU_DEBUG_CODE {#enablecpudebugcode}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `#define ENABLE_CPU_DEBUG_CODE 0
#define THREADS_PER_BLOCK     512

/* genTridiag: generate a random `


## T

### THREADS_PER_BLOCK {#threadsperblock}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `#define THREADS_PER_BLOCK     512

/* genTridiag: generate a random tridiagonal symmetric matrix */
`


## A

### areAlmostEqual {#arealmostequal}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `bool areAlmostEqual(float a, float b, float maxRelDiff)
{`


## C

### cpuConjugateGrad {#cpuconjugategrad}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void cpuConjugateGrad(int *I, int *J, float *val, float *x, float *Ax, float *p, float *r, int nnz, `

### cpuSpMV {#cpuspmv}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void cpuSpMV(int *I, int *J, float *val, int nnz, int num_rows, float alpha, float *inputVecX, float`


## D

### dotProduct {#dotproduct}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `double dotProduct(float *vecA, float *vecB, int size)
{`


## G

### genTridiag {#gentridiag}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void genTridiag(int *I, int *J, float *val, int N, int nz)
{`

### gpuConjugateGradient {#gpuconjugategradient}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `__global__ void gpuConjugateGradient(`

### gpuCopyVector {#gpucopyvector}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void gpuCopyVector(float *srcA, float *destB, int size, const cg::grid_group &grid)
{`

### gpuDotProduct {#gpudotproduct}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void gpuDotProduct(float                  *vecA,
                              float                `

### gpuSaxpy {#gpusaxpy}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void gpuSaxpy(float *x, float *y, float a, int size, const cg::grid_group &grid)
{`

### gpuScaleVectorAndSaxpy {#gpuscalevectorandsaxpy}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void
gpuScaleVectorAndSaxpy(const float *x, float *y, float a, float scale, int size, const cg::grid`

### gpuSpMV {#gpuspmv}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void gpuSpMV(int                  *I,
                        int                  *J,
             `


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## S

### saxpy {#saxpy}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void saxpy(float *x, float *y, float a, int size)
{`

### scaleVector {#scalevector}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiBlockCG/conjugateGradientMultiBlockCG.cu](./conjugateGradientMultiBlockCG.cu_docs.md)
- **Context**: `void scaleVector(float *vec, float alpha, int size)
{`

