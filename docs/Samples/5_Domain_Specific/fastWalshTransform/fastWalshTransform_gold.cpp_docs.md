# Documentation: Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp`
- **Filename**: `fastWalshTransform_gold.cpp`
- **Language**: cpp
- **Size**: 3992 bytes
- **Lines**: 100
- **Generated**: 2025-11-15 12:53:51 UTC

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

///////////////////////////////////////////////////////////////////////////////
// CPU Fast Walsh Transform
///////////////////////////////////////////////////////////////////////////////
extern "C" void fwtCPU(float *h_Output, float *h_Input, int log2N)
{
    const int N = 1 << log2N;

    for (int pos = 0; pos < N; pos++)
        h_Output[pos] = h_Input[pos];

    // Cycle through stages with different butterfly strides
    for (int stride = N / 2; stride >= 1; stride >>= 1) {
        // Cycle through subvectors of (2 * stride) elements
        for (int base = 0; base < N; base += 2 * stride)

            // Butterfly index within subvector of (2 * stride) size
            for (int j = 0; j < stride; j++) {
                int i0 = base + j + 0;
                int i1 = base + j + stride;

                float T1     = h_Output[i0];
                float T2     = h_Output[i1];
                h_Output[i0] = T1 + T2;
                h_Output[i1] = T1 - T2;
            }
    }
}

///////////////////////////////////////////////////////////////////////////////
// Straightforward Walsh Transform: used to test both CPU and GPU FWT
// Slow. Uses doubles because of straightforward accumulation
///////////////////////////////////////////////////////////////////////////////
extern "C" void slowWTcpu(float *h_Output, float *h_Input, int log2N)
{
    const int N = 1 << log2N;

    for (int i = 0; i < N; i++) {
        double sum = 0;

        for (int j = 0; j < N; j++) {
            // Walsh-Hadamard quotient
            double q = 1.0;

            for (int t = i & j; t != 0; t >>= 1)
                if (t & 1)
                    q = -q;

            sum += q * h_Input[j];
        }

        h_Output[i] = (float)sum;
    }
}

////////////////////////////////////////////////////////////////////////////////
// Reference CPU dyadic convolution.
// Extremely slow because of non-linear memory access patterns (cache thrashing)
////////////////////////////////////////////////////////////////////////////////
extern "C" void dyadicConvolutionCPU(float *h_Result, float *h_Data, float *h_Kernel, int log2dataN, int log2kernelN)
{
    const int dataN   = 1 << log2dataN;
    const int kernelN = 1 << log2kernelN;

    for (int i = 0; i < dataN; i++) {
        double sum = 0;

        for (int j = 0; j < kernelN; j++)
            sum += h_Data[i ^ j] * h_Kernel[j];

        h_Result[i] = (float)sum;
    }
}

```

---
## High-Level Overview
This file is a cpp source file with 5 function(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Functions
#### `void fwtCPU(float *h_Output, float *h_Input, int log2N)`
- Function in Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp

#### `strides for(int stride = N / 2; stride >= 1; stride >>= 1)`
- Function in Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp

#### `size for(int j = 0; j < stride; j++)`
- Function in Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp

#### `void slowWTcpu(float *h_Output, float *h_Input, int log2N)`
- Function in Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp

#### `void dyadicConvolutionCPU(float *h_Result, float *h_Data, float *h_Kernel, int log2dataN, int log2kernelN)`
- Function in Samples/5_Domain_Specific/fastWalshTransform/fastWalshTransform_gold.cpp


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

