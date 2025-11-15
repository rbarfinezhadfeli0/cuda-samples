# Documentation: Samples/2_Concepts_and_Techniques/histogram/histogram_common.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/histogram/histogram_common.h`
- **Filename**: `histogram_common.h`
- **Language**: h
- **Size**: 3711 bytes
- **Lines**: 85
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

#ifndef HISTOGRAM_COMMON_H
#define HISTOGRAM_COMMON_H

////////////////////////////////////////////////////////////////////////////////
// Common definitions
////////////////////////////////////////////////////////////////////////////////
#define HISTOGRAM64_BIN_COUNT  64
#define HISTOGRAM256_BIN_COUNT 256
#define UINT_BITS              32
typedef unsigned int  uint;
typedef unsigned char uchar;

////////////////////////////////////////////////////////////////////////////////
// GPU-specific common definitions
////////////////////////////////////////////////////////////////////////////////
#define LOG2_WARP_SIZE 5U
#define WARP_SIZE      (1U << LOG2_WARP_SIZE)

// May change on future hardware, so better parametrize the code
#define SHARED_MEMORY_BANKS 16

// Threadblock size: must be a multiple of (4 * SHARED_MEMORY_BANKS)
// because of the bit permutation of threadIdx.x
#define HISTOGRAM64_THREADBLOCK_SIZE (4 * SHARED_MEMORY_BANKS)

// Warps ==subhistograms per threadblock
#define WARP_COUNT 6

// Threadblock size
#define HISTOGRAM256_THREADBLOCK_SIZE (WARP_COUNT * WARP_SIZE)

// Shared memory per threadblock
#define HISTOGRAM256_THREADBLOCK_MEMORY (WARP_COUNT * HISTOGRAM256_BIN_COUNT)

#define UMUL(a, b)    ((a) * (b))
#define UMAD(a, b, c) (UMUL((a), (b)) + (c))

////////////////////////////////////////////////////////////////////////////////
// Reference CPU histogram
////////////////////////////////////////////////////////////////////////////////
extern "C" void histogram64CPU(uint *h_Histogram, void *h_Data, uint byteCount);

extern "C" void histogram256CPU(uint *h_Histogram, void *h_Data, uint byteCount);

////////////////////////////////////////////////////////////////////////////////
// GPU histogram
////////////////////////////////////////////////////////////////////////////////
extern "C" void initHistogram64(void);
extern "C" void initHistogram256(void);
extern "C" void closeHistogram64(void);
extern "C" void closeHistogram256(void);

extern "C" void histogram64(uint *d_Histogram, void *d_Data, uint byteCount);

extern "C" void histogram256(uint *d_Histogram, void *d_Data, uint byteCount);

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **HISTOGRAM_COMMON_H**: `////////////////////////////////////////////////////////////////////////////////`
- **HISTOGRAM64_BIN_COUNT**: `64`
- **HISTOGRAM256_BIN_COUNT**: `256`
- **UINT_BITS**: `32`
- **LOG2_WARP_SIZE**: `5U`
- **WARP_SIZE**: `(1U << LOG2_WARP_SIZE)`
- **SHARED_MEMORY_BANKS**: `16`
- **HISTOGRAM64_THREADBLOCK_SIZE**: `(4 * SHARED_MEMORY_BANKS)`
- **WARP_COUNT**: `6`
- **HISTOGRAM256_THREADBLOCK_SIZE**: `(WARP_COUNT * WARP_SIZE)`
- **HISTOGRAM256_THREADBLOCK_MEMORY**: `(WARP_COUNT * HISTOGRAM256_BIN_COUNT)`
- **UMUL**: ``
- **UMAD**: ``


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

