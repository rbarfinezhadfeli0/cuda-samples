# Documentation: Samples/2_Concepts_and_Techniques/eigenvalues/gerschgorin.cpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/eigenvalues/gerschgorin.cpp`
- **Filename**: `gerschgorin.cpp`
- **Language**: cpp
- **Size**: 3295 bytes
- **Lines**: 85
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

/* Computation of Gerschgorin interval for symmetric, tridiagonal matrix */


#include "gerschgorin.h"

#include <cfloat>
#include <cmath>
#include <cstdio>
#include <cstdlib>

#include "util.h"

////////////////////////////////////////////////////////////////////////////////
//! Compute Gerschgorin interval for symmetric, tridiagonal matrix
//! @param  d  diagonal elements
//! @param  s  superdiagonal elements
//! @param  n  size of matrix
//! @param  lg  lower limit of Gerschgorin interval
//! @param  ug  upper limit of Gerschgorin interval
////////////////////////////////////////////////////////////////////////////////
void computeGerschgorin(float *d, float *s, unsigned int n, float &lg, float &ug)
{
    lg = FLT_MAX;
    ug = -FLT_MAX;

    // compute bounds
    for (unsigned int i = 1; i < (n - 1); ++i) {
        // sum over the absolute values of all elements of row i
        float sum_abs_ni = fabsf(s[i - 1]) + fabsf(s[i]);

        lg = min(lg, d[i] - sum_abs_ni);
        ug = max(ug, d[i] + sum_abs_ni);
    }

    // first and last row, only one superdiagonal element

    // first row
    lg = min(lg, d[0] - fabsf(s[0]));
    ug = max(ug, d[0] + fabsf(s[0]));

    // last row
    lg = min(lg, d[n - 1] - fabsf(s[n - 2]));
    ug = max(ug, d[n - 1] + fabsf(s[n - 2]));

    // increase interval to avoid side effects of fp arithmetic
    float bnorm = max(fabsf(ug), fabsf(lg));

    // these values depend on the implementation of floating count that is
    // employed in the following
    float psi_0 = 11 * FLT_EPSILON * bnorm;
    float psi_n = 11 * FLT_EPSILON * bnorm;

    lg = lg - bnorm * 2 * n * FLT_EPSILON - psi_0;
    ug = ug + bnorm * 2 * n * FLT_EPSILON + psi_n;

    ug = max(lg, ug);
}

```

---
## High-Level Overview
This file is a cpp source file with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 6 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `gerschgorin.h`
- `cfloat`
- `cmath`
- `cstdio`
- `cstdlib`
- `util.h`

### Functions
#### `void computeGerschgorin(float *d, float *s, unsigned int n, float &lg, float &ug)`
- Function in Samples/2_Concepts_and_Techniques/eigenvalues/gerschgorin.cpp


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

