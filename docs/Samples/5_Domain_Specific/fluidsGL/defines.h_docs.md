# Documentation: Samples/5_Domain_Specific/fluidsGL/defines.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/fluidsGL/defines.h`
- **Filename**: `defines.h`
- **Language**: h
- **Size**: 2286 bytes
- **Lines**: 48
- **Generated**: 2025-11-15 12:53:53 UTC

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

#ifndef DEFINES_H
#define DEFINES_H

#define DIM   512                 // Square size of solver domain
#define DS    (DIM * DIM)         // Total domain size
#define CPADW (DIM / 2 + 1)       // Padded width for real->complex in-place FFT
#define RPADW (2 * (DIM / 2 + 1)) // Padded width for real->complex in-place FFT
#define PDS   (DIM * CPADW)       // Padded total domain size

#define DT    0.09f        // Delta T for interative solver
#define VIS   0.0025f      // Viscosity constant
#define FORCE (5.8f * DIM) // Force scale factor
#define FR    4            // Force update radius

#define TILEX 64 // Tile width
#define TILEY 64 // Tile height
#define TIDSX 64 // Tids in X
#define TIDSY 4  // Tids in Y

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **DEFINES_H**: `#define DIM   512                 // Square size of solver domain`
- **DS**: `(DIM * DIM)         // Total domain size`
- **CPADW**: `(DIM / 2 + 1)       // Padded width for real->complex in-place FFT`
- **RPADW**: `(2 * (DIM / 2 + 1)) // Padded width for real->complex in-place FFT`
- **PDS**: `(DIM * CPADW)       // Padded total domain size`
- **DT**: `0.09f        // Delta T for interative solver`
- **VIS**: `0.0025f      // Viscosity constant`
- **FORCE**: `(5.8f * DIM) // Force scale factor`
- **FR**: `4            // Force update radius`
- **TILEX**: `64 // Tile width`
- **TILEY**: `64 // Tile height`
- **TIDSX**: `64 // Tids in X`
- **TIDSY**: `4  // Tids in Y`


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

