# Documentation: Samples/2_Concepts_and_Techniques/particles/shaders.cpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/particles/shaders.cpp`
- **Filename**: `shaders.cpp`
- **Language**: cpp
- **Size**: 3184 bytes
- **Lines**: 66
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

#define STRINGIFY(A) #A

// vertex shader
const char *vertexShader = STRINGIFY(uniform float pointRadius; // point size in world space
                                     uniform float pointScale;  // scale to calculate size in pixels
                                     uniform float densityScale;
                                     uniform float densityOffset;
                                     void          main() {
                                         // calculate window-space point size
                                         vec3  posEye = vec3(gl_ModelViewMatrix * vec4(gl_Vertex.xyz, 1.0));
                                         float dist   = length(posEye);
                                         gl_PointSize = pointRadius * (pointScale / dist);

                                         gl_TexCoord[0] = gl_MultiTexCoord0;
                                         gl_Position = gl_ModelViewProjectionMatrix * vec4(gl_Vertex.xyz, 1.0);

                                         gl_FrontColor = gl_Color;
                                     });

// pixel shader for rendering points as shaded spheres
const char *spherePixelShader = STRINGIFY(void main() {
    const vec3 lightDir = vec3(0.577, 0.577, 0.577);

    // calculate normal from texture coordinates
    vec3 N;
    N.xy      = gl_TexCoord[0].xy * vec2(2.0, -2.0) + vec2(-1.0, 1.0);
    float mag = dot(N.xy, N.xy);

    if (mag > 1.0)
        discard; // kill pixels outside circle

    N.z = sqrt(1.0 - mag);

    // calculate lighting
    float diffuse = max(0.0, dot(lightDir, N));

    gl_FragColor = gl_Color * diffuse;
});

```

---
## High-Level Overview
This file is a cpp source file with 2 function(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **STRINGIFY**: ``

### Functions
#### `void main()`
- Function in Samples/2_Concepts_and_Techniques/particles/shaders.cpp

#### `void main()`
- Function in Samples/2_Concepts_and_Techniques/particles/shaders.cpp


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

