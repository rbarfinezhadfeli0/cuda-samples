# Documentation: Samples/8_Platform_Specific/Tegra/fluidsGLES/fluidsGLES_kernels.h
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/fluidsGLES/fluidsGLES_kernels.h`
- **Filename**: `fluidsGLES_kernels.h`
- **Language**: h
- **Size**: 2543 bytes
- **Lines**: 53
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```h
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
#ifndef __STABLEFLUIDS_KERNELS_CUH_
#define __STABLEFLUIDS_KERNELS_CUH_

// Vector data type used to velocity and force fields
typedef float2 cData;

void setupTexture(int x, int y);
void updateTexture(cData *data, size_t w, size_t h, size_t pitch);
void deleteTexture(void);

// This method adds constant force vectors to the velocity field
// stored in 'v' according to v(x,t+1) = v(x,t) + dt * f.
__global__ void addForces_k(cData *v, int dx, int dy, int spx, int spy, float fx, float fy, int r, size_t pitch);

// This method performs the velocity advection step, where we
// trace velocity vectors back in time to update each grid cell.
// That is, v(x,t+1) = v(p(x,-dt),t). Here we perform bilinear
// interpolation in the velocity space.
__global__ void
advectVelocity_k(cData *v, float *vx, float *vy, int dx, int pdx, int dy, float dt, int lb, cudaTextureObject_t tex);

// This method performs velocity diffusion and forces mass conservation
// in the frequency domain. The inputs 'vx' and 'vy' are complex-valued
// arrays holding the Fourier coefficients of the velocity field in
// X and Y. Diffusion in this space takes a simple form described as:
// v(k,t) = v(k,t) / (1 + visc * dt * k^2), where visc is the viscosity,
// and k is the wavenumber. The projection step forces the Fourier
// velocity vectors to be orthogonal to the wave wave vectors for each
// wavenumber: v(k,t) = v(k,t) - ((k dot v(k,t) * k) / k^2.
__global__ void diffuseProject_k(cData *vx, cData *vy, int dx, int dy, float dt, float visc, int lb);

// This method updates the velocity field 'v' using the two complex
// arrays from the previous step: 'vx' and 'vy'. Here we scale the
// real components by 1/(dx*dy) to account for an unnormalized FFT.
__global__ void updateVelocity_k(cData *v, float *vx, float *vy, int dx, int pdx, int dy, int lb, size_t pitch);

// This method updates the particles by moving particle positions
// according to the velocity field and time step. That is, for each
// particle: p(t+1) = p(t) + dt * v(p(t)).
__global__ void advectParticles_k(cData *part, cData *v, int dx, int dy, float dt, int lb, size_t pitch);

#endif

```

---
## High-Level Overview
This file is a h source file containing 5 CUDA kernel(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **__STABLEFLUIDS_KERNELS_CUH_**: `// Vector data type used to velocity and force fields`

### CUDA Kernels
#### `addForces_k`
- **Return Type**: `void`
- **Parameters**: `cData *v, int dx, int dy, int spx, int spy, float fx, float fy, int r, size_t pitch`
- **Description**: CUDA kernel function for GPU execution

#### `advectVelocity_k`
- **Return Type**: `void`
- **Parameters**: `cData *v, float *vx, float *vy, int dx, int pdx, int dy, float dt, int lb, cudaTextureObject_t tex`
- **Description**: CUDA kernel function for GPU execution

#### `diffuseProject_k`
- **Return Type**: `void`
- **Parameters**: `cData *vx, cData *vy, int dx, int dy, float dt, float visc, int lb`
- **Description**: CUDA kernel function for GPU execution

#### `updateVelocity_k`
- **Return Type**: `void`
- **Parameters**: `cData *v, float *vx, float *vy, int dx, int pdx, int dy, int lb, size_t pitch`
- **Description**: CUDA kernel function for GPU execution

#### `advectParticles_k`
- **Return Type**: `void`
- **Parameters**: `cData *part, cData *v, int dx, int dy, float dt, int lb, size_t pitch`
- **Description**: CUDA kernel function for GPU execution


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

