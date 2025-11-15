# Documentation: Samples/2_Concepts_and_Techniques/particles/render_particles.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/particles/render_particles.h`
- **Filename**: `render_particles.h`
- **Language**: h
- **Size**: 2715 bytes
- **Lines**: 76
- **Generated**: 2025-11-15 12:53:54 UTC

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

#ifndef __RENDER_PARTICLES__
#define __RENDER_PARTICLES__

class ParticleRenderer
{
public:
    ParticleRenderer();
    ~ParticleRenderer();

    void setPositions(float *pos, int numParticles);
    void setVertexBuffer(unsigned int vbo, int numParticles);
    void setColorBuffer(unsigned int vbo) { m_colorVBO = vbo; }

    enum DisplayMode { PARTICLE_POINTS, PARTICLE_SPHERES, PARTICLE_NUM_MODES };

    void display(DisplayMode mode = PARTICLE_POINTS);
    void displayGrid();

    void setPointSize(float size) { m_pointSize = size; }
    void setParticleRadius(float r) { m_particleRadius = r; }
    void setFOV(float fov) { m_fov = fov; }
    void setWindowSize(int w, int h)
    {
        m_window_w = w;
        m_window_h = h;
    }

protected: // methods
    void   _initGL();
    void   _drawPoints();
    GLuint _compileProgram(const char *vsource, const char *fsource);

protected: // data
    float *m_pos;
    int    m_numParticles;

    float m_pointSize;
    float m_particleRadius;
    float m_fov;
    int   m_window_w, m_window_h;

    GLuint m_program;

    GLuint m_vbo;
    GLuint m_colorVBO;
};

#endif //__ RENDER_PARTICLES__

```

---
## High-Level Overview
This file is a h source file with 5 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **__RENDER_PARTICLES__**: `class ParticleRenderer`

### Functions
#### `void setColorBuffer(unsigned int vbo)`
- Function in Samples/2_Concepts_and_Techniques/particles/render_particles.h

#### `void setPointSize(float size)`
- Function in Samples/2_Concepts_and_Techniques/particles/render_particles.h

#### `void setParticleRadius(float r)`
- Function in Samples/2_Concepts_and_Techniques/particles/render_particles.h

#### `void setFOV(float fov)`
- Function in Samples/2_Concepts_and_Techniques/particles/render_particles.h

#### `void setWindowSize(int w, int h)`
- Function in Samples/2_Concepts_and_Techniques/particles/render_particles.h

### Classes / Structures
#### `class ParticleRenderer`
- Defined in Samples/2_Concepts_and_Techniques/particles/render_particles.h


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

