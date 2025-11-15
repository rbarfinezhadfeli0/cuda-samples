# Documentation for Samples/8_Platform_Specific/Tegra/fluidsGLES/fluidsGLES_kernels.cu

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/fluidsGLES/fluidsGLES_kernels.cu`
- **Type**: .cu
- **Location**: Samples/8_Platform_Specific/Tegra/fluidsGLES
- **Binary**: No

## Purpose and Role

This is a CUDA source file containing GPU kernel implementations and host code.

## Original Source Content

```cu
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

#include <cuda_runtime.h>
#include <cufft.h>       // CUDA FFT Libraries
#include <helper_cuda.h> // Helper functions for CUDA Error handling
#include <stdio.h>
#include <stdlib.h>

// OpenGL Graphics includes
#include <GLES3/gl31.h>

// FluidsGLES CUDA kernel definitions
#include "fluidsGLES_kernels.cuh"

// Texture object for reading velocity field
cudaTextureObject_t texObj;
static cudaArray   *array = NULL;

// Particle data
extern GLuint                       vbo;               // OpenGL vertex buffer object
extern struct cudaGraphicsResource *cuda_vbo_resource; // handles OpenGL-CUDA exchange

// Texture pitch
extern size_t      tPitch;
extern cufftHandle planr2c;
extern cufftHandle planc2r;
cData             *vxfield = NULL;
cData             *vyfield = NULL;

void setupTexture(int x, int y)
{
    cudaChannelFormatDesc desc = cudaCreateChannelDesc<float2>();

    cudaMallocArray(&array, &desc, y, x);
    getLastCudaError("cudaMalloc failed");

    cudaResourceDesc texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType         = cudaResourceTypeArray;
    texRes.res.array.array = array;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&texObj, &texRes, &texDescr, NULL));
}

void updateTexture(cData *data, size_t wib, size_t h, size_t pitch)
{
    checkCudaErrors(cudaMemcpy2DToArray(array, 0, 0, data, pitch, wib, h, cudaMemcpyDeviceToDevice));
}

void deleteTexture(void)
{
    checkCudaErrors(cudaDestroyTextureObject(texObj));
    checkCudaErrors(cudaFreeArray(array));
}

// Note that these kernels are designed to work with arbitrary
// domain sizes, not just domains that are multiples of the tile
// size. Therefore, we have extra code that checks to make sure
// a given thread location falls within the domain boundaries in
// both X and Y. Also, the domain is covered by looping over
// multiple elements in the Y direction, while there is a one-to-one
// mapping between threads in X and the tile size in X.
// Nolan Goodnight 9/22/06

// This method adds constant force vectors to the velocity field
// stored in 'v' according to v(x,t+1) = v(x,t) + dt * f.
__global__ void addForces_k(cData *v, int dx, int dy, int spx, int spy, float fx, float fy, int r, size_t pitch)
{
    int    tx = threadIdx.x;
    int    ty = threadIdx.y;
    cData *fj = (cData *)((char *)v + (ty + spy) * pitch) + tx + spx;

    cData vterm = *fj;
    tx -= r;
    ty -= r;
    float s = 1.f / (1.f + tx * tx * tx * tx + ty * ty * ty * ty);
    vterm.x += s * fx;
    vterm.y += s * fy;
    *fj = vterm;
}

// This method performs the velocity advection step, where we
// trace velocity vectors back in time to update each grid cell.
// That is, v(x,t+1) = v(p(x,-dt),t). Here we perform bilinear
// interpolation in the velocity space.
__global__ void advectVelocity_k(cData              *v,
                                 float              *vx,
                                 float              *vy,
                                 int                 dx,
                                 int                 pdx,
                                 int                 dy,
                                 float               dt,
                                 int                 lb,
                                 cudaTextureObject_t texObject)
{
    int gtidx = blockIdx.x * blockDim.x + threadIdx.x;
    int gtidy = blockIdx.y * (lb * blockDim.y) + threadIdx.y * lb;
    int p;

    cData vterm, ploc;
    float vxterm, vyterm;

    // gtidx is the domain location in x for this thread
    if (gtidx < dx) {
        for (p = 0; p < lb; p++) {
            // fi is the domain location in y for this thread
            int fi = gtidy + p;

            if (fi < dy) {
                int fj = fi * pdx + gtidx;
                vterm  = tex2D<cData>(texObject, (float)gtidx, (float)fi);
                ploc.x = (gtidx + 0.5f) - (dt * vterm.x * dx);
                ploc.y = (fi + 0.5f) - (dt * vterm.y * dy);
                vterm  = tex2D<cData>(texObject, ploc.x, ploc.y);
                vxterm = vterm.x;
                vyterm = vterm.y;
                vx[fj] = vxterm;
                vy[fj] = vyterm;
            }
        }
    }
}

