# Keywords: Samples/0_Introduction/simpleMPI/simpleMPI.cu
---

**Total Keywords**: 5

---

## C

### CUDA_CHECK {#cudacheck}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleMPI/simpleMPI.cu](./simpleMPI.cu_docs.md)
- **Context**: `#define CUDA_CHECK(call)                                                     \
    if ((call) != cud`

### computeGPU {#computegpu}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMPI/simpleMPI.cu](./simpleMPI.cu_docs.md)
- **Context**: `void computeGPU(float *hostData, int blockSize, int gridSize)
{`


## I

### initData {#initdata}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMPI/simpleMPI.cu](./simpleMPI.cu_docs.md)
- **Context**: `void initData(float *data, int dataSize)
{`


## S

### simpleMPIKernel {#simplempikernel}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/simpleMPI/simpleMPI.cu](./simpleMPI.cu_docs.md)
- **Context**: `__global__ void simpleMPIKernel(`

### sum {#sum}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMPI/simpleMPI.cu](./simpleMPI.cu_docs.md)
- **Context**: `float sum(float *data, int size)
{`

