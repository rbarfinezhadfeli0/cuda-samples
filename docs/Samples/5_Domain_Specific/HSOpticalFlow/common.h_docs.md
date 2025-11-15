# Documentation: Samples/5_Domain_Specific/HSOpticalFlow/common.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/HSOpticalFlow/common.h`
- **Filename**: `common.h`
- **Language**: h
- **Size**: 2893 bytes
- **Lines**: 77
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

///////////////////////////////////////////////////////////////////////////////
// Header for common includes and utility functions
///////////////////////////////////////////////////////////////////////////////

#ifndef COMMON_H
#define COMMON_H

///////////////////////////////////////////////////////////////////////////////
// Common includes
///////////////////////////////////////////////////////////////////////////////

#include <helper_cuda.h>
#include <math.h>
#include <memory.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

///////////////////////////////////////////////////////////////////////////////
// Common constants
///////////////////////////////////////////////////////////////////////////////
const int StrideAlignment = 32;

///////////////////////////////////////////////////////////////////////////////
// Common functions
///////////////////////////////////////////////////////////////////////////////

// Align up n to the nearest multiple of m
inline int iAlignUp(int n, int m = StrideAlignment)
{
    int mod = n % m;

    if (mod)
        return n + m - mod;
    else
        return n;
}

// round up n/m
inline int iDivUp(int n, int m) { return (n + m - 1) / m; }

// swap two values
template <typename T> inline void Swap(T &a, T &b)
{
    T t = a;
    a   = b;
    b   = t;
}
#endif

```

---
## High-Level Overview
This file is a h source file with 3 function(s) in the CUDA Samples repository.

**Dependencies**: 6 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `helper_cuda.h`
- `math.h`
- `memory.h`
- `stdio.h`
- `stdlib.h`
- `time.h`

### Preprocessor Definitions
- **COMMON_H**: `///////////////////////////////////////////////////////////////////////////////`

### Functions
#### `int iAlignUp(int n, int m = StrideAlignment)`
- Function in Samples/5_Domain_Specific/HSOpticalFlow/common.h

#### `int iDivUp(int n, int m)`
- Function in Samples/5_Domain_Specific/HSOpticalFlow/common.h

#### `void Swap(T &a, T &b)`
- Function in Samples/5_Domain_Specific/HSOpticalFlow/common.h


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

