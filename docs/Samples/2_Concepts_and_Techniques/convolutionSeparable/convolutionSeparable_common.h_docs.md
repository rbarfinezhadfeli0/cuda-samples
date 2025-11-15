# Documentation: Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable_common.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable_common.h`
- **Filename**: `convolutionSeparable_common.h`
- **Language**: h
- **Size**: 2566 bytes
- **Lines**: 51
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

#ifndef CONVOLUTIONSEPARABLE_COMMON_H
#define CONVOLUTIONSEPARABLE_COMMON_H

#define KERNEL_RADIUS 8
#define KERNEL_LENGTH (2 * KERNEL_RADIUS + 1)

////////////////////////////////////////////////////////////////////////////////
// Reference CPU convolution
////////////////////////////////////////////////////////////////////////////////
extern "C" void convolutionRowCPU(float *h_Dst, float *h_Src, float *h_Kernel, int imageW, int imageH, int kernelR);

extern "C" void convolutionColumnCPU(float *h_Dst, float *h_Src, float *h_Kernel, int imageW, int imageH, int kernelR);

////////////////////////////////////////////////////////////////////////////////
// GPU convolution
////////////////////////////////////////////////////////////////////////////////
extern "C" void setConvolutionKernel(float *h_Kernel);

extern "C" void convolutionRowsGPU(float *d_Dst, float *d_Src, int imageW, int imageH);

extern "C" void convolutionColumnsGPU(float *d_Dst, float *d_Src, int imageW, int imageH);

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **CONVOLUTIONSEPARABLE_COMMON_H**: `#define KERNEL_RADIUS 8`
- **KERNEL_LENGTH**: `(2 * KERNEL_RADIUS + 1)`


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

