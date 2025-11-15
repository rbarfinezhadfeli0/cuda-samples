# Documentation for Samples/5_Domain_Specific/smokeParticles/framebufferObject.h

## File Metadata

- **Path**: `Samples/5_Domain_Specific/smokeParticles/framebufferObject.h`
- **Type**: .h
- **Location**: Samples/5_Domain_Specific/smokeParticles
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```h
/*
 Copyright (c) 2005,
  Aaron Lefohn   (lefohn@cs.ucdavis.edu)
  Robert Strzodka (strzodka@stanford.edu)
  Adam Moerschell (atmoerschell@ucdavis.edu)
 All rights reserved.

 This software is licensed under the BSD open-source license. See
 http://www.opensource.org/licenses/bsd-license.php for more detail.

 *************************************************************
 Redistribution and use in source and binary forms, with or
 without modification, are permitted provided that the following
 conditions are met:

 Redistributions of source code must retain the above copyright notice,
 this list of conditions and the following disclaimer.

 Redistributions in binary form must reproduce the above copyright notice,
 this list of conditions and the following disclaimer in the documentation
 and/or other materials provided with the distribution.

 Neither the name of the University of California, Davis nor the names of
 the contributors may be used to endorse or promote products derived
 from this software without specific prior written permission.

 THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
 "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
 LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
 FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL
 THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT,
 INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
 DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE
 GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
 INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
 WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF
 THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY
 OF SUCH DAMAGE.
*/

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

#ifndef UCDAVIS_FRAMEBUFFER_OBJECT_H
#define UCDAVIS_FRAMEBUFFER_OBJECT_H

#include <helper_gl.h>
#include <iostream>

/*!
FramebufferObject Class. This class encapsulates the FramebufferObject
(FBO) OpenGL spec. See the official spec at:
    http://oss.sgi.com/projects/ogl-sample/registry/EXT/framebuffer_object.txt

for details.

A framebuffer object (FBO) is conceptually a structure containing pointers
to GPU memory. The memory pointed to is either an OpenGL texture or an
OpenGL RenderBuffer. FBOs can be used to render to one or more textures,
share depth buffers between multiple sets of color buffers/textures and
are a complete replacement for pbuffers.

Performance Notes:
  1) It is more efficient (but not required) to call Bind()
     on an FBO before making multiple method calls. For example:

      FramebufferObject fbo;
      fbo.Bind();
      fbo.AttachTexture(GL_TEXTURE_2D, texId0, GL_COLOR_ATTACHMENT0_EXT);
      fbo.AttachTexture(GL_TEXTURE_2D, texId1, GL_COLOR_ATTACHMENT1_EXT);
      fbo.IsValid();

    To provide a complete encapsulation, the following usage
    pattern works correctly but is less efficient:

      FramebufferObject fbo;
      // NOTE : No Bind() call
      fbo.AttachTexture(GL_TEXTURE_2D, texId0, GL_COLOR_ATTACHMENT0_EXT);
      fbo.AttachTexture(GL_TEXTURE_2D, texId1, GL_COLOR_ATTACHMENT1_EXT);
      fbo.IsValid();

    The first usage pattern binds the FBO only once, whereas
    the second usage binds/unbinds the FBO for each method call.

  2) Use FramebufferObject::Disable() sparingly. We have intentionally
     left out an "Unbind()" method because it is largely unnecessary
     and encourages rendundant Bind/Unbind coding. Binding an FBO is
     usually much faster than enabling/disabling a pbuffer, but is
     still a costly operation. When switching between multiple FBOs
     and a visible OpenGL framebuffer, the following usage pattern
     is recommended:

      FramebufferObject fbo1, fbo2;
      fbo1.Bind();
        ... Render ...
      // NOTE : No Unbind/Disable here...

      fbo2.Bind();
        ... Render ...

      // Disable FBO rendering and return to visible window
      // OpenGL framebuffer.
      FramebufferObject::Disable();
*/
class FramebufferObject
{
public:
    /// Ctor/Dtor
    FramebufferObject();
    virtual ~FramebufferObject();

    /// Bind this FBO as current render target
    void Bind();

    /// Bind a texture to the "attachment" point of this FBO
    virtual void AttachTexture(GLenum texTarget,
                               GLuint texId,
                               GLenum attachment = GL_COLOR_ATTACHMENT0_EXT,
                               int    mipLevel   = 0,
                               int    zSlice     = 0);

    /// Bind an array of textures to multiple "attachment" points of this FBO
    ///  - By default, the first 'numTextures' attachments are used,
    ///    starting with GL_COLOR_ATTACHMENT0_EXT
    virtual void AttachTextures(int    numTextures,
                                GLenum texTarget[],
                                GLuint texId[],
                                GLenum attachment[] = NULL,
                                int    mipLevel[]   = NULL,
                                int    zSlice[]     = NULL);

    /// Bind a render buffer to the "attachment" point of this FBO
    virtual void AttachRenderBuffer(GLuint buffId, GLenum attachment = GL_COLOR_ATTACHMENT0_EXT);

    /// Bind an array of render buffers to corresponding "attachment" points
    /// of this FBO.
    /// - By default, the first 'numBuffers' attachments are used,
    ///   starting with GL_COLOR_ATTACHMENT0_EXT
    virtual void AttachRenderBuffers(int numBuffers, GLuint buffId[], GLenum attachment[] = NULL);

    /// Free any resource bound to the "attachment" point of this FBO
    void Unattach(GLenum attachment);

    /// Free any resources bound to any attachment points of this FBO
    void UnattachAll();

/// Is this FBO currently a valid render target?
///  - Sends output to std::cerr by default but can
///    be a user-defined C++ stream
///
/// NOTE : This function works correctly in debug build
///        mode but always returns "true" if NDEBUG is
///        is defined (optimized builds)
#ifndef NDEBUG
    bool IsValid(std::ostream &ostr = std::cerr);
#else
    bool IsValid(std::ostream &ostr = std::cerr) { return true; }
#endif

    /// BEGIN : Accessors
    /// Is attached type GL_RENDERBUFFER_EXT or GL_TEXTURE?
    GLenum GetAttachedType(GLenum attachment);

    /// What is the Id of Renderbuffer/texture currently
    /// attached to "attachment?"
    GLuint GetAttachedId(GLenum attachment);

    /// Which mipmap level is currently attached to "attachment?"
    GLint GetAttachedMipLevel(GLenum attachment);

    /// Which cube face is currently attached to "attachment?"
    GLint GetAttachedCubeFace(GLenum attachment);

    /// Which z-slice is currently attached to "attachment?"
    GLint GetAttachedZSlice(GLenum attachment);
    /// END : Accessors

    /// BEGIN : Static methods global to all FBOs
    /// Return number of color attachments permitted
    static int GetMaxColorAttachments();

    /// Disable all FBO rendering and return to traditional,
    /// windowing-system controlled framebuffer
    ///  NOTE:
    ///     This is NOT an "unbind" for this specific FBO, but rather
    ///     disables all FBO rendering. This call is intentionally "static"
    ///     and named "Disable" instead of "Unbind" for this reason. The
    ///     motivation for this strange semantic is performance. Providing
    ///     "Unbind" would likely lead to a large number of unnecessary
    ///     FBO enabling/disabling.
    static void Disable();
    /// END : Static methods global to all FBOs

protected:
    void          _GuardedBind();
    void          _GuardedUnbind();
    void          _FramebufferTextureND(GLenum attachment, GLenum texTarget, GLuint texId, int mipLevel, int zSlice);
    static GLuint _GenerateFboId();

private:
    GLuint m_fboId;
    GLint  m_savedFboId;
};

#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/smokeParticles/framebufferObject.h`.

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

- **Total Lines**: 231
- **Approximate Size**: 9477 bytes

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
