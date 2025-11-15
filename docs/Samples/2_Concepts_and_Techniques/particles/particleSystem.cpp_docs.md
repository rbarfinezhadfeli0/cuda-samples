# Documentation for Samples/2_Concepts_and_Techniques/particles/particleSystem.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/particles/particleSystem.cpp`
- **Type**: .cpp
- **Location**: Samples/2_Concepts_and_Techniques/particles
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp
/*
 * Copyright 1993-2015 NVIDIA Corporation.  All rights reserved.
 *
 * Please refer to the NVIDIA end user license agreement (EULA) associated
 * with this source code for terms and conditions that govern your use of
 * this software. Any use, reproduction, disclosure, or distribution of
 * this software and related documentation outside the terms of the EULA
 * is strictly prohibited.
 *
 */

// OpenGL Graphics includes
#define HELPERGL_EXTERN_GL_FUNC_IMPLEMENTATION
#include "particleSystem.h"

#include <algorithm>
#include <assert.h>
#include <cstdio>
#include <cstdlib>
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_functions.h>
#include <helper_gl.h>
#include <math.h>
#include <memory.h>

#include "particleSystem.cuh"
#include "particles_kernel.cuh"

#ifndef CUDART_PI_F
#define CUDART_PI_F 3.141592654f
#endif

ParticleSystem::ParticleSystem(uint numParticles, uint3 gridSize, bool bUseOpenGL)
    : m_bInitialized(false)
    , m_bUseOpenGL(bUseOpenGL)
    , m_numParticles(numParticles)
    , m_hPos(0)
    , m_hVel(0)
    , m_dPos(0)
    , m_dVel(0)
    , m_gridSize(gridSize)
    , m_timer(NULL)
    , m_solverIterations(1)
{
    m_numGridCells = m_gridSize.x * m_gridSize.y * m_gridSize.z;
    //    float3 worldSize = make_float3(2.0f, 2.0f, 2.0f);

    m_gridSortBits = 18; // increase this for larger grids

    // set simulation parameters
    m_params.gridSize  = m_gridSize;
    m_params.numCells  = m_numGridCells;
    m_params.numBodies = m_numParticles;

    m_params.particleRadius = 1.0f / 64.0f;
    m_params.colliderPos    = make_float3(-1.2f, -0.8f, 0.8f);
    m_params.colliderRadius = 0.2f;

    m_params.worldOrigin = make_float3(-1.0f, -1.0f, -1.0f);
    //    m_params.cellSize = make_float3(worldSize.x / m_gridSize.x, worldSize.y / m_gridSize.y, worldSize.z /
    //    m_gridSize.z);
    float cellSize    = m_params.particleRadius * 2.0f; // cell size equal to particle diameter
    m_params.cellSize = make_float3(cellSize, cellSize, cellSize);

    m_params.spring          = 0.5f;
    m_params.damping         = 0.02f;
    m_params.shear           = 0.1f;
    m_params.attraction      = 0.0f;
    m_params.boundaryDamping = -0.5f;

    m_params.gravity       = make_float3(0.0f, -0.0003f, 0.0f);
    m_params.globalDamping = 1.0f;

    _initialize(numParticles);
}

ParticleSystem::~ParticleSystem()
{
    _finalize();
    m_numParticles = 0;
}

uint ParticleSystem::createVBO(uint size)
{
    GLuint vbo;
    glGenBuffers(1, &vbo);
    glBindBuffer(GL_ARRAY_BUFFER, vbo);
    glBufferData(GL_ARRAY_BUFFER, size, 0, GL_DYNAMIC_DRAW);
    glBindBuffer(GL_ARRAY_BUFFER, 0);
    return vbo;
}

inline float lerp(float a, float b, float t) { return a + t * (b - a); }

// create a color ramp
void colorRamp(float t, float *r)
{
    const int ncolors       = 7;
    float     c[ncolors][3] = {
        {
            1.0,
            0.0,
            0.0,
        },
        {
            1.0,
            0.5,
            0.0,
        },
        {
            1.0,
            1.0,
            0.0,
        },
        {
            0.0,
            1.0,
            0.0,
        },
        {
            0.0,
            1.0,
            1.0,
        },
        {
            0.0,
            0.0,
            1.0,
        },
        {
            1.0,
            0.0,
            1.0,
        },
    };
    t       = t * (ncolors - 1);
    int   i = (int)t;
    float u = t - floorf(t);
    r[0]    = lerp(c[i][0], c[i + 1][0], u);
    r[1]    = lerp(c[i][1], c[i + 1][1], u);
    r[2]    = lerp(c[i][2], c[i + 1][2], u);
}

