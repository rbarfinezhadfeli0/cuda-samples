# Documentation: Samples/4_CUDA_Libraries/randomFog/rng.h
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/randomFog/rng.h`
- **Filename**: `rng.h`
- **Language**: h
- **Size**: 2626 bytes
- **Lines**: 73
- **Generated**: 2025-11-15 12:53:50 UTC

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

#include <curand.h>
#include <string>

// RNGs
class RNG
{
public:
    enum RngType { Pseudo, Quasi, ScrambledQuasi };
    RNG(unsigned long prngSeed, unsigned int qrngDimensions, unsigned int nSamples);
    virtual ~RNG();

    float getNextU01(void);
    void  getInfoString(std::string &msg);
    void  selectRng(RngType type);
    void  resetSeed(void);
    void  resetDimensions(void);
    void  incrementDimensions(void);

private:
    // Generators
    curandGenerator_t *m_pCurrent;
    curandGenerator_t  m_prng;
    curandGenerator_t  m_qrng;
    curandGenerator_t  m_sqrng;

    // Parameters
    unsigned long m_prngSeed;
    unsigned int  m_qrngDimensions;

    // Batches
    const unsigned int m_nSamplesBatchTarget;
    unsigned int       m_nSamplesBatchActual;
    unsigned int       m_nSamplesRemaining;
    void               generateBatch(void);

    // Helpers
    void updateDimensions(void);
    void setBatchSize(void);

    // Buffers
    float *m_h_samples;
    float *m_d_samples;

    static const unsigned int s_maxQrngDimensions;
};

```

---
## High-Level Overview
This file is a h source file and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `curand.h`
- `string`

### Classes / Structures
#### `class RNG`
- Defined in Samples/4_CUDA_Libraries/randomFog/rng.h


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

