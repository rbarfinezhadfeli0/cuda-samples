# Documentation: Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_common.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_common.h`
- **Filename**: `MonteCarlo_common.h`
- **Language**: h
- **Size**: 3119 bytes
- **Lines**: 102
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```h
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

#ifndef MONTECARLO_COMMON_H
#define MONTECARLO_COMMON_H
#include "curand_kernel.h"
#include "realtype.h"

////////////////////////////////////////////////////////////////////////////////
// Global types
////////////////////////////////////////////////////////////////////////////////
typedef struct
{
    float S;
    float X;
    float T;
    float R;
    float V;
} TOptionData;

typedef struct
// #ifdef __CUDACC__
//__align__(8)
// #endif
{
    float Expected;
    float Confidence;
} TOptionValue;

// GPU outputs before CPU postprocessing
typedef struct
{
    real Expected;
    real Confidence;
} __TOptionValue;

typedef struct
{
    // Device ID for multi-GPU version
    int device;
    // Option count for this plan
    int optionCount;

    // Host-side data source and result destination
    TOptionData  *optionData;
    TOptionValue *callValue;

    // Temporary Host-side pinned memory for async + faster data transfers
    __TOptionValue *h_CallValue;

    // Device- and host-side option data
    void *d_OptionData;
    void *h_OptionData;

    // Device-side option values
    void *d_CallValue;

    // Intermediate device-side buffers
    void *d_Buffer;

    // random number generator states
    curandState *rngStates;

    // Pseudorandom samples count
    int pathN;

    // Time stamp
    float time;

    int gridSize;
} TOptionPlan;

extern "C" void initMonteCarloGPU(TOptionPlan *plan);
extern "C" void MonteCarloGPU(TOptionPlan *plan, cudaStream_t stream = 0);
extern "C" void closeMonteCarloGPU(TOptionPlan *plan);

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `curand_kernel.h`
- `realtype.h`

### Preprocessor Definitions
- **MONTECARLO_COMMON_H**: `#include "curand_kernel.h"`


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


---
## Performance & Security Notes
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