void ParticleSystem::_initialize(int numParticles)
{
    assert(!m_bInitialized);

    m_numParticles = numParticles;

    // allocate host storage
    m_hPos = new float[m_numParticles * 4];
    m_hVel = new float[m_numParticles * 4];
    memset(m_hPos, 0, m_numParticles * 4 * sizeof(float));
    memset(m_hVel, 0, m_numParticles * 4 * sizeof(float));

    m_hCellStart = new uint[m_numGridCells];
    memset(m_hCellStart, 0, m_numGridCells * sizeof(uint));

    m_hCellEnd = new uint[m_numGridCells];
    memset(m_hCellEnd, 0, m_numGridCells * sizeof(uint));

    // allocate GPU data
    unsigned int memSize = sizeof(float) * 4 * m_numParticles;

    if (m_bUseOpenGL) {
        m_posVbo = createVBO(memSize);
        registerGLBufferObject(m_posVbo, &m_cuda_posvbo_resource);
    }
    else {
        checkCudaErrors(cudaMalloc((void **)&m_cudaPosVBO, memSize));
    }

    allocateArray((void **)&m_dVel, memSize);

    allocateArray((void **)&m_dSortedPos, memSize);
    allocateArray((void **)&m_dSortedVel, memSize);

    allocateArray((void **)&m_dGridParticleHash, m_numParticles * sizeof(uint));
    allocateArray((void **)&m_dGridParticleIndex, m_numParticles * sizeof(uint));

    allocateArray((void **)&m_dCellStart, m_numGridCells * sizeof(uint));
    allocateArray((void **)&m_dCellEnd, m_numGridCells * sizeof(uint));

    if (m_bUseOpenGL) {
        m_colorVBO = createVBO(m_numParticles * 4 * sizeof(float));
        registerGLBufferObject(m_colorVBO, &m_cuda_colorvbo_resource);

        // fill color buffer
        glBindBuffer(GL_ARRAY_BUFFER, m_colorVBO);
        float *data = (float *)glMapBuffer(GL_ARRAY_BUFFER, GL_WRITE_ONLY);
        float *ptr  = data;

        for (uint i = 0; i < m_numParticles; i++) {
            float t = i / (float)m_numParticles;
#if 0
            *ptr++ = rand() / (float) RAND_MAX;
            *ptr++ = rand() / (float) RAND_MAX;
            *ptr++ = rand() / (float) RAND_MAX;
#else
            colorRamp(t, ptr);
            ptr += 3;
#endif
            *ptr++ = 1.0f;
        }

        glUnmapBuffer(GL_ARRAY_BUFFER);
    }
    else {
        checkCudaErrors(cudaMalloc((void **)&m_cudaColorVBO, sizeof(float) * numParticles * 4));
    }

    sdkCreateTimer(&m_timer);

    setParameters(&m_params);

    m_bInitialized = true;
}

void ParticleSystem::_finalize()
{
    assert(m_bInitialized);

    delete[] m_hPos;
    delete[] m_hVel;
    delete[] m_hCellStart;
    delete[] m_hCellEnd;

    freeArray(m_dVel);
    freeArray(m_dSortedPos);
    freeArray(m_dSortedVel);

    freeArray(m_dGridParticleHash);
    freeArray(m_dGridParticleIndex);
    freeArray(m_dCellStart);
    freeArray(m_dCellEnd);

    if (m_bUseOpenGL) {
        unregisterGLBufferObject(m_cuda_colorvbo_resource);
        unregisterGLBufferObject(m_cuda_posvbo_resource);
        glDeleteBuffers(1, (const GLuint *)&m_posVbo);
        glDeleteBuffers(1, (const GLuint *)&m_colorVBO);
    }
    else {
        checkCudaErrors(cudaFree(m_cudaPosVBO));
        checkCudaErrors(cudaFree(m_cudaColorVBO));
    }
}

