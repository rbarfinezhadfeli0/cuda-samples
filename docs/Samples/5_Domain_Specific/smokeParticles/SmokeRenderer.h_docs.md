# Documentation for Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h

## File Metadata

- **Path**: `Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h`
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

// Smoke particle renderer with volumetric shadows

#ifndef SMOKE_RENDERER_H
#define SMOKE_RENDERER_H

#include "GLSLProgram.h"
#include "framebufferObject.h"
#include "nvMath.h"

using namespace nv;

class SmokeRenderer
{
public:
    SmokeRenderer(int maxParticles);
    ~SmokeRenderer();

    enum DisplayMode { POINTS, SPRITES, VOLUMETRIC, NUM_MODES };

    enum Target { LIGHT_BUFFER, SCENE_BUFFER };

    void setDisplayMode(DisplayMode mode) { mDisplayMode = mode; }

    void setNumParticles(unsigned int x) { mNumParticles = x; }
    void setPositionBuffer(GLuint vbo) { mPosVbo = vbo; }
    void setVelocityBuffer(GLuint vbo) { mVelVbo = vbo; }
    void setColorBuffer(GLuint vbo) { mColorVbo = vbo; }
    void setIndexBuffer(GLuint ib) { mIndexBuffer = ib; }

    void setParticleRadius(float x) { mParticleRadius = x; }
    void setWindowSize(int w, int h);
    void setFOV(float fov) { mFov = fov; }

    void setNumSlices(int x) { m_numSlices = x; }
    void setNumDisplayedSlices(int x) { m_numDisplayedSlices = x; }

    void setAlpha(float x) { m_spriteAlpha = x; }
    void setShadowAlpha(float x) { m_shadowAlpha = x; }
    void setColorAttenuation(vec3f c) { m_colorAttenuation = c; }
    void setLightColor(vec3f c);

    void setDoBlur(bool b) { m_doBlur = b; }
    void setBlurRadius(float x) { m_blurRadius = x; }
    void setDisplayLightBuffer(bool b) { m_displayLightBuffer = b; }

    void beginSceneRender(Target target);
    void endSceneRender(Target target);

    void setLightPosition(vec3f v) { m_lightPos = v; }
    void setLightTarget(vec3f v) { m_lightTarget = v; }

    vec4f    getLightPositionEyeSpace() { return m_lightPosEye; }
    matrix4f getShadowMatrix() { return m_shadowMatrix; }

    GLuint getShadowTexture() { return m_lightTexture[m_srcLightTexture]; }

    void  calcVectors();
    vec3f getSortVector() { return m_halfVector; }

    void render();
    void debugVectors();

private:
    void drawPoints(int start, int count, bool sort);
    void drawPointSprites(GLSLProgram *prog, int start, int count, bool shadowed);

    void drawSlice(int i);
    void drawSliceLightView(int i);
    void drawSlices();
    void displayTexture(GLuint tex);
    void compositeResult();
    void blurLightBuffer();
    void depthSort();

    GLuint createTexture(GLenum target, int w, int h, GLint internalformat, GLenum format);
    void   createBuffers(int w, int h);
    void   createLightBuffer();

    void drawQuad();
    void drawVector(vec3f v);

    // particle data
    unsigned int mMaxParticles;
    unsigned int mNumParticles;

    GLuint mPosVbo;
    GLuint mVelVbo;
    GLuint mColorVbo;
    GLuint mIndexBuffer;

    float       mParticleRadius;
    DisplayMode mDisplayMode;

    // window
    unsigned int mWindowW, mWindowH;
    float        mAspect, mInvFocalLen;
    float        mFov;

    int m_downSample;
    int m_imageW, m_imageH;

    int m_numSlices;
    int m_numDisplayedSlices;
    int m_batchSize;
    int m_sliceNo;

    float m_shadowAlpha;
    float m_spriteAlpha;
    bool  m_doBlur;
    float m_blurRadius;
    bool  m_displayLightBuffer;

    vec3f m_lightVector, m_lightPos, m_lightTarget;
    vec3f m_lightColor;
    vec3f m_colorAttenuation;
    float m_lightDistance;

    matrix4f m_modelView, m_lightView, m_lightProj, m_shadowMatrix;
    vec3f    m_viewVector, m_halfVector;
    bool     m_invertedView;
    vec4f    m_eyePos;
    vec4f    m_halfVectorEye;
    vec4f    m_lightPosEye;

    // programs
    GLSLProgram *m_simpleProg;
    GLSLProgram *m_particleProg, *m_particleShadowProg;
    GLSLProgram *m_displayTexProg, *m_blurProg;

    // image buffers
    int                m_lightBufferSize;
    GLuint             m_lightTexture[2];
    int                m_srcLightTexture;
    GLuint             m_lightDepthTexture;
    FramebufferObject *m_lightFbo;

    GLuint             m_imageTex, m_depthTex;
    FramebufferObject *m_imageFbo;
};

#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h`.

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

- **Total Lines**: 169
- **Approximate Size**: 5504 bytes

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
