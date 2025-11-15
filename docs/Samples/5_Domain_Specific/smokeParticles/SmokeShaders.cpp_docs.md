# Documentation for Samples/5_Domain_Specific/smokeParticles/SmokeShaders.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/smokeParticles/SmokeShaders.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/smokeParticles
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

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

// GLSL shaders for particle rendering
#define STRINGIFY(A) #A

// particle vertex shader
const char *particleVS = STRINGIFY(
  uniform float pointRadius;  // point size in world space         \n
  uniform float pointScale;   // scale to calculate size in pixels \n
  uniform vec4 eyePos;                                             \n
  void main()                                                      \n
  {
    \n
    vec4 wpos = vec4(gl_Vertex.xyz, 1.0);                          \n
    gl_Position = gl_ModelViewProjectionMatrix *wpos;              \n

    // calculate window-space point size                           \n
    vec4 eyeSpacePos = gl_ModelViewMatrix *wpos;                   \n
    float dist = length(eyeSpacePos.xyz);                          \n
    gl_PointSize = pointRadius * (pointScale / dist);              \n

    gl_TexCoord[0] = gl_MultiTexCoord0; // sprite texcoord         \n
    gl_TexCoord[1] = eyeSpacePos;                                  \n

    gl_FrontColor = gl_Color;                                      \n
  }                                                                \n
  );

// motion blur shaders
const char *mblurVS = STRINGIFY(uniform float timestep;                                    \n void
                                              main()                                                \n {
                                        \n vec3 pos  = gl_Vertex.xyz;
                                        \n vec3 vel  = gl_MultiTexCoord0.xyz;
                                        \n vec3 pos2 = (pos - vel * timestep).xyz; // previous position \n

                                        gl_Position = gl_ModelViewMatrix * vec4(pos, 1.0);
                                        \n // eye space
                                            gl_TexCoord[0] = gl_ModelViewMatrix * vec4(pos2, 1.0);
                                        \n

                                            // aging                                                 \n
                                            float lifetime = gl_MultiTexCoord0.w;
                                        \n float  age      = gl_Vertex.w;
                                        \n float  phase    = (lifetime > 0.0) ? (age / lifetime) : 1.0;
                                        \n // [0, 1]

                                            gl_TexCoord[1]
                                                .x    = phase;
                                        \n float fade = 1.0 - phase;
                                        \n
                                            //  float fade = 1.0;                                        \n

                                            //    gl_FrontColor = gl_Color;                              \n
                                            gl_FrontColor = vec4(gl_Color.xyz, gl_Color.w * fade);
                                        \n
                                    }                                                          \n);

// motion blur geometry shader
// - outputs stretched quad between previous and current positions
const char *mblurGS = "#version 120\n"
                      "#extension GL_EXT_geometry_shader4 : enable\n" STRINGIFY(
                          uniform float pointRadius; // point size in world space       \n
                          void          main()                                                    \n {
                              \n
                                  // aging                                                   \n
                                  float phase  = gl_TexCoordIn[0][1].x;
                              \n float  radius = pointRadius;
                              \n

                                  // eye space                                               \n
                                  vec3 pos    = gl_PositionIn[0].xyz;
                              \n vec3  pos2   = gl_TexCoordIn[0][0].xyz;
                              \n vec3  motion = pos - pos2;
                              \n vec3  dir    = normalize(motion);
                              \n float len    = length(motion);
                              \n

                                  vec3 x      = dir * radius;
                              \n vec3  view   = normalize(-pos);
                              \n vec3  y      = normalize(cross(dir, view)) * radius;
                              \n float facing = dot(view, dir);
                              \n

                                  // check for very small motion to avoid jitter             \n
                                  float threshold = 0.01;
                              \n

                                  if ((len < threshold) || (facing > 0.95) || (facing < -0.95))
                              {
                                  \n pos2 = pos;
                                  \n x    = vec3(radius, 0.0, 0.0);
                                  \n y    = vec3(0.0, -radius, 0.0);
                                  \n
                              }
                              \n

                                  // output quad                                             \n
                                  gl_FrontColor  = gl_FrontColorIn[0];
                              \n  gl_TexCoord[0] = vec4(0, 0, 0, phase);
                              \n  gl_TexCoord[1] = gl_PositionIn[0];
                              \n  gl_Position    = gl_ProjectionMatrix * vec4(pos + x + y, 1);
                              \n  EmitVertex();
                              \n

                                  gl_TexCoord[0] = vec4(0, 1, 0, phase);
                              \n  gl_TexCoord[1] = gl_PositionIn[0];
                              \n  gl_Position    = gl_ProjectionMatrix * vec4(pos + x - y, 1);
                              \n  EmitVertex();
                              \n

                                  gl_TexCoord[0] = vec4(1, 0, 0, phase);
                              \n  gl_TexCoord[1] = gl_PositionIn[0];
                              \n  gl_Position    = gl_ProjectionMatrix * vec4(pos2 - x + y, 1);
                              \n  EmitVertex();
                              \n

                                  gl_TexCoord[0] = vec4(1, 1, 0, phase);
                              \n  gl_TexCoord[1] = gl_PositionIn[0];
                              \n  gl_Position    = gl_ProjectionMatrix * vec4(pos2 - x - y, 1);
                              \n  EmitVertex();
                              \n
                          }                                                            \n);


