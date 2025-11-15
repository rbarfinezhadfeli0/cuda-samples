# Documentation: Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh`
- **Filename**: `quasirandomGenerator_gpu.cuh`
- **Language**: cuda
- **Size**: 4498 bytes
- **Lines**: 109
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```cuda
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

#ifndef QUASIRANDOMGENERATOR_GPU_CUH
#define QUASIRANDOMGENERATOR_GPU_CUH

#include <nvrtc_helper.h>

#include "quasirandomGenerator_common.h"

// Fast integer multiplication
#define MUL(a, b) __umul24(a, b)

// Global variables for nvrtc outputs
char    *cubin;
size_t   cubinSize;
CUmodule module;

////////////////////////////////////////////////////////////////////////////////
// GPU code
////////////////////////////////////////////////////////////////////////////////

////////////////////////////////////////////////////////////////////////////////
// Niederreiter quasirandom number generation kernel
////////////////////////////////////////////////////////////////////////////////

// Table initialization routine
void initTableGPU(unsigned int tableCPU[QRNG_DIMENSIONS][QRNG_RESOLUTION])
{
    CUdeviceptr c_Table;
    checkCudaErrors(cuModuleGetGlobal(&c_Table, NULL, module, "c_Table"));
    checkCudaErrors(cuMemcpyHtoD(c_Table, tableCPU, QRNG_DIMENSIONS * QRNG_RESOLUTION * sizeof(unsigned int)));
}

// Host-side interface
void quasirandomGeneratorGPU(CUdeviceptr d_Output, unsigned int seed, unsigned int N)
{
    dim3 threads(128, QRNG_DIMENSIONS);
    dim3 cudaGridSize(128, 1, 1);

    CUfunction kernel_addr;
    checkCudaErrors(cuModuleGetFunction(&kernel_addr, module, "quasirandomGeneratorKernel"));

    void *args[] = {(void *)&d_Output, (void *)&seed, (void *)&N};
    checkCudaErrors(cuLaunchKernel(kernel_addr,
                                   cudaGridSize.x,
                                   cudaGridSize.y,
                                   cudaGridSize.z, /* grid dim */
                                   threads.x,
                                   threads.y,
                                   threads.z, /* block dim */
                                   0,
                                   0,        /* shared mem, stream */
                                   &args[0], /* arguments */
                                   0));

    checkCudaErrors(cuCtxSynchronize());
}

void inverseCNDgpu(CUdeviceptr d_Output, unsigned int N)
{
    dim3 threads(128, 1, 1);
    dim3 cudaGridSize(128, 1, 1);

    CUfunction kernel_addr;
    checkCudaErrors(cuModuleGetFunction(&kernel_addr, module, "inverseCNDKernel"));

    void *args[] = {(void *)&d_Output, (void *)&N};
    checkCudaErrors(cuLaunchKernel(kernel_addr,
                                   cudaGridSize.x,
                                   cudaGridSize.y,
                                   cudaGridSize.z, /* grid dim */
                                   threads.x,
                                   threads.y,
                                   threads.z, /* block dim */
                                   0,
                                   0,        /* shared mem, stream */
                                   &args[0], /* arguments */
                                   0));

    checkCudaErrors(cuCtxSynchronize());
}

#endif

```

---
## High-Level Overview
This file is a cuda source file with 3 function(s) in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `nvrtc_helper.h`
- `quasirandomGenerator_common.h`

### Preprocessor Definitions
- **QUASIRANDOMGENERATOR_GPU_CUH**: `#include <nvrtc_helper.h>`
- **MUL**: ``

### Functions
#### `void initTableGPU(unsigned int tableCPU[QRNG_DIMENSIONS][QRNG_RESOLUTION])`
- Function in Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh

#### `void quasirandomGeneratorGPU(CUdeviceptr d_Output, unsigned int seed, unsigned int N)`
- Function in Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh

#### `void inverseCNDgpu(CUdeviceptr d_Output, unsigned int N)`
- Function in Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh


---
## Usage Examples
This is a CUDA source file. Typical usage involves:
1. Compiling with nvcc (NVIDIA CUDA Compiler)
2. Linking with CUDA runtime libraries
3. Executing on NVIDIA GPU hardware


---
## Performance & Security Notes
### Performance Considerations
- CUDA kernel execution is asynchronous
- Memory transfers between host and device can be a bottleneck
- Thread block and grid dimensions affect performance

### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

