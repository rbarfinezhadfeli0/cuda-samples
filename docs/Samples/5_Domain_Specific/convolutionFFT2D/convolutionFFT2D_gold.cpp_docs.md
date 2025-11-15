# Documentation: Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D_gold.cpp
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D_gold.cpp`
- **Filename**: `convolutionFFT2D_gold.cpp`
- **Language**: cpp
- **Size**: 3175 bytes
- **Lines**: 72
- **Generated**: 2025-11-15 12:53:52 UTC

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

#include "convolutionFFT2D_common.h"

////////////////////////////////////////////////////////////////////////////////
// Reference straightforward CPU convolution
////////////////////////////////////////////////////////////////////////////////
extern "C" void convolutionClampToBorderCPU(float *h_Result,
                                            float *h_Data,
                                            float *h_Kernel,
                                            int    dataH,
                                            int    dataW,
                                            int    kernelH,
                                            int    kernelW,
                                            int    kernelY,
                                            int    kernelX)
{
    for (int y = 0; y < dataH; y++)
        for (int x = 0; x < dataW; x++) {
            double sum = 0;

            for (int ky = -(kernelH - kernelY - 1); ky <= kernelY; ky++)
                for (int kx = -(kernelW - kernelX - 1); kx <= kernelX; kx++) {
                    int dy = y + ky;
                    int dx = x + kx;

                    if (dy < 0)
                        dy = 0;

                    if (dx < 0)
                        dx = 0;

                    if (dy >= dataH)
                        dy = dataH - 1;

                    if (dx >= dataW)
                        dx = dataW - 1;

                    sum += h_Data[dy * dataW + dx] * h_Kernel[(kernelY - ky) * kernelW + (kernelX - kx)];
                }

            h_Result[y * dataW + x] = (float)sum;
        }
}

```

---
## High-Level Overview
This file is a cpp source file with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `assert.h`
- `convolutionFFT2D_common.h`

### Functions
#### `void convolutionClampToBorderCPU(float *h_Result,
                                            float *h_Data,
                                            float *h_Kernel,
                                            int    dataH,
                                            int    dataW,
                                            int    kernelH,
                                            int    kernelW,
                                            int    kernelY,
                                            int    kernelX)`
- Function in Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D_gold.cpp


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

