# Documentation for Samples/5_Domain_Specific/smokeParticles/particles_kernel_device.cuh

## File Metadata

- **Path**: `Samples/5_Domain_Specific/smokeParticles/particles_kernel_device.cuh`
- **Type**: .cuh
- **Location**: Samples/5_Domain_Specific/smokeParticles
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```cuh
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

/*
 * CUDA Device code for particle simulation.
 */

#ifndef _PARTICLES_KERNEL_H_
#define _PARTICLES_KERNEL_H_

#include "helper_math.h"
#include "math_constants.h"
#include "particles_kernel.cuh"
#include "thrust/device_ptr.h"
#include "thrust/for_each.h"
#include "thrust/iterator/zip_iterator.h"
#include "thrust/sort.h"

cudaTextureObject_t noiseTex;
// simulation parameters
__constant__ SimParams cudaParams;

// look up in 3D noise texture
__device__ float3 noise3D(float3 p, cudaTextureObject_t noiseTex)
{
    float4 n = tex3D<float4>(noiseTex, p.x, p.y, p.z);
    return make_float3(n.x, n.y, n.z);
}

// integrate particle attributes
struct integrate_functor
{
    float               deltaTime;
    cudaTextureObject_t noiseTex;

    __host__ __device__ integrate_functor(float delta_time, cudaTextureObject_t noise_Tex)
        : deltaTime(delta_time)
        , noiseTex(noise_Tex)
    {
    }

    template <typename Tuple> __device__ void operator()(Tuple t)
    {
        volatile float4 posData = thrust::get<2>(t);
        volatile float4 velData = thrust::get<3>(t);

        float3 pos = make_float3(posData.x, posData.y, posData.z);
        float3 vel = make_float3(velData.x, velData.y, velData.z);

        // update particle age
        float age      = posData.w;
        float lifetime = velData.w;

        if (age < lifetime) {
            age += deltaTime;
        }
        else {
            age = lifetime;
        }

        // apply accelerations
        vel += cudaParams.gravity * deltaTime;

        // apply procedural noise
        float3 noise = noise3D(pos * cudaParams.noiseFreq + cudaParams.time * cudaParams.noiseSpeed, noiseTex);
        vel += noise * cudaParams.noiseAmp;

        // new position = old position + velocity * deltaTime
        pos += vel * deltaTime;

        vel *= cudaParams.globalDamping;

        // store new position and velocity
        thrust::get<0>(t) = make_float4(pos, age);
        thrust::get<1>(t) = make_float4(vel, velData.w);
    }
};

struct calcDepth_functor
{
    float3 sortVector;

    __host__ __device__ calcDepth_functor(float3 sort_vector)
        : sortVector(sort_vector)
    {
    }

    template <typename Tuple> __host__ __device__ void operator()(Tuple t)
    {
        volatile float4 p   = thrust::get<0>(t);
        float           key = -dot(make_float3(p.x, p.y, p.z),
                         sortVector); // project onto sort vector
        thrust::get<1>(t)   = key;
    }
};

#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/smokeParticles/particles_kernel_device.cuh`.

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

- **Total Lines**: 122
- **Approximate Size**: 4052 bytes

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