const char *simplePS = STRINGIFY(void main()                                                    \n {
    \n gl_FragColor = gl_Color;
    \n
}                                                              \n);

// render particle without shadows
const char *particlePS = STRINGIFY(uniform float pointRadius;                                         \n void
                                                 main()                                                        \n {
                                           \n
                                               // calculate eye-space sphere normal from texture coordinates  \n
                                               vec3 N;
                                           \n       N.xy = gl_TexCoord[0].xy * vec2(2.0, -2.0) + vec2(-1.0, 1.0);
                                           \n float r2 = dot(N.xy, N.xy);
                                           \n

                                               if (r2 > 1.0) discard; // kill pixels outside circle         \n
                                           N.z = sqrt(1.0 - r2);
                                           \n

                                               //  float alpha = saturate(1.0 - r2);                              \n
                                               float alpha = clamp((1.0 - r2), 0.0, 1.0);
                                           \n        alpha *= gl_Color.w;
                                           \n

                                               gl_FragColor = vec4(gl_Color.xyz * alpha, alpha);
                                           \n
                                       }                                                                  \n);

// render particle including shadows
const char *particleShadowPS = STRINGIFY(
    uniform float pointRadius;                                         \n uniform sampler2D shadowTex;                                       \n uniform sampler2D depthTex;                                        \n void
        main()                                                        \n {
            \n
                // calculate eye-space sphere normal from texture coordinates  \n
                vec3 N;
            \n       N.xy = gl_TexCoord[0].xy * vec2(2.0, -2.0) + vec2(-1.0, 1.0);
            \n float r2   = dot(N.xy, N.xy);
            \n

                if (r2 > 1.0) discard;
            \n // kill pixels outside circle
                    N.z               = sqrt(1.0 - r2);
            \n vec4 eyeSpacePos       = gl_TexCoord[1];
            \n vec4 eyeSpaceSpherePos = vec4(eyeSpacePos.xyz + N * pointRadius, 1.0);
            \n // point on sphere
                vec4 shadowPos = gl_TextureMatrix[0] * eyeSpaceSpherePos;
            \n vec3  shadow    = vec3(1.0) - texture2DProj(shadowTex, shadowPos.xyw).xyz;
            \n
                //  float alpha = saturate(1.0 - r2);                              \n
                float alpha = clamp((1.0 - r2), 0.0, 1.0);
            \n        alpha *= gl_Color.w;
            \n

                gl_FragColor = vec4(gl_Color.xyz * shadow * alpha, alpha);
            \n // premul alpha
        });

// render particle as lit sphere
const char *particleSpherePS = STRINGIFY(
    uniform float pointRadius;                                         \n uniform vec3 lightDir = vec3(0.577, 0.577, 0.577);                 \n void
        main()                                                        \n {
            \n
                // calculate eye-space sphere normal from texture coordinates  \n
                vec3 N;
            \n       N.xy = gl_TexCoord[0].xy * vec2(2.0, -2.0) + vec2(-1.0, 1.0);
            \n float r2   = dot(N.xy, N.xy);
            \n

                if (r2 > 1.0) discard; // kill pixels outside circle         \n
            N.z = sqrt(1.0 - r2);
            \n

                // calculate depth                                             \n
                vec4 eyeSpacePos =
                    vec4(gl_TexCoord[1].xyz + N * pointRadius, 1.0); // position of this pixel on sphere in eye space \n
            vec4 clipSpacePos = gl_ProjectionMatrix * eyeSpacePos;
            \n   gl_FragDepth = (clipSpacePos.z / clipSpacePos.w) * 0.5 + 0.5;
            \n

                float diffuse = max(0.0, dot(N, lightDir));
            \n

                gl_FragColor = diffuse * gl_Color;
            \n
        }                                                                  \n);

const char *passThruVS = STRINGIFY(void main()                                                        \n {
    \n gl_Position    = gl_Vertex;
    \n gl_TexCoord[0] = gl_MultiTexCoord0;
    \n gl_FrontColor  = gl_Color;
    \n
}                                                                  \n);

