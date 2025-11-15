# Documentation: Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h`
- **Filename**: `ParticleSystem.h`
- **Language**: h
- **Size**: 4410 bytes
- **Lines**: 131
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

#ifndef __PARTICLESYSTEM_H__
#define __PARTICLESYSTEM_H__

#include <helper_functions.h>

#include "GpuArray.h"
#include "nvMath.h"
#include "particles_kernel.cuh"
#include "vector_functions.h"

using namespace nv;

// CUDA BodySystem: runs on the GPU
class ParticleSystem
{
public:
    ParticleSystem(uint numParticles, bool bUseVBO = true, bool bUseGL = true);
    ~ParticleSystem();

    enum ParticleConfig { CONFIG_RANDOM, CONFIG_GRID, _NUM_CONFIGS };

    void step(float deltaTime);
    void depthSort();
    void reset(ParticleConfig config);

    uint getNumParticles() { return m_numParticles; }

    uint  getPosBuffer() { return m_pos.getVbo(); }
    uint  getVelBuffer() { return m_vel.getVbo(); }
    uint  getColorBuffer() { return 0; }
    uint  getSortedIndexBuffer() { return m_indices.getVbo(); }
    uint *getSortedIndices();

    float getParticleRadius() { return m_particleRadius; }

    SimParams &getParams() { return m_params; }

    void setSorting(bool x) { m_doDepthSort = x; }
    void setModelView(float *m);
    void setSortVector(float3 v) { m_sortVector = v; }

    void addSphere(uint &index, vec3f pos, vec3f vel, int r, float spacing, float jitter, float lifetime);
    void discEmitter(uint &index,
                     vec3f pos,
                     vec3f vel,
                     vec3f vx,
                     vec3f vy,
                     float r,
                     int   n,
                     float lifetime,
                     float lifetimeVariance);
    void sphereEmitter(uint &index,
                       vec3f pos,
                       vec3f vel,
                       vec3f spread,
                       float r,
                       int   n,
                       float lifetime,
                       float lifetimeVariance);

    void dumpParticles(uint start, uint count);
    void dumpBin(float4 **posData, float4 **velData);

protected: // methods
    ParticleSystem() {}

    void _initialize(int numParticlesm, bool bUseGL = true);
    void _free();

    void initGrid(vec3f start,
                  uint3 size,
                  vec3f spacing,
                  float jitter,
                  vec3f vel,
                  uint  numParticles,
                  float lifetime = 100.0f);
    void initCubeRandom(vec3f origin, vec3f size, vec3f vel, float lifetime = 100.0f);

protected: // data
    bool m_bInitialized;
    bool m_bUseVBO;
    uint m_numParticles;

    float m_particleRadius;

    GpuArray<float4> m_pos;
    GpuArray<float4> m_vel;

    // params
    SimParams m_params;

    float4x4 m_modelView;
    float3   m_sortVector;
    bool     m_doDepthSort;

    GpuArray<float> m_sortKeys;
    GpuArray<uint>  m_indices; // sorted indices for rendering

    StopWatchInterface *m_timer;
    float               m_time;
};

#endif // __PARTICLESYSTEM_H__

```

---
## High-Level Overview
This file is a h source file with 9 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 5 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `helper_functions.h`
- `GpuArray.h`
- `nvMath.h`
- `particles_kernel.cuh`
- `vector_functions.h`

### Preprocessor Definitions
- **__PARTICLESYSTEM_H__**: `#include <helper_functions.h>`

### Functions
#### `uint getNumParticles()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `uint getPosBuffer()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `uint getVelBuffer()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `uint getColorBuffer()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `uint getSortedIndexBuffer()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `float getParticleRadius()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `void setSorting(bool x)`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `void setSortVector(float3 v)`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

#### `methods ParticleSystem()`
- Function in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

### Classes / Structures
#### `class ParticleSystem`
- Defined in Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h


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

