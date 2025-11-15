# Keywords: Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh
---

**Total Keywords**: 8

---

## E

### ELEMENTARY_LOG2SIZE {#elementarylog2size}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `#define ELEMENTARY_LOG2SIZE 11

__global__ void fwtBatch1Kernel(float *d_Output, float *d_Input, int`


## F

### FWT_KERNEL_CUH {#fwtkernelcuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `#define FWT_KERNEL_CUH
#ifndef fwt_kernel_cuh
#define fwt_kernel_cuh

#include <cooperative_groups.h`

### fwtBatch1Kernel {#fwtbatch1kernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `__global__ void fwtBatch1Kernel(`

### fwtBatch2Kernel {#fwtbatch2kernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `__global__ void fwtBatch2Kernel(`

### fwtBatchGPU {#fwtbatchgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `void fwtBatchGPU(float *d_Data, int M, int log2N)
{`

### fwt_kernel_cuh {#fwtkernelcuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `#define fwt_kernel_cuh

#include <cooperative_groups.h>

namespace cg = cooperative_groups;

///////`


## M

### modulateGPU {#modulategpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `void modulateGPU(float *d_A, float *d_B, int N) {`

### modulateKernel {#modulatekernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_kernel.cuh](./fastWalshTransform_kernel.cuh_docs.md)
- **Context**: `__global__ void modulateKernel(`

