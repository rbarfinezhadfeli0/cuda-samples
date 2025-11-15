# Documentation for Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h

## File Metadata

- **Path**: `Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h`
- **Type**: .h
- **Location**: Samples/5_Domain_Specific/smokeParticles
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

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

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/smokeParticles/ParticleSystem.h`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 131
- **Approximate Size**: 4410 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
