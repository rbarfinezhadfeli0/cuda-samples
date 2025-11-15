# Documentation: Samples/5_Domain_Specific/simpleVulkan/SineWaveSimulation.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/simpleVulkan/SineWaveSimulation.h`
- **Filename**: `SineWaveSimulation.h`
- **Language**: h
- **Size**: 2249 bytes
- **Lines**: 57
- **Generated**: 2025-11-15 12:53:52 UTC

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

#pragma once
#ifndef __SINESIM_H__
#define __SINESIM_H__

#include <cuda_runtime_api.h>
#include <stdint.h>
#include <vector>

#include "linmath.h"

class SineWaveSimulation
{
    float *m_heightMap;
    size_t m_width, m_height;
    int    m_blocks, m_threads;

public:
    SineWaveSimulation(size_t width, size_t height);
    ~SineWaveSimulation();
    void initSimulation(float *heightMap);
    void stepSimulation(float time, cudaStream_t stream = 0);
    void initCudaLaunchConfig(int device);
    int  initCuda(uint8_t *vkDeviceUUID, size_t UUID_SIZE);

    size_t getWidth() const { return m_width; }
    size_t getHeight() const { return m_height; }
};

#endif // __SINESIM_H__

```

---
## High-Level Overview
This file is a h source file and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda_runtime_api.h`
- `stdint.h`
- `vector`
- `linmath.h`

### Preprocessor Definitions
- **__SINESIM_H__**: `#include <cuda_runtime_api.h>`

### Classes / Structures
#### `class SineWaveSimulation`
- Defined in Samples/5_Domain_Specific/simpleVulkan/SineWaveSimulation.h


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

