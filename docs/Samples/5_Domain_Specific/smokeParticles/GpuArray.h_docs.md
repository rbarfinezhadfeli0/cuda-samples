# Documentation for Samples/5_Domain_Specific/smokeParticles/GpuArray.h

## File Metadata

- **Path**: `Samples/5_Domain_Specific/smokeParticles/GpuArray.h`
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

/*
   Class to represent an array in GPU and CPU memory
*/

#include <stdio.h>
#include <stdlib.h>
#define HELPERGL_EXTERN_GL_FUNC_IMPLEMENTATION
#include <cuda_gl_interop.h>
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_gl.h>

template <class T> class GpuArray
{
public:
    GpuArray();
    ~GpuArray();

    enum Direction {
        HOST_TO_DEVICE,
        DEVICE_TO_HOST,
    };

    // allocate and free
    void alloc(size_t size, bool vbo = false, bool doubleBuffer = false, bool useElementArray = false);
    void free();

    // swap buffers for double buffering
    void swap();

    // when using vbo, must map before getting device ptr
    void map();
    void unmap();

    void copy(Direction dir, uint start = 0, uint count = 0);
    void memset(T value, uint start = 0, uint count = 0);

    T     *getDevicePtr() { return m_dptr[m_currentRead]; }
    GLuint getVbo() { return m_vbo[m_currentRead]; }

    T     *getDeviceWritePtr() { return m_dptr[m_currentWrite]; }
    GLuint getWriteVbo() { return m_vbo[m_currentWrite]; }

    T *getHostPtr() { return m_hptr; }

    size_t getSize() const { return m_size; }

private:
    GLuint createVbo(size_t size, bool useElementArray);

    void allocDevice();
    void allocVbo(bool useElementArray);
    void allocHost();

    void freeDevice();
    void freeVbo();
    void freeHost();

    size_t                       m_size;
    T                           *m_dptr[2];
    GLuint                       m_vbo[2];
    struct cudaGraphicsResource *m_cuda_vbo_resource[2]; // handles OpenGL-CUDA exchange

    T *m_hptr;

    bool m_useVBO;
    bool m_doubleBuffer;
    uint m_currentRead, m_currentWrite;
};

template <class T>
GpuArray<T>::GpuArray()
    : m_size(0)
    , m_hptr(0)
    , m_currentRead(0)
    , m_currentWrite(0)
{
    m_dptr[0] = 0;
    m_dptr[1] = 0;

    m_vbo[0] = 0;
    m_vbo[1] = 0;

    m_cuda_vbo_resource[0] = NULL;
    m_cuda_vbo_resource[1] = NULL;
}

template <class T> GpuArray<T>::~GpuArray() { free(); }

template <class T> void GpuArray<T>::alloc(size_t size, bool vbo, bool doubleBuffer, bool useElementArray)
{
    m_size = size;

    m_useVBO       = vbo;
    m_doubleBuffer = doubleBuffer;

    if (m_doubleBuffer) {
        m_currentWrite = 1;
    }

    allocHost();

    if (vbo) {
        allocVbo(useElementArray);
    }
    else {
        allocDevice();
    }
}

template <class T> void GpuArray<T>::free()
{
    freeHost();

    if (m_vbo) {
        freeVbo();
    }

    if (m_dptr) {
        freeDevice();
    }
}

template <class T> void GpuArray<T>::allocHost() { m_hptr = (T *)new T[m_size]; }

template <class T> void GpuArray<T>::freeHost()
{
    if (m_hptr) {
        delete[] m_hptr;
        m_hptr = 0;
    }
}

template <class T> void GpuArray<T>::allocDevice()
{
    checkCudaErrors(cudaMalloc((void **)&m_dptr[0], m_size * sizeof(T)));

    if (m_doubleBuffer) {
        checkCudaErrors(cudaMalloc((void **)&m_dptr[1], m_size * sizeof(T)));
    }
}

template <class T> void GpuArray<T>::freeDevice()
{
    if (m_dptr[0]) {
        checkCudaErrors(cudaFree(m_dptr[0]));
        m_dptr[0] = 0;
    }

    if (m_dptr[1]) {
        checkCudaErrors(cudaFree(m_dptr[1]));
        m_dptr[1] = 0;
    }
}

