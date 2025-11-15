# Documentation: Samples/2_Concepts_and_Techniques/histogram/histogram_gold.cpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/histogram/histogram_gold.cpp`
- **Filename**: `histogram_gold.cpp`
- **Language**: cpp
- **Size**: 2611 bytes
- **Lines**: 63
- **Generated**: 2025-11-15 12:53:54 UTC

---
## Original Source
```cpp
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

#include <assert.h>

#include "histogram_common.h"

extern "C" void histogram64CPU(uint *h_Histogram, void *h_Data, uint byteCount)
{
    for (uint i = 0; i < HISTOGRAM64_BIN_COUNT; i++)
        h_Histogram[i] = 0;

    assert(sizeof(uint) == 4 && (byteCount % 4) == 0);

    for (uint i = 0; i < (byteCount / 4); i++) {
        uint data = ((uint *)h_Data)[i];
        h_Histogram[(data >> 2) & 0x3FU]++;
        h_Histogram[(data >> 10) & 0x3FU]++;
        h_Histogram[(data >> 18) & 0x3FU]++;
        h_Histogram[(data >> 26) & 0x3FU]++;
    }
}

extern "C" void histogram256CPU(uint *h_Histogram, void *h_Data, uint byteCount)
{
    for (uint i = 0; i < HISTOGRAM256_BIN_COUNT; i++)
        h_Histogram[i] = 0;

    assert(sizeof(uint) == 4 && (byteCount % 4) == 0);

    for (uint i = 0; i < (byteCount / 4); i++) {
        uint data = ((uint *)h_Data)[i];
        h_Histogram[(data >> 0) & 0xFFU]++;
        h_Histogram[(data >> 8) & 0xFFU]++;
        h_Histogram[(data >> 16) & 0xFFU]++;
        h_Histogram[(data >> 24) & 0xFFU]++;
    }
}

```

---
## High-Level Overview
This file is a cpp source file with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `assert.h`
- `histogram_common.h`

### Functions
#### `void histogram64CPU(uint *h_Histogram, void *h_Data, uint byteCount)`
- Function in Samples/2_Concepts_and_Techniques/histogram/histogram_gold.cpp

#### `void histogram256CPU(uint *h_Histogram, void *h_Data, uint byteCount)`
- Function in Samples/2_Concepts_and_Techniques/histogram/histogram_gold.cpp


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