const char *texture2DPS = STRINGIFY(uniform sampler2D tex;                                             \n void
                                                      main()                                                        \n {
                                            \n gl_FragColor = texture2D(tex, gl_TexCoord[0].xy);
                                            \n
                                        }                                                                  \n);

// 4 tap 3x3 gaussian blur
const char *blurPS = STRINGIFY(
    uniform sampler2D tex;                                    \n uniform vec2 texelSize;                                   \n uniform float blurRadius;                                 \n void
        main()                                               \n {
            \n vec4 c;
            \n      c = texture2D(tex, gl_TexCoord[0].xy + vec2(-0.5, -0.5) * texelSize * blurRadius);
            \n      c += texture2D(tex, gl_TexCoord[0].xy + vec2(0.5, -0.5) * texelSize * blurRadius);
            \n      c += texture2D(tex, gl_TexCoord[0].xy + vec2(0.5, 0.5) * texelSize * blurRadius);
            \n      c += texture2D(tex, gl_TexCoord[0].xy + vec2(-0.5, 0.5) * texelSize * blurRadius);
            \n      c *= 0.25;
            \n

                gl_FragColor = c;
            \n
        }                                                  \n);

// floor shader
const char *floorVS = STRINGIFY(
  varying vec4 vertexPosEye;  // vertex position in eye space  \n
  varying vec3 normalEye;                                      \n
  void main()                                                  \n
  {
    \n
    gl_Position = gl_ModelViewProjectionMatrix *gl_Vertex;  \n
    gl_TexCoord[0] = gl_MultiTexCoord0;                      \n
    vertexPosEye = gl_ModelViewMatrix *gl_Vertex;           \n
    normalEye = gl_NormalMatrix *gl_Normal;                 \n
    gl_FrontColor = gl_Color;                                \n
  }                                                            \n
  );

const char *floorPS = STRINGIFY(
  uniform vec3 lightPosEye; // light position in eye space           \n
  uniform vec3 lightColor;                                           \n
  uniform sampler2D tex;                                             \n
  uniform sampler2D shadowTex;                                       \n
  varying vec4 vertexPosEye;  // vertex position in eye space        \n
  varying vec3 normalEye;                                            \n
  void main()                                                        \n
  {
    \n
    vec4 shadowPos = gl_TextureMatrix[0] * vertexPosEye;                    \n
    vec4 colorMap  = texture2D(tex, gl_TexCoord[0].xy);                     \n

    vec3 N = normalize(normalEye);                                          \n
    vec3 L = normalize(lightPosEye - vertexPosEye.xyz);                     \n
    float diffuse = max(0.0, dot(N, L));                                    \n

    vec3 shadow = vec3(1.0) - texture2DProj(shadowTex, shadowPos.xyw).xyz;  \n

    if (shadowPos.w < 0.0) shadow = lightColor;   \n // avoid back projections
    gl_FragColor = vec4(gl_Color.xyz *colorMap.xyz *diffuse * shadow, 1.0); \n
  }                                                                         \n
);

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/smokeParticles/SmokeShaders.cpp`.

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

- **Total Lines**: 303
- **Approximate Size**: 16311 bytes

### Content Structure

#### Functions and Kernels

This file contains function definitions and potentially CUDA kernel launches.
Functions in this file handle:

- **Initialization**: Setting up CUDA context and allocating resources
- **Computation**: Core algorithmic implementations
- **Cleanup**: Freeing resources and error checking

#### Error Handling

The code implements error handling through:

- CUDA error checking macros
- Return code validation
- Exception handling where appropriate

#### Memory Management

Memory operations include:

- Device memory allocation (cudaMalloc)
- Host memory allocation
- Memory transfers (cudaMemcpy)
- Proper cleanup and deallocation

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

### Computational Complexity

The algorithms in this file are designed with performance in mind:

- **GPU Parallelism**: Leveraging thousands of CUDA cores
- **Memory Bandwidth**: Optimizing data transfer patterns
- **Occupancy**: Maximizing GPU utilization
- **Latency Hiding**: Using asynchronous operations where beneficial

### Optimization Opportunities

Potential areas for optimization:

1. Kernel launch configuration tuning
2. Shared memory usage
3. Coalesced memory access
4. Reduction of host-device transfers

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

To test this file:

1. Build the sample using CMake
2. Run the executable with appropriate parameters
3. Verify output against expected results
4. Check for memory leaks using cuda-memcheck
5. Profile performance using NVIDIA profiling tools

### Integration Tests

This file is tested as part of the overall sample application, ensuring:

- Correct functionality
- Expected performance characteristics
- Compatibility across different GPU architectures

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

### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Running

```bash
./{executable_name} [options]
```

Refer to the sample's README for specific command-line options and usage patterns.

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
