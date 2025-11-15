# Documentation: Samples/5_Domain_Specific/nbody/render_particles.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/nbody/render_particles.h`
- **Filename**: `render_particles.h`
- **Language**: h
- **Size**: 2878 bytes
- **Lines**: 80
- **Generated**: 2025-11-15 12:53:51 UTC

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
    void setPositions(double *pos, int numParticles);
    void setBaseColor(float color[4]);
    void setColors(float *color, int numParticles);
    void setPBO(unsigned int pbo, int numParticles, bool fp64);

    enum DisplayMode { PARTICLE_POINTS, PARTICLE_SPRITES, PARTICLE_SPRITES_COLOR, PARTICLE_NUM_MODES };

    void display(DisplayMode mode = PARTICLE_POINTS);

    void setPointSize(float size) { m_pointSize = size; }
    void setSpriteSize(float size) { m_spriteSize = size; }

    void resetPBO();

protected: // methods
    void _initGL();
    void _createTexture(int resolution);
    void _drawPoints(bool color = false);

protected: // data
    float  *m_pos;
    double *m_pos_fp64;
    int     m_numParticles;

    float m_pointSize;
    float m_spriteSize;

    unsigned int m_vertexShader;
    unsigned int m_vertexShaderPoints;
    unsigned int m_pixelShader;
    unsigned int m_programPoints;
    unsigned int m_programSprites;
    unsigned int m_texture;
    unsigned int m_pbo;
    unsigned int m_vboColor;

    float m_baseColor[4];

    bool m_bFp64Positions;
};

#endif //__ RENDER_PARTICLES__

```

---
## High-Level Overview
This file is a h source file with 2 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **__RENDER_PARTICLES__**: `class ParticleRenderer`

### Functions
#### `void setPointSize(float size)`
- Function in Samples/5_Domain_Specific/nbody/render_particles.h

#### `void setSpriteSize(float size)`
- Function in Samples/5_Domain_Specific/nbody/render_particles.h

### Classes / Structures
#### `class ParticleRenderer`
- Defined in Samples/5_Domain_Specific/nbody/render_particles.h


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

