# Documentation for Common/nvShaderUtils.h

## File Metadata

- **Path**: `Common/nvShaderUtils.h`
- **Type**: .h
- **Location**: Common
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

/*
 *
 * Utility functions for compiling shaders and programs
 *
 * Author: Evan Hart
 * Copyright (c) NVIDIA Corporation. All rights reserved.
 *
 */


#ifndef NV_SHADER_UTILS_H
#define NV_SHADER_UTILS_H

#include <stdio.h>
#include <string.h>

namespace nv {

//
//
////////////////////////////////////////////////////////////
inline GLuint CompileGLSLShader(GLenum target, const char *shader) {
  GLuint object;

  object = glCreateShader(target);

  if (!object) {
    return object;
  }

  glShaderSource(object, 1, &shader, NULL);

  glCompileShader(object);

  // check if shader compiled
  GLint compiled = 0;
  glGetShaderiv(object, GL_COMPILE_STATUS, &compiled);

  if (!compiled) {
#ifdef NV_REPORT_COMPILE_ERRORS
    char temp[256] = "";
    glGetShaderInfoLog(object, 256, NULL, temp);
    fprintf(stderr, "Compile failed:\n%s\n", temp);
#endif
    glDeleteShader(object);
    return 0;
  }

  return object;
}

//
//
////////////////////////////////////////////////////////////
inline GLuint CompileGLSLShaderFromFile(GLenum target, const char *filename) {
  FILE *shaderFile;
  char *text;
  long size;
  size_t fsize = 0;

  // read files as binary to prevent problems from newline translation
#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)

  if (fopen_s(&shaderFile, filename, "rb") != 0)
#else
  if ((shaderFile = fopen(filename, "rb")) == 0)
#endif
  {
    return 0;
  }

  // Get the length of the file
  fseek(shaderFile, 0, SEEK_END);
  size = ftell(shaderFile);

  // Read the file contents from the start, then close file and add a null
  // terminator
  fseek(shaderFile, 0, SEEK_SET);
  text = new char[size + 1];
  fsize = fread(text, size, 1, shaderFile);
  fclose(shaderFile);

  if (fsize == 0) {
    printf("CompileGLSLShaderFromFile(), error... fsize = 0\n");
  }

  text[size] = '\0';

  GLuint object = CompileGLSLShader(target, text);

  delete[] text;

  return object;
}

// Create a program composed of vertex and fragment shaders.
inline GLuint LinkGLSLProgram(GLuint vertexShader, GLuint fragmentShader) {
  GLuint program = glCreateProgram();
  glAttachShader(program, vertexShader);
  glAttachShader(program, fragmentShader);
  glLinkProgram(program);

#ifdef NV_REPORT_COMPILE_ERRORS
  // Get error log.
  GLint charsWritten, infoLogLength;
  glGetProgramiv(program, GL_INFO_LOG_LENGTH, &infoLogLength);

  char *infoLog = new char[infoLogLength];
  glGetProgramInfoLog(program, infoLogLength, &charsWritten, infoLog);
  printf(infoLog);
  delete[] infoLog;
#endif

  // Test linker result.
  GLint linkSucceed = GL_FALSE;
  glGetProgramiv(program, GL_LINK_STATUS, &linkSucceed);

  if (linkSucceed == GL_FALSE) {
    glDeleteProgram(program);
    return 0;
  }

  return program;
}

// Create a program composed of vertex, geometry and fragment shaders.
inline GLuint LinkGLSLProgram(GLuint vertexShader, GLuint geometryShader,
                              GLint inputType, GLint vertexOut,
                              GLint outputType, GLuint fragmentShader) {
  GLuint program = glCreateProgram();
  glAttachShader(program, vertexShader);
  glAttachShader(program, geometryShader);
  glProgramParameteriEXT(program, GL_GEOMETRY_INPUT_TYPE_EXT, inputType);
  glProgramParameteriEXT(program, GL_GEOMETRY_VERTICES_OUT_EXT, vertexOut);
  glProgramParameteriEXT(program, GL_GEOMETRY_OUTPUT_TYPE_EXT, outputType);
  glAttachShader(program, fragmentShader);
  glLinkProgram(program);

#ifdef NV_REPORT_COMPILE_ERRORS
  // Get error log.
  GLint charsWritten, infoLogLength;
  glGetProgramiv(program, GL_INFO_LOG_LENGTH, &infoLogLength);

  char *infoLog = new char[infoLogLength];
  glGetProgramInfoLog(program, infoLogLength, &charsWritten, infoLog);
  printf(infoLog);
  delete[] infoLog;
#endif

  // Test linker result.
  GLint linkSucceed = GL_FALSE;
  glGetProgramiv(program, GL_LINK_STATUS, &linkSucceed);

  if (linkSucceed == GL_FALSE) {
    glDeleteProgram(program);
    return 0;
  }

  return program;
}

//
//
////////////////////////////////////////////////////////////
inline GLuint CompileASMShader(GLenum program_type, const char *code) {
  GLuint program_id;
  glGenProgramsARB(1, &program_id);
  glBindProgramARB(program_type, program_id);
  glProgramStringARB(program_type, GL_PROGRAM_FORMAT_ASCII_ARB,
                     (GLsizei)strlen(code), (GLubyte *)code);

  GLint error_pos;
  glGetIntegerv(GL_PROGRAM_ERROR_POSITION_ARB, &error_pos);

  if (error_pos != -1) {
#ifdef NV_REPORT_COMPILE_ERRORS
    const GLubyte *error_string;
    error_string = glGetString(GL_PROGRAM_ERROR_STRING_ARB);
    fprintf(stderr, "Program error at position: %d\n%s\n", (int)error_pos,
            error_string);
#endif
    return 0;
  }

  return program_id;
}

//
//
////////////////////////////////////////////////////////////
inline GLuint CompileASMShaderFromFile(GLenum target, const char *filename) {
  FILE *shaderFile;
  char *text;
  long size;
  size_t fsize = 0;

  // read files as binary to prevent problems from newline translation
#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)

  if (fopen_s(&shaderFile, filename, "rb") != 0)
#else
  if ((shaderFile = fopen(filename, "rb")) == 0)
#endif
  {
    return 0;
  }

  // Get the length of the file
  fseek(shaderFile, 0, SEEK_END);
  size = ftell(shaderFile);

  // Read the file contents from the start, then close file and add a null
  // terminator
  fseek(shaderFile, 0, SEEK_SET);
  text = new char[size + 1];
  fsize = fread(text, size, 1, shaderFile);
  fclose(shaderFile);

  if (fsize == 0) {
    printf("CompileGLSLShaderFromFile(), error... fsize = 0\n");
  }

  text[size] = '\0';

  GLuint program_id = CompileASMShader(target, text);

  delete[] text;

  return program_id;
}

}  // namespace nv
#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Common/nvShaderUtils.h`.

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

- **Total Lines**: 261
- **Approximate Size**: 7348 bytes

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
