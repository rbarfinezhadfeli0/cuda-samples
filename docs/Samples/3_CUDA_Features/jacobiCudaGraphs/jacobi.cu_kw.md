# Keywords: Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu
---

**Total Keywords**: 9

---

## J

### JacobiMethod {#jacobimethod}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `__global__ void
JacobiMethod(`

### JacobiMethodGpu {#jacobimethodgpu}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `double JacobiMethodGpu(const float  *A,
                       const double *b,
                    `

### JacobiMethodGpuCudaGraphExecKernelSetParams {#jacobimethodgpucudagraphexeckernelsetparams}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `double JacobiMethodGpuCudaGraphExecKernelSetParams(const float  *A,
                                `

### JacobiMethodGpuCudaGraphExecUpdate {#jacobimethodgpucudagraphexecupdate}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `double JacobiMethodGpuCudaGraphExecUpdate(const float  *A,
                                         `


## N

### NodeParams0 {#nodeparams0}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `udaKernelNodeParams NodeParams0, NodeParams1;
    N`

### NodeParams1 {#nodeparams1}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `Params NodeParams0, NodeParams1;
    NodeParams0.fu`


## R

### ROWS_PER_CTA {#rowspercta}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `#define ROWS_PER_CTA 8

#if !defined(__CUDA_ARCH__) || __CUDA_ARCH__ >= 600
#else
__device__ double `


## A

### atomicAdd {#atomicadd}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `double atomicAdd(double *address, double val)
{`


## F

### finalError {#finalerror}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu](./jacobi.cu_docs.md)
- **Context**: `__global__ void finalError(`

