# Documentation: Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable_gold.cpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable_gold.cpp`
- **Filename**: `convolutionSeparable_gold.cpp`
- **Language**: cpp
- **Size**: 3004 bytes
- **Lines**: 69
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

#include "convolutionSeparable_common.h"

////////////////////////////////////////////////////////////////////////////////
// Reference row convolution filter
////////////////////////////////////////////////////////////////////////////////
extern "C" void convolutionRowCPU(float *h_Dst, float *h_Src, float *h_Kernel, int imageW, int imageH, int kernelR)
{
    for (int y = 0; y < imageH; y++)
        for (int x = 0; x < imageW; x++) {
            float sum = 0;

            for (int k = -kernelR; k <= kernelR; k++) {
                int d = x + k;

                if (d >= 0 && d < imageW)
                    sum += h_Src[y * imageW + d] * h_Kernel[kernelR - k];
            }

            h_Dst[y * imageW + x] = sum;
        }
}

////////////////////////////////////////////////////////////////////////////////
// Reference column convolution filter
////////////////////////////////////////////////////////////////////////////////
extern "C" void convolutionColumnCPU(float *h_Dst, float *h_Src, float *h_Kernel, int imageW, int imageH, int kernelR)
{
    for (int y = 0; y < imageH; y++)
        for (int x = 0; x < imageW; x++) {
            float sum = 0;

            for (int k = -kernelR; k <= kernelR; k++) {
                int d = y + k;

                if (d >= 0 && d < imageH)
                    sum += h_Src[d * imageW + x] * h_Kernel[kernelR - k];
            }

            h_Dst[y * imageW + x] = sum;
        }
}

```

---
## High-Level Overview
This file is a cpp source file with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `convolutionSeparable_common.h`

### Functions
#### `void convolutionRowCPU(float *h_Dst, float *h_Src, float *h_Kernel, int imageW, int imageH, int kernelR)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable_gold.cpp

#### `void convolutionColumnCPU(float *h_Dst, float *h_Src, float *h_Kernel, int imageW, int imageH, int kernelR)`
- Function in Samples/2_Concepts_and_Techniques/convolutionSeparable/convolutionSeparable_gold.cpp


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

