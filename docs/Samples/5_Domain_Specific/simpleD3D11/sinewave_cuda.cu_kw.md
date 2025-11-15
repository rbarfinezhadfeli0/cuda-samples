# Keywords: Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu
---

**Total Keywords**: 6

---

## R

### RunSineWaveKernel {#runsinewavekernel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu](./sinewave_cuda.cu_docs.md)
- **Context**: `void RunSineWaveKernel(cudaExternalSemaphore_t &extSemaphore,
                       uint64_t       `


## S

### ShaderStructs {#shaderstructs}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu](./sinewave_cuda.cu_docs.md)
- **Context**: `stdio.h>

#include "ShaderStructs.h"
#include "helper`


## C

### cudaAcquireSync {#cudaacquiresync}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu](./sinewave_cuda.cu_docs.md)
- **Context**: `void cudaAcquireSync(cudaExternalSemaphore_t &extSemaphore,
                     uint64_t           `

### cudaImportKeyedMutex {#cudaimportkeyedmutex}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu](./sinewave_cuda.cu_docs.md)
- **Context**: `void cudaImportKeyedMutex(void *sharedHandle, cudaExternalSemaphore_t &extSemaphore)
{`

### cudaReleaseSync {#cudareleasesync}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu](./sinewave_cuda.cu_docs.md)
- **Context**: `void cudaReleaseSync(cudaExternalSemaphore_t &extSemaphore, uint64_t key, cudaStream_t streamToRun)
`


## S

### sinewave_gen_kernel {#sinewavegenkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/simpleD3D11/sinewave_cuda.cu](./sinewave_cuda.cu_docs.md)
- **Context**: `__global__ void sinewave_gen_kernel(`