// step the simulation
void ParticleSystem::update(float deltaTime)
{
    assert(m_bInitialized);

    float *dPos;

    if (m_bUseOpenGL) {
        dPos = (float *)mapGLBufferObject(&m_cuda_posvbo_resource);
    }
    else {
        dPos = (float *)m_cudaPosVBO;
    }

    // update constants
    setParameters(&m_params);

    // integrate
    integrateSystem(dPos, m_dVel, deltaTime, m_numParticles);

    // calculate grid hash
    calcHash(m_dGridParticleHash, m_dGridParticleIndex, dPos, m_numParticles);

    // sort particles based on hash
    sortParticles(m_dGridParticleHash, m_dGridParticleIndex, m_numParticles);

    // reorder particle arrays into sorted order and
    // find start and end of each cell
    reorderDataAndFindCellStart(m_dCellStart,
                                m_dCellEnd,
                                m_dSortedPos,
                                m_dSortedVel,
                                m_dGridParticleHash,
                                m_dGridParticleIndex,
                                dPos,
                                m_dVel,
                                m_numParticles,
                                m_numGridCells);

    // process collisions
    collide(m_dVel,
            m_dSortedPos,
            m_dSortedVel,
            m_dGridParticleIndex,
            m_dCellStart,
            m_dCellEnd,
            m_numParticles,
            m_numGridCells);

    // note: do unmap at end here to avoid unnecessary graphics/CUDA context switch
    if (m_bUseOpenGL) {
        unmapGLBufferObject(m_cuda_posvbo_resource);
    }
}

void ParticleSystem::dumpGrid()
{
    // dump grid information
    copyArrayFromDevice(m_hCellStart, m_dCellStart, 0, sizeof(uint) * m_numGridCells);
    copyArrayFromDevice(m_hCellEnd, m_dCellEnd, 0, sizeof(uint) * m_numGridCells);
    uint maxCellSize = 0;

    for (uint i = 0; i < m_numGridCells; i++) {
        if (m_hCellStart[i] != 0xffffffff) {
            uint cellSize = m_hCellEnd[i] - m_hCellStart[i];

            //            printf("cell: %d, %d particles\n", i, cellSize);
            if (cellSize > maxCellSize) {
                maxCellSize = cellSize;
            }
        }
    }

    printf("maximum particles per cell = %d\n", maxCellSize);
}

void ParticleSystem::dumpParticles(uint start, uint count)
{
    // debug
    copyArrayFromDevice(m_hPos, 0, &m_cuda_posvbo_resource, sizeof(float) * 4 * count);
    copyArrayFromDevice(m_hVel, m_dVel, 0, sizeof(float) * 4 * count);

    for (uint i = start; i < start + count; i++) {
        //        printf("%d: ", i);
        printf("pos: (%.4f, %.4f, %.4f, %.4f)\n",
               m_hPos[i * 4 + 0],
               m_hPos[i * 4 + 1],
               m_hPos[i * 4 + 2],
               m_hPos[i * 4 + 3]);
        printf("vel: (%.4f, %.4f, %.4f, %.4f)\n",
               m_hVel[i * 4 + 0],
               m_hVel[i * 4 + 1],
               m_hVel[i * 4 + 2],
               m_hVel[i * 4 + 3]);
    }
}

float *ParticleSystem::getArray(ParticleArray array)
{
    assert(m_bInitialized);

    float                       *hdata             = 0;
    float                       *ddata             = 0;
    struct cudaGraphicsResource *cuda_vbo_resource = 0;

    switch (array) {
    default:
    case POSITION:
        hdata             = m_hPos;
        ddata             = m_dPos;
        cuda_vbo_resource = m_cuda_posvbo_resource;
        break;

    case VELOCITY:
        hdata = m_hVel;
        ddata = m_dVel;
        break;
    }

    copyArrayFromDevice(hdata, ddata, &cuda_vbo_resource, m_numParticles * 4 * sizeof(float));
    return hdata;
}

void ParticleSystem::setArray(ParticleArray array, const float *data, int start, int count)
{
    assert(m_bInitialized);

    switch (array) {
    default:
    case POSITION: {
        if (m_bUseOpenGL) {
            unregisterGLBufferObject(m_cuda_posvbo_resource);
            glBindBuffer(GL_ARRAY_BUFFER, m_posVbo);
            glBufferSubData(GL_ARRAY_BUFFER, start * 4 * sizeof(float), count * 4 * sizeof(float), data);
            glBindBuffer(GL_ARRAY_BUFFER, 0);
            registerGLBufferObject(m_posVbo, &m_cuda_posvbo_resource);
        }
        else {
            copyArrayToDevice(m_cudaPosVBO, data, start * 4 * sizeof(float), count * 4 * sizeof(float));
        }
    } break;

    case VELOCITY:
        copyArrayToDevice(m_dVel, data, start * 4 * sizeof(float), count * 4 * sizeof(float));
        break;
    }
}

