# Documentation: Samples/5_Domain_Specific/marchingCubes/defines.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/marchingCubes/defines.h`
- **Filename**: `defines.h`
- **Language**: h
- **Size**: 2075 bytes
- **Lines**: 50
- **Generated**: 2025-11-15 12:53:51 UTC

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

#ifndef _DEFINES_H_
#define _DEFINES_H_

typedef unsigned int  uint;
typedef unsigned char uchar;

// if SAMPLE_VOLUME is 0, an implicit dataset is generated. If 1, a voxelized
// dataset is loaded from file
#define SAMPLE_VOLUME 1

// Using shared to store computed vertices and normals during triangle
// generation
// improves performance
#define USE_SHARED 1

// The number of threads to use for triangle generation (limited by shared
// memory size)
#define NTHREADS 32

#define SKIP_EMPTY_VOXELS 1

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **_DEFINES_H_**: `typedef unsigned int  uint;`
- **SAMPLE_VOLUME**: `1`
- **USE_SHARED**: `1`
- **NTHREADS**: `32`
- **SKIP_EMPTY_VOXELS**: `1`


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