// This method performs velocity diffusion and forces mass conservation
// in the frequency domain. The inputs 'vx' and 'vy' are complex-valued
// arrays holding the Fourier coefficients of the velocity field in
// X and Y. Diffusion in this space takes a simple form described as:
// v(k,t) = v(k,t) / (1 + visc * dt * k^2), where visc is the viscosity,
// and k is the wavenumber. The projection step forces the Fourier
// velocity vectors to be orthogonal to the vectors for each
// wavenumber: v(k,t) = v(k,t) - ((k dot v(k,t) * k) / k^2.
__global__ void diffuseProject_k(cData *vx, cData *vy, int dx, int dy, float dt, float visc, int lb)
{
    int gtidx = blockIdx.x * blockDim.x + threadIdx.x;
    int gtidy = blockIdx.y * (lb * blockDim.y) + threadIdx.y * lb;
    int p;

    cData xterm, yterm;

    // gtidx is the domain location in x for this thread
    if (gtidx < dx) {
        for (p = 0; p < lb; p++) {
            // fi is the domain location in y for this thread
            int fi = gtidy + p;

            if (fi < dy) {
                int fj = fi * dx + gtidx;
                xterm  = vx[fj];
                yterm  = vy[fj];

                // Compute the index of the wavenumber based on the
                // data order produced by a standard NN FFT.
                int iix = gtidx;
                int iiy = (fi > dy / 2) ? (fi - (dy)) : fi;

                // Velocity diffusion
                float kk   = (float)(iix * iix + iiy * iiy); // k^2
                float diff = 1.f / (1.f + visc * dt * kk);
                xterm.x *= diff;
                xterm.y *= diff;
                yterm.x *= diff;
                yterm.y *= diff;

                // Velocity projection
                if (kk > 0.f) {
                    float rkk = 1.f / kk;
                    // Real portion of velocity projection
                    float rkp = (iix * xterm.x + iiy * yterm.x);
                    // Imaginary portion of velocity projection
                    float ikp = (iix * xterm.y + iiy * yterm.y);
                    xterm.x -= rkk * rkp * iix;
                    xterm.y -= rkk * ikp * iix;
                    yterm.x -= rkk * rkp * iiy;
                    yterm.y -= rkk * ikp * iiy;
                }

                vx[fj] = xterm;
                vy[fj] = yterm;
            }
        }
    }
}

// This method updates the velocity field 'v' using the two complex
// arrays from the previous step: 'vx' and 'vy'. Here we scale the
// real components by 1/(dx*dy) to account for an unnormalized FFT.
__global__ void updateVelocity_k(cData *v, float *vx, float *vy, int dx, int pdx, int dy, int lb, size_t pitch)
{
    int gtidx = blockIdx.x * blockDim.x + threadIdx.x;
    int gtidy = blockIdx.y * (lb * blockDim.y) + threadIdx.y * lb;
    int p;

    float vxterm, vyterm;
    cData nvterm;

    // gtidx is the domain location in x for this thread
    if (gtidx < dx) {
        for (p = 0; p < lb; p++) {
            // fi is the domain location in y for this thread
            int fi = gtidy + p;

            if (fi < dy) {
                int fjr = fi * pdx + gtidx;
                vxterm  = vx[fjr];
                vyterm  = vy[fjr];

                // Normalize the result of the inverse FFT
                float scale = 1.f / (dx * dy);
                nvterm.x    = vxterm * scale;
                nvterm.y    = vyterm * scale;

                cData *fj = (cData *)((char *)v + fi * pitch) + gtidx;
                *fj       = nvterm;
            }
        } // If this thread is inside the domain in Y
    } // If this thread is inside the domain in X
}