inline float frand() { return rand() / (float)RAND_MAX; }

void ParticleSystem::initGrid(uint *size, float spacing, float jitter, uint numParticles)
{
    srand(1973);

    for (uint z = 0; z < size[2]; z++) {
        for (uint y = 0; y < size[1]; y++) {
            for (uint x = 0; x < size[0]; x++) {
                uint i = (z * size[1] * size[0]) + (y * size[0]) + x;

                if (i < numParticles) {
                    m_hPos[i * 4] = (spacing * x) + m_params.particleRadius - 1.0f + (frand() * 2.0f - 1.0f) * jitter;
                    m_hPos[i * 4 + 1] =
                        (spacing * y) + m_params.particleRadius - 1.0f + (frand() * 2.0f - 1.0f) * jitter;
                    m_hPos[i * 4 + 2] =
                        (spacing * z) + m_params.particleRadius - 1.0f + (frand() * 2.0f - 1.0f) * jitter;
                    m_hPos[i * 4 + 3] = 1.0f;

                    m_hVel[i * 4]     = 0.0f;
                    m_hVel[i * 4 + 1] = 0.0f;
                    m_hVel[i * 4 + 2] = 0.0f;
                    m_hVel[i * 4 + 3] = 0.0f;
                }
            }
        }
    }
}

void ParticleSystem::reset(ParticleConfig config)
{
    switch (config) {
    default:
    case CONFIG_RANDOM: {
        int p = 0, v = 0;

        for (uint i = 0; i < m_numParticles; i++) {
            float point[3];
            point[0]    = frand();
            point[1]    = frand();
            point[2]    = frand();
            m_hPos[p++] = 2 * (point[0] - 0.5f);
            m_hPos[p++] = 2 * (point[1] - 0.5f);
            m_hPos[p++] = 2 * (point[2] - 0.5f);
            m_hPos[p++] = 1.0f; // radius
            m_hVel[v++] = 0.0f;
            m_hVel[v++] = 0.0f;
            m_hVel[v++] = 0.0f;
            m_hVel[v++] = 0.0f;
        }
    } break;

    case CONFIG_GRID: {
        float jitter = m_params.particleRadius * 0.01f;
        uint  s      = (int)ceilf(powf((float)m_numParticles, 1.0f / 3.0f));
        uint  gridSize[3];
        gridSize[0] = gridSize[1] = gridSize[2] = s;
        initGrid(gridSize, m_params.particleRadius * 2.0f, jitter, m_numParticles);
    } break;
    }

    setArray(POSITION, m_hPos, 0, m_numParticles);
    setArray(VELOCITY, m_hVel, 0, m_numParticles);
}

void ParticleSystem::addSphere(int start, float *pos, float *vel, int r, float spacing)
{
    uint index = start;

    for (int z = -r; z <= r; z++) {
        for (int y = -r; y <= r; y++) {
            for (int x = -r; x <= r; x++) {
                float dx     = x * spacing;
                float dy     = y * spacing;
                float dz     = z * spacing;
                float l      = sqrtf(dx * dx + dy * dy + dz * dz);
                float jitter = m_params.particleRadius * 0.01f;

                if ((l <= m_params.particleRadius * 2.0f * r) && (index < m_numParticles)) {
                    m_hPos[index * 4]     = pos[0] + dx + (frand() * 2.0f - 1.0f) * jitter;
                    m_hPos[index * 4 + 1] = pos[1] + dy + (frand() * 2.0f - 1.0f) * jitter;
                    m_hPos[index * 4 + 2] = pos[2] + dz + (frand() * 2.0f - 1.0f) * jitter;
                    m_hPos[index * 4 + 3] = pos[3];

                    m_hVel[index * 4]     = vel[0];
                    m_hVel[index * 4 + 1] = vel[1];
                    m_hVel[index * 4 + 2] = vel[2];
                    m_hVel[index * 4 + 3] = vel[3];
                    index++;
                }
            }
        }
    }

    setArray(POSITION, m_hPos, start, index);
    setArray(VELOCITY, m_hVel, start, index);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/particles/particleSystem.cpp`.

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

- **Total Lines**: 495
- **Approximate Size**: 14615 bytes

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
