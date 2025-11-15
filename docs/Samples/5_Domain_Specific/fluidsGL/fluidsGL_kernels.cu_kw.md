# Keywords: Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu
---

**Total Keywords**: 17

---

## F

### FluidsGL {#fluidsgl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `e <helper_gl.h>

// FluidsGL CUDA kernel definit`


## H

### HELPERGL_EXTERN_GL_FUNC_IMPLEMENTATION {#helperglexternglfuncimplementation}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `#define HELPERGL_EXTERN_GL_FUNC_IMPLEMENTATION
#include <helper_gl.h>

// FluidsGL CUDA kernel defin`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `lude <stdlib.h>

// OpenGL Graphics includes
#`


## A

### addForces {#addforces}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void addForces(cData *v, int dx, int dy, int spx, int spy, float fx, float fy, int r)
{`

### addForces_k {#addforcesk}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `__global__ void addForces_k(`

### advectParticles {#advectparticles}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void advectParticles(GLuint vbo, cData *v, int dx, int dy, float dt)
{`

### advectParticles_k {#advectparticlesk}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `__global__ void advectParticles_k(`

### advectVelocity {#advectvelocity}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void advectVelocity(cData *v, float *vx, float *vy, int dx, int pdx, int dy, float dt)
{`

### advectVelocity_k {#advectvelocityk}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `__global__ void advectVelocity_k(`


## C

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### deleteTexture {#deletetexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void deleteTexture(void)
{`

### diffuseProject {#diffuseproject}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void diffuseProject(cData *vx, cData *vy, int dx, int dy, float dt, float visc)
{`

### diffuseProject_k {#diffuseprojectk}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `__global__ void diffuseProject_k(`


## S

### setupTexture {#setuptexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void setupTexture(int x, int y)
{`


## U

### updateTexture {#updatetexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void updateTexture(cData *data, size_t wib, size_t h, size_t pitch)
{`

### updateVelocity {#updatevelocity}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `void updateVelocity(cData *v, float *vx, float *vy, int dx, int pdx, int dy)
{`

### updateVelocity_k {#updatevelocityk}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/fluidsGL/fluidsGL_kernels.cu](./fluidsGL_kernels.cu_docs.md)
- **Context**: `__global__ void updateVelocity_k(`

