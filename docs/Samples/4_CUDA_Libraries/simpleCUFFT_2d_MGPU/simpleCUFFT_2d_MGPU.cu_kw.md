# Keywords: Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu
---

**Total Keywords**: 8

---

## M

### MakePlan {#makeplan}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: ` {
        printf("*MakePlan* failed\n");
      `


## X

### XtExecC2C {#xtexecc2c}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: ` {
        printf("*XtExecC2C  failed\n");
      `

### XtFree {#xtfree}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: ` {
        printf("*XtFree failed\n");
       `

### XtMalloc {#xtmalloc}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: ` {
        printf("*XtMalloc failed\n");
       `

### XtMemcpy {#xtmemcpy}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: ` {
        printf("*XtMemcpy failed\n");
       `


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## S

### solvePoisson {#solvepoisson}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: `__global__ void solvePoisson(`

### solvePoissonEquation {#solvepoissonequation}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu](./simpleCUFFT_2d_MGPU.cu_docs.md)
- **Context**: `void solvePoissonEquation(cudaLibXtDesc *d_ft, cudaLibXtDesc *d_ft_k, float **k, int N, int nGPUs)
{`

