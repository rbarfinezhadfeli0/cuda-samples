# Documentation: Samples/2_Concepts_and_Techniques/scan/scan_common.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/scan/scan_common.h`
- **Filename**: `scan_common.h`
- **Language**: h
- **Size**: 2968 bytes
- **Lines**: 63
- **Generated**: 2025-11-15 12:53:54 UTC

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

#ifndef SCAN_COMMON_H
#define SCAN_COMMON_H

#include <stdlib.h>

////////////////////////////////////////////////////////////////////////////////
// Shortcut typename
////////////////////////////////////////////////////////////////////////////////
typedef unsigned int uint;

////////////////////////////////////////////////////////////////////////////////
// Implementation limits
////////////////////////////////////////////////////////////////////////////////
extern "C" const uint MAX_BATCH_ELEMENTS;
extern "C" const uint MIN_SHORT_ARRAY_SIZE;
extern "C" const uint MAX_SHORT_ARRAY_SIZE;
extern "C" const uint MIN_LARGE_ARRAY_SIZE;
extern "C" const uint MAX_LARGE_ARRAY_SIZE;

////////////////////////////////////////////////////////////////////////////////
// CUDA scan
////////////////////////////////////////////////////////////////////////////////
extern "C" void initScan(void);
extern "C" void closeScan(void);

extern "C" size_t scanExclusiveShort(uint *d_Dst, uint *d_Src, uint batchSize, uint arrayLength);

extern "C" size_t scanExclusiveLarge(uint *d_Dst, uint *d_Src, uint batchSize, uint arrayLength);

////////////////////////////////////////////////////////////////////////////////
// Reference CPU scan
////////////////////////////////////////////////////////////////////////////////
extern "C" void scanExclusiveHost(uint *dst, uint *src, uint batchSize, uint arrayLength);

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `stdlib.h`

### Preprocessor Definitions
- **SCAN_COMMON_H**: `#include <stdlib.h>`


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

