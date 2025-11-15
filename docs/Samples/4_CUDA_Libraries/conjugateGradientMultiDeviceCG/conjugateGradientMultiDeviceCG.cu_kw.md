# Keywords: Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu
---

**Total Keywords**: 19

---

## C

### ConjugateGradient {#conjugategradient}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: ` // temp memory for ConjugateGradient
    checkCudaErrors`


## E

### ENABLE_CPU_DEBUG_CODE {#enablecpudebugcode}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `#define ENABLE_CPU_DEBUG_CODE 0
#define THREADS_PER_BLOCK     512

__device__ double grid_dot_result`


## M

### MultiDeviceData {#multidevicedata}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `struct MultiDeviceData`

### MultiGPU {#multigpu}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `d on CPU needed for MultiGPU operations.
struct `


## P

### PeerGroup {#peergroup}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `class PeerGroup`


## T

### THREADS_PER_BLOCK {#threadsperblock}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `#define THREADS_PER_BLOCK     512

__device__ double grid_dot_result = 0.0;

/* genTridiag: generate`


## C

### cpuConjugateGrad {#cpuconjugategrad}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void cpuConjugateGrad(int *I, int *J, float *val, float *x, float *Ax, float *p, float *r, int nnz, `

### cpuSpMV {#cpuspmv}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void cpuSpMV(int *I, int *J, float *val, int nnz, int num_rows, float alpha, float *inputVecX, float`


## D

### dotProduct {#dotproduct}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `float dotProduct(float *vecA, float *vecB, int size)
{`


## G

### genTridiag {#gentridiag}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void genTridiag(int *I, int *J, float *val, int N, int nz)
{`

### gpuCopyVector {#gpucopyvector}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void gpuCopyVector(float *srcA, float *destB, int size, const PeerGroup &peer_group)
{`

### gpuDotProduct {#gpudotproduct}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void
gpuDotProduct(float *vecA, float *vecB, int size, const cg::thread_block &cta, const PeerGroup `

### gpuSaxpy {#gpusaxpy}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void gpuSaxpy(float *x, float *y, float a, int size, const PeerGroup &peer_group)
{`

### gpuScaleVectorAndSaxpy {#gpuscalevectorandsaxpy}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void gpuScaleVectorAndSaxpy(float *x, float *y, float a, float scale, int size, const PeerGroup &pee`

### gpuSpMV {#gpuspmv}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void gpuSpMV(int             *I,
                        int             *J,
                       `


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### multiGpuConjugateGradient {#multigpuconjugategradient}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `__global__ void multiGpuConjugateGradient(`


## S

### saxpy {#saxpy}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void saxpy(float *x, float *y, float a, int size)
{`

### scaleVector {#scalevector}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/conjugateGradientMultiDeviceCG/conjugateGradientMultiDeviceCG.cu](./conjugateGradientMultiDeviceCG.cu_docs.md)
- **Context**: `void scaleVector(float *vec, float alpha, int size)
{`

