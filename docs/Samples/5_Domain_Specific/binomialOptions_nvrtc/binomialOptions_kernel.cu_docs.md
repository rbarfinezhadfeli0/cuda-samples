# Documentation: Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_kernel.cu
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_kernel.cu`
- **Filename**: `binomialOptions_kernel.cu`
- **Language**: cuda
- **Size**: 4061 bytes
- **Lines**: 111
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

#include "binomialOptions_common.h"
#include "common_gpu_header.h"
#include "realtype.h"

// Preprocessed input option data
typedef struct
{
    real S;
    real X;
    real vDt;
    real puByDf;
    real pdByDf;
} __TOptionData;
static __constant__ __TOptionData d_OptionData[MAX_OPTIONS];
__device__ real                   d_CallValue[MAX_OPTIONS];

#define THREADBLOCK_SIZE 128
#define ELEMS_PER_THREAD (NUM_STEPS / THREADBLOCK_SIZE)
#if NUM_STEPS % THREADBLOCK_SIZE
#error Bad constants
#endif

////////////////////////////////////////////////////////////////////////////////
// Overloaded shortcut functions for different precision modes
////////////////////////////////////////////////////////////////////////////////

#ifndef DOUBLE_PRECISION
__device__ inline float expiryCallValue(float S, float X, float vDt, int i)
{
    float d = S * __expf(vDt * (2.0f * i - NUM_STEPS)) - X;
    return (d > 0.0F) ? d : 0.0F;
}

#else
__device__ inline double expiryCallValue(double S, double X, double vDt, int i)
{
    double d = S * exp(vDt * (2.0 * i - NUM_STEPS)) - X;
    return (d > 0.0) ? d : 0.0;
}
#endif

////////////////////////////////////////////////////////////////////////////////
// GPU kernel
////////////////////////////////////////////////////////////////////////////////
extern "C" __global__ void binomialOptionsKernel()
{
    __shared__ real call_exchange[THREADBLOCK_SIZE + 1];

    const int  tid    = threadIdx.x;
    const real S      = d_OptionData[blockIdx.x].S;
    const real X      = d_OptionData[blockIdx.x].X;
    const real vDt    = d_OptionData[blockIdx.x].vDt;
    const real puByDf = d_OptionData[blockIdx.x].puByDf;
    const real pdByDf = d_OptionData[blockIdx.x].pdByDf;

    real call[ELEMS_PER_THREAD + 1];
#pragma unroll
    for (int i = 0; i < ELEMS_PER_THREAD; ++i)
        call[i] = expiryCallValue(S, X, vDt, tid * ELEMS_PER_THREAD + i);

    if (tid == 0)
        call_exchange[THREADBLOCK_SIZE] = expiryCallValue(S, X, vDt, NUM_STEPS);

    int final_it = max(0, tid * ELEMS_PER_THREAD - 1);

#pragma unroll 16
    for (int i = NUM_STEPS; i > 0; --i) {
        call_exchange[tid] = call[0];
        __syncthreads();
        call[ELEMS_PER_THREAD] = call_exchange[tid + 1];
        __syncthreads();

        if (i > final_it) {
#pragma unroll
            for (int j = 0; j < ELEMS_PER_THREAD; ++j)
                call[j] = puByDf * call[j + 1] + pdByDf * call[j];
        }
    }

    if (tid == 0) {
        d_CallValue[blockIdx.x] = call[0];
    }
}

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 4 function(s) in the CUDA Samples repository.

**Dependencies**: 3 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `binomialOptions_common.h`
- `common_gpu_header.h`
- `realtype.h`

### Preprocessor Definitions
- **THREADBLOCK_SIZE**: `128`
- **ELEMS_PER_THREAD**: `(NUM_STEPS / THREADBLOCK_SIZE)`

### CUDA Kernels
#### `binomialOptionsKernel`
- **Return Type**: `void`
- **Parameters**: ``
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `float expiryCallValue(float S, float X, float vDt, int i)`
- Function in Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_kernel.cu

#### `double expiryCallValue(double S, double X, double vDt, int i)`
- Function in Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_kernel.cu

#### `void binomialOptionsKernel()`
- Function in Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_kernel.cu

#### `16 for(int i = NUM_STEPS; i > 0; --i)`
- Function in Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_kernel.cu


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