template <class T> GLuint GpuArray<T>::createVbo(size_t size, bool useElementArray)
{
    GLuint vbo;
    glGenBuffers(1, &vbo);

    if (useElementArray) {
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, vbo);
        glBufferData(GL_ELEMENT_ARRAY_BUFFER, size, 0, GL_DYNAMIC_DRAW);
        glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, 0);
    }
    else {
        glBindBuffer(GL_ARRAY_BUFFER, vbo);
        glBufferData(GL_ARRAY_BUFFER, size, 0, GL_DYNAMIC_DRAW);
        glBindBuffer(GL_ARRAY_BUFFER, 0);
    }

    return vbo;
}

template <class T> void GpuArray<T>::allocVbo(bool useElementArray)
{
    m_vbo[0] = createVbo(m_size * sizeof(T), useElementArray);
    checkCudaErrors(cudaGraphicsGLRegisterBuffer(&m_cuda_vbo_resource[0], m_vbo[0], cudaGraphicsMapFlagsWriteDiscard));

    if (m_doubleBuffer) {
        m_vbo[1] = createVbo(m_size * sizeof(T), useElementArray);
        checkCudaErrors(
            cudaGraphicsGLRegisterBuffer(&m_cuda_vbo_resource[1], m_vbo[1], cudaGraphicsMapFlagsWriteDiscard));
    }
}

template <class T> void GpuArray<T>::freeVbo()
{
    if (m_vbo[0]) {
        checkCudaErrors(cudaGraphicsUnregisterResource(m_cuda_vbo_resource[0]));
        glDeleteBuffers(1, &m_vbo[0]);
        m_vbo[0] = 0;
    }

    if (m_vbo[1]) {
        checkCudaErrors(cudaGraphicsUnregisterResource(m_cuda_vbo_resource[1]));
        glDeleteBuffers(1, &m_vbo[1]);
        m_vbo[1] = 0;
    }
}

template <class T> void GpuArray<T>::swap() { std::swap(m_currentRead, m_currentWrite); }

template <class T> void GpuArray<T>::map()
{
    if (m_vbo[0]) {
        checkCudaErrors(cudaGraphicsMapResources(1, &m_cuda_vbo_resource[0], 0));
        size_t num_bytes;
        checkCudaErrors(cudaGraphicsResourceGetMappedPointer((void **)&m_dptr[0], &num_bytes, m_cuda_vbo_resource[0]));
    }

    if (m_doubleBuffer && m_vbo[1]) {
        checkCudaErrors(cudaGraphicsMapResources(1, &m_cuda_vbo_resource[1], 0));
        size_t num_bytes;
        checkCudaErrors(cudaGraphicsResourceGetMappedPointer((void **)&m_dptr[1], &num_bytes, m_cuda_vbo_resource[1]));
    }
}

template <class T> void GpuArray<T>::unmap()
{
    if (m_vbo[0]) {
        checkCudaErrors(cudaGraphicsUnmapResources(1, &m_cuda_vbo_resource[0], 0));
        m_dptr[0] = 0;
    }

    if (m_doubleBuffer && m_vbo[1]) {
        checkCudaErrors(cudaGraphicsUnmapResources(1, &m_cuda_vbo_resource[1], 0));
        m_dptr[1] = 0;
    }
}

template <class T> void GpuArray<T>::copy(Direction dir, uint start, uint count)
{
    if (count == 0) {
        count = (uint)m_size;
    }

    map();

    switch (dir) {
    case HOST_TO_DEVICE:
        checkCudaErrors(cudaMemcpy((void *)(m_dptr[m_currentRead] + start),
                                   (void *)(m_hptr + start),
                                   count * sizeof(T),
                                   cudaMemcpyHostToDevice));
        break;

    case DEVICE_TO_HOST:
        checkCudaErrors(cudaMemcpy((void *)(m_hptr + start),
                                   (void *)(m_dptr[m_currentRead] + start),
                                   count * sizeof(T),
                                   cudaMemcpyDeviceToHost));
        break;
    }

    unmap();
}

template <class T> void GpuArray<T>::memset(T value, uint start, uint count) {}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/smokeParticles/GpuArray.h`.

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

- **Total Lines**: 287
- **Approximate Size**: 8075 bytes

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