// This method updates the particles by moving particle positions
// according to the velocity field and time step. That is, for each
// particle: p(t+1) = p(t) + dt * v(p(t)).
__global__ void advectParticles_k(cData *part, cData *v, int dx, int dy, float dt, int lb, size_t pitch)
{
    int gtidx = blockIdx.x * blockDim.x + threadIdx.x;
    int gtidy = blockIdx.y * (lb * blockDim.y) + threadIdx.y * lb;
    int p;

    // gtidx is the domain location in x for this thread
    cData pterm, vterm;

    if (gtidx < dx) {
        for (p = 0; p < lb; p++) {
            // fi is the domain location in y for this thread
            int fi = gtidy + p;

            if (fi < dy) {
                int fj = fi * dx + gtidx;
                pterm  = part[fj];

                int xvi = ((int)(pterm.x * dx));
                int yvi = ((int)(pterm.y * dy));
                vterm   = *((cData *)((char *)v + yvi * pitch) + xvi);

                pterm.x += dt * vterm.x;
                pterm.x = pterm.x - (int)pterm.x;
                pterm.x += 1.f;
                pterm.x = pterm.x - (int)pterm.x;
                pterm.y += dt * vterm.y;
                pterm.y = pterm.y - (int)pterm.y;
                pterm.y += 1.f;
                pterm.y = pterm.y - (int)pterm.y;

                part[fj] = pterm;
            }
        } // If this thread is inside the domain in Y
    } // If this thread is inside the domain in X
}

// These are the external function calls necessary for launching fluid simuation
extern "C" void addForces(cData *v, int dx, int dy, int spx, int spy, float fx, float fy, int r)
{
    dim3 tids(2 * r + 1, 2 * r + 1);

    addForces_k<<<1, tids>>>(v, dx, dy, spx, spy, fx, fy, r, tPitch);
    getLastCudaError("addForces_k failed.");
}

extern "C" void advectVelocity(cData *v, float *vx, float *vy, int dx, int pdx, int dy, float dt)
{
    dim3 grid((dx / TILEX) + (!(dx % TILEX) ? 0 : 1), (dy / TILEY) + (!(dy % TILEY) ? 0 : 1));

    dim3 tids(TIDSX, TIDSY);

    updateTexture(v, DIM * sizeof(cData), DIM, tPitch);
    advectVelocity_k<<<grid, tids>>>(v, vx, vy, dx, pdx, dy, dt, TILEY / TIDSY, texObj);
    getLastCudaError("advectVelocity_k failed.");
}

extern "C" void diffuseProject(cData *vx, cData *vy, int dx, int dy, float dt, float visc)
{
    // Forward FFT
    checkCudaErrors(cufftExecR2C(planr2c, (cufftReal *)vx, (cufftComplex *)vx));
    checkCudaErrors(cufftExecR2C(planr2c, (cufftReal *)vy, (cufftComplex *)vy));

    uint3 grid = make_uint3((dx / TILEX) + (!(dx % TILEX) ? 0 : 1), (dy / TILEY) + (!(dy % TILEY) ? 0 : 1), 1);
    uint3 tids = make_uint3(TIDSX, TIDSY, 1);

    diffuseProject_k<<<grid, tids>>>(vx, vy, dx, dy, dt, visc, TILEY / TIDSY);
    getLastCudaError("diffuseProject_k failed.");

    // Inverse FFT
    checkCudaErrors(cufftExecC2R(planc2r, (cufftComplex *)vx, (cufftReal *)vx));
    checkCudaErrors(cufftExecC2R(planc2r, (cufftComplex *)vy, (cufftReal *)vy));
}

extern "C" void updateVelocity(cData *v, float *vx, float *vy, int dx, int pdx, int dy)
{
    dim3 grid((dx / TILEX) + (!(dx % TILEX) ? 0 : 1), (dy / TILEY) + (!(dy % TILEY) ? 0 : 1));
    dim3 tids(TIDSX, TIDSY);

    updateVelocity_k<<<grid, tids>>>(v, vx, vy, dx, pdx, dy, TILEY / TIDSY, tPitch);
    getLastCudaError("updateVelocity_k failed.");
}

extern "C" void advectParticles(GLuint vbo, cData *v, int dx, int dy, float dt)
{
    dim3 grid((dx / TILEX) + (!(dx % TILEX) ? 0 : 1), (dy / TILEY) + (!(dy % TILEY) ? 0 : 1));
    dim3 tids(TIDSX, TIDSY);

    cData *p;
    checkCudaErrors(cudaGraphicsMapResources(1, &cuda_vbo_resource, 0));
    getLastCudaError("cudaGraphicsMapResources failed");

    size_t num_bytes;
    checkCudaErrors(cudaGraphicsResourceGetMappedPointer((void **)&p, &num_bytes, cuda_vbo_resource));
    getLastCudaError("cudaGraphicsResourceGetMappedPointer failed");

    advectParticles_k<<<grid, tids>>>(p, v, dx, dy, dt, TILEY / TIDSY, tPitch);
    getLastCudaError("advectParticles_k failed.");

    checkCudaErrors(cudaGraphicsUnmapResources(1, &cuda_vbo_resource, 0));
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/fluidsGLES/fluidsGLES_kernels.cu`.

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

- **Total Lines**: 357
- **Approximate Size**: 13701 bytes

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
