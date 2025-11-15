# Keywords: Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh
---

**Total Keywords**: 5

---

## M

### MUL {#mul}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh](./quasirandomGenerator_gpu.cuh_docs.md)
- **Context**: `#define MUL(a, b) __umul24(a, b)

// Global variables for nvrtc outputs
char    *cubin;
size_t   cub`


## Q

### QUASIRANDOMGENERATOR_GPU_CUH {#quasirandomgeneratorgpucuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh](./quasirandomGenerator_gpu.cuh_docs.md)
- **Context**: `#define QUASIRANDOMGENERATOR_GPU_CUH

#include <nvrtc_helper.h>

#include "quasirandomGenerator_comm`


## I

### initTableGPU {#inittablegpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh](./quasirandomGenerator_gpu.cuh_docs.md)
- **Context**: `void initTableGPU(unsigned int tableCPU[QRNG_DIMENSIONS][QRNG_RESOLUTION])
{`

### inverseCNDgpu {#inversecndgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh](./quasirandomGenerator_gpu.cuh_docs.md)
- **Context**: `void inverseCNDgpu(CUdeviceptr d_Output, unsigned int N)
{`


## Q

### quasirandomGeneratorGPU {#quasirandomgeneratorgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh](./quasirandomGenerator_gpu.cuh_docs.md)
- **Context**: `void quasirandomGeneratorGPU(CUdeviceptr d_Output, unsigned int seed, unsigned int N)
{`

