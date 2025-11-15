# Keywords: Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu
---

**Total Keywords**: 12

---

## H

### HELPERGL_EXTERN_GL_FUNC_IMPLEMENTATION {#helperglexternglfuncimplementation}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `#define HELPERGL_EXTERN_GL_FUNC_IMPLEMENTATION

// includes for OpenGL
#include <helper_gl.h>

// in`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `ON

// includes for OpenGL
#include <helper_gl`


## P

### ParticleSystem {#particlesystem}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `tring.h>

#include "ParticleSystem.cuh"
#include "part`


## S

### SimParams {#simparams}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: ` void setParameters(SimParams *hostParams)
    {
`


## C

### calcDepth {#calcdepth}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `void calcDepth(float4 *pos,
                   float  *keys,    // output
                   uint   `

### computeGridSize {#computegridsize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `void computeGridSize(int n, int blockSize, int &numBlocks, int &numThreads)
    {`

### createNoiseTexture {#createnoisetexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `void createNoiseTexture(int w, int h, int d)
    {`


## F

### frand {#frand}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `float frand() {`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `int iDivUp(int a, int b) {`

### integrateSystem {#integratesystem}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `void
    integrateSystem(float4 *oldPos, float4 *newPos, float4 *oldVel, float4 *newVel, float delta`


## S

### setParameters {#setparameters}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `void setParameters(SimParams *hostParams)
    {`

### sortParticles {#sortparticles}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/ParticleSystem_cuda.cu](./ParticleSystem_cuda.cu_docs.md)
- **Context**: `void sortParticles(float *sortKeys, uint *indices, uint numParticles)
    {`

