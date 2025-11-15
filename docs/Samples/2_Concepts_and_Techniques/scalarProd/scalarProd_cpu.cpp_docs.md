# Documentation: Samples/2_Concepts_and_Techniques/scalarProd/scalarProd_cpu.cpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/scalarProd/scalarProd_cpu.cpp`
- **Filename**: `scalarProd_cpu.cpp`
- **Language**: cpp
- **Size**: 2235 bytes
- **Lines**: 46
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

////////////////////////////////////////////////////////////////////////////
// Calculate scalar products of VectorN vectors of ElementN elements on CPU.
// Straight accumulation in double precision.
////////////////////////////////////////////////////////////////////////////
extern "C" void scalarProdCPU(float *h_C, float *h_A, float *h_B, int vectorN, int elementN)
{
    for (int vec = 0; vec < vectorN; vec++) {
        int vectorBase = elementN * vec;
        int vectorEnd  = vectorBase + elementN;

        double sum = 0;

        for (int pos = vectorBase; pos < vectorEnd; pos++)
            sum += h_A[pos] * h_B[pos];

        h_C[vec] = (float)sum;
    }
}

```

---
## High-Level Overview
This file is a cpp source file with 1 function(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Functions
#### `void scalarProdCPU(float *h_C, float *h_A, float *h_B, int vectorN, int elementN)`
- Function in Samples/2_Concepts_and_Techniques/scalarProd/scalarProd_cpu.cpp


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

