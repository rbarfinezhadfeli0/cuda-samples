# Documentation: Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh`
- **Filename**: `sortingNetworks_common.cuh`
- **Language**: cuda
- **Size**: 2145 bytes
- **Lines**: 55
- **Generated**: 2025-11-15 12:53:54 UTC

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

#ifndef SORTINGNETWORKS_COMMON_CUH
#define SORTINGNETWORKS_COMMON_CUH

#include "sortingNetworks_common.h"

// Enables maximum occupancy
#define SHARED_SIZE_LIMIT 1024U

// Map to single instructions on G8x / G9x / G100
#define UMUL(a, b)    __umul24((a), (b))
#define UMAD(a, b, c) (UMUL((a), (b)) + (c))

__device__ inline void Comparator(uint &keyA, uint &valA, uint &keyB, uint &valB, uint dir)
{
    uint t;

    if ((keyA > keyB) == dir) {
        t    = keyA;
        keyA = keyB;
        keyB = t;
        t    = valA;
        valA = valB;
        valB = t;
    }
}

#endif

```

---
## High-Level Overview
This file is a cuda source file with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `sortingNetworks_common.h`

### Preprocessor Definitions
- **SORTINGNETWORKS_COMMON_CUH**: `#include "sortingNetworks_common.h"`
- **SHARED_SIZE_LIMIT**: `1024U`
- **UMUL**: ``
- **UMAD**: ``

### Functions
#### `void Comparator(uint &keyA, uint &valA, uint &keyB, uint &valB, uint dir)`
- Function in Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh


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

