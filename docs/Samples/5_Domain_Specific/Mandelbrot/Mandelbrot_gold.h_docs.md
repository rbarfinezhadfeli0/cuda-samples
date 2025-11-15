# Documentation: Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_gold.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_gold.h`
- **Filename**: `Mandelbrot_gold.h`
- **Language**: h
- **Size**: 4631 bytes
- **Lines**: 87
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

#ifndef _MANDELBROT_GOLD_h_
#define _MANDELBROT_GOLD_h_

#include <vector_types.h>

extern "C" void RunMandelbrotGold0(uchar4      *dst,
                                   const int    imageW,
                                   const int    imageH,
                                   const int    crunch,
                                   const float  xOff,
                                   const float  yOff,
                                   const float  xJParam,
                                   const float  yJParam,
                                   const float  scale,
                                   const uchar4 colors,
                                   const int    frame,
                                   const int    animationFrame,
                                   const bool   isJulia);
extern "C" void RunMandelbrotDSGold0(uchar4      *dst,
                                     const int    imageW,
                                     const int    imageH,
                                     const int    crunch,
                                     const double xOff,
                                     const double yOff,
                                     const double xJParam,
                                     const double yJParam,
                                     const double scale,
                                     const uchar4 colors,
                                     const int    frame,
                                     const int    animationFrame,
                                     const bool   isJulia);
extern "C" void RunMandelbrotGold1(uchar4      *dst,
                                   const int    imageW,
                                   const int    imageH,
                                   const int    crunch,
                                   const float  xOff,
                                   const float  yOff,
                                   const float  xJParam,
                                   const float  yJParam,
                                   const float  scale,
                                   const uchar4 colors,
                                   const int    frame,
                                   const int    animationFrame,
                                   const bool   isJulia);
extern "C" void RunMandelbrotDSGold1(uchar4      *dst,
                                     const int    imageW,
                                     const int    imageH,
                                     const int    crunch,
                                     const double xOff,
                                     const double yOff,
                                     const double xJParam,
                                     const double yJParam,
                                     const double scale,
                                     const uchar4 colors,
                                     const int    frame,
                                     const int    animationFrame,
                                     const bool   isJulia);

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `vector_types.h`

### Preprocessor Definitions
- **_MANDELBROT_GOLD_h_**: `#include <vector_types.h>`


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

