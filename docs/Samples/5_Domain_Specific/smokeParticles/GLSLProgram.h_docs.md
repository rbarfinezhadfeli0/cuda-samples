# Documentation: Samples/5_Domain_Specific/smokeParticles/GLSLProgram.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/smokeParticles/GLSLProgram.h`
- **Filename**: `GLSLProgram.h`
- **Language**: h
- **Size**: 3014 bytes
- **Lines**: 72
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

// Simple class to contain GLSL shaders/programs

#ifndef GLSL_PROGRAM_H
#define GLSL_PROGRAM_H

#include <stdio.h>

class GLSLProgram
{
public:
    // construct program from strings
    GLSLProgram(const char *vsource, const char *fsource);
    GLSLProgram(const char *vsource,
                const char *gsource,
                const char *fsource,
                GLenum      gsInput  = GL_POINTS,
                GLenum      gsOutput = GL_TRIANGLE_STRIP);
    ~GLSLProgram();

    void enable();
    void disable();

    void setUniform1f(const GLchar *name, GLfloat x);
    void setUniform2f(const GLchar *name, GLfloat x, GLfloat y);
    void setUniform3f(const char *name, float x, float y, float z);
    void setUniform4f(const char *name, float x, float y, float z, float w);
    void setUniformfv(const GLchar *name, GLfloat *v, int elementSize, int count = 1);
    void setUniformMatrix4fv(const GLchar *name, GLfloat *m, bool transpose);

    void bindTexture(const char *name, GLuint tex, GLenum target, GLint unit);

    inline GLuint getProgId() { return mProg; }

private:
    GLuint checkCompileStatus(GLuint shader, GLint *status);
    GLuint compileProgram(const char *vsource,
                          const char *gsource,
                          const char *fsource,
                          GLenum      gsInput  = GL_POINTS,
                          GLenum      gsOutput = GL_TRIANGLE_STRIP);
    GLuint mProg;
};

#endif

```

---
## High-Level Overview
This file is a h source file with 1 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `stdio.h`

### Preprocessor Definitions
- **GLSL_PROGRAM_H**: `#include <stdio.h>`

### Functions
#### `GLuint getProgId()`
- Function in Samples/5_Domain_Specific/smokeParticles/GLSLProgram.h

### Classes / Structures
#### `class GLSLProgram`
- Defined in Samples/5_Domain_Specific/smokeParticles/GLSLProgram.h


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

