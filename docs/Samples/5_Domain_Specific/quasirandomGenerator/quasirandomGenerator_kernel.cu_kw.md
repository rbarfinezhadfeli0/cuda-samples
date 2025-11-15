# Keywords: Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu
---

**Total Keywords**: 8

---

## M

### MUL {#mul}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `#define MUL(a, b) __umul24(a, b)

//////////////////////////////////////////////////////////////////`

### MoroInvCNDgpu {#moroinvcndgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `float MoroInvCNDgpu(unsigned int x)
{`


## Q

### QUASIRANDOMGENERATOR_KERNEL_CUH {#quasirandomgeneratorkernelcuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `#define QUASIRANDOMGENERATOR_KERNEL_CUH

#include <helper_cuda.h>
#include <stdio.h>
#include <stdli`


## I

### initTableGPU {#inittablegpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `void initTableGPU(unsigned int tableCPU[QRNG_DIMENSIONS][QRNG_RESOLUTION])
{`

### inverseCNDKernel {#inversecndkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `__global__ void inverseCNDKernel(`

### inverseCNDgpu {#inversecndgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `void inverseCNDgpu(float *d_Output, unsigned int *d_Input, unsigned int N)
{`


## Q

### quasirandomGeneratorGPU {#quasirandomgeneratorgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `void quasirandomGeneratorGPU(float *d_Output, unsigned int seed, unsigned int N)
{`

### quasirandomGeneratorKernel {#quasirandomgeneratorkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_kernel.cu](./quasirandomGenerator_kernel.cu_docs.md)
- **Context**: `__global__ void quasirandomGeneratorKernel(`

