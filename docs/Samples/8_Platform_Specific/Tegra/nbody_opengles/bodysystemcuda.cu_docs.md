# Documentation for Samples/8_Platform_Specific/Tegra/nbody_opengles/bodysystemcuda.cu

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/nbody_opengles/bodysystemcuda.cu`
- **Type**: .cu
- **Location**: Samples/8_Platform_Specific/Tegra/nbody_opengles
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

#include <helper_cuda.h>
#include <math.h>

// #include <GL/glew.h>
// #include <GL/freeglut.h>

// CUDA standard includes
#include <cuda_runtime.h>
// #include <cuda_gl_interop.h>

#include "bodysystem.h"

__constant__ float  softeningSquared;
__constant__ double softeningSquared_fp64;

cudaError_t setSofteningSquared(float softeningSq)
{
    return cudaMemcpyToSymbol(softeningSquared, &softeningSq, sizeof(float), 0, cudaMemcpyHostToDevice);
}

cudaError_t setSofteningSquared(double softeningSq)
{
    return cudaMemcpyToSymbol(softeningSquared_fp64, &softeningSq, sizeof(double), 0, cudaMemcpyHostToDevice);
}

template <class T> struct SharedMemory
{
    __device__ inline operator T *()
    {
        extern __shared__ int __smem[];
        return (T *)__smem;
    }

    __device__ inline operator const T *() const
    {
        extern __shared__ int __smem[];
        return (T *)__smem;
    }
};

template <typename T> __device__ T rsqrt_T(T x) { return rsqrt(x); }

template <> __device__ float rsqrt_T<float>(float x) { return rsqrtf(x); }

template <> __device__ double rsqrt_T<double>(double x) { return rsqrt(x); }

// Macros to simplify shared memory addressing
#define SX(i) sharedPos[i + blockDim.x * threadIdx.y]
// This macro is only used when multithreadBodies is true (below)
#define SX_SUM(i, j) sharedPos[i + blockDim.x * j]

template <typename T> __device__ T getSofteningSquared() { return softeningSquared; }
template <> __device__ double      getSofteningSquared<double>() { return softeningSquared_fp64; }

template <typename T> struct DeviceData
{
    T           *dPos[2]; // mapped host pointers
    T           *dVel;
    cudaEvent_t  event;
    unsigned int offset;
    unsigned int numBodies;
};

template <typename T>
__device__ typename vec3<T>::Type
bodyBodyInteraction(typename vec3<T>::Type ai, typename vec4<T>::Type bi, typename vec4<T>::Type bj)
{
    typename vec3<T>::Type r;

    // r_ij  [3 FLOPS]
    r.x = bj.x - bi.x;
    r.y = bj.y - bi.y;
    r.z = bj.z - bi.z;

    // distSqr = dot(r_ij, r_ij) + EPS^2  [6 FLOPS]
    T distSqr = r.x * r.x + r.y * r.y + r.z * r.z;
    distSqr += getSofteningSquared<T>();

    // invDistCube =1/distSqr^(3/2)  [4 FLOPS (2 mul, 1 sqrt, 1 inv)]
    T invDist     = rsqrt_T(distSqr);
    T invDistCube = invDist * invDist * invDist;

    // s = m_j * invDistCube [1 FLOP]
    T s = bj.w * invDistCube;

    // a_i =  a_i + s * r_ij [6 FLOPS]
    ai.x += r.x * s;
    ai.y += r.y * s;
    ai.z += r.z * s;

    return ai;
}

template <typename T>
__device__ typename vec3<T>::Type
computeBodyAccel(typename vec4<T>::Type bodyPos, typename vec4<T>::Type *positions, int numTiles)
{
    typename vec4<T>::Type *sharedPos = SharedMemory<typename vec4<T>::Type>();

    typename vec3<T>::Type acc = {0.0f, 0.0f, 0.0f};

    for (int tile = 0; tile < numTiles; tile++) {
        sharedPos[threadIdx.x] = positions[tile * blockDim.x + threadIdx.x];

        __syncthreads();

        // This is the "tile_calculation" from the GPUG3 article.
#pragma unroll 128

        for (unsigned int counter = 0; counter < blockDim.x; counter++) {
            acc = bodyBodyInteraction<T>(acc, bodyPos, sharedPos[counter]);
        }

        __syncthreads();
    }

    return acc;
}

template <typename T>
__global__ void integrateBodies(typename vec4<T>::Type *__restrict__ newPos,
                                typename vec4<T>::Type *__restrict__ oldPos,
                                typename vec4<T>::Type *vel,
                                unsigned int            deviceOffset,
                                unsigned int            deviceNumBodies,
                                float                   deltaTime,
                                float                   damping,
                                int                     numTiles)
{
    int index = blockIdx.x * blockDim.x + threadIdx.x;

    if (index >= deviceNumBodies) {
        return;
    }

    typename vec4<T>::Type position = oldPos[deviceOffset + index];

    typename vec3<T>::Type accel = computeBodyAccel<T>(position, oldPos, numTiles);

    // acceleration = force / mass;
    // new velocity = old velocity + acceleration * deltaTime
    // note we factor out the body's mass from the equation, here and in
    // bodyBodyInteraction (because they cancel out).  Thus here force ==
    // acceleration
    typename vec4<T>::Type velocity = vel[deviceOffset + index];

    velocity.x += accel.x * deltaTime;
    velocity.y += accel.y * deltaTime;
    velocity.z += accel.z * deltaTime;

    velocity.x *= damping;
    velocity.y *= damping;
    velocity.z *= damping;

    // new position = old position + velocity * deltaTime
    position.x += velocity.x * deltaTime;
    position.y += velocity.y * deltaTime;
    position.z += velocity.z * deltaTime;

    // store new position and velocity
    newPos[deviceOffset + index] = position;
    vel[deviceOffset + index]    = velocity;
}

template <typename T>
void integrateNbodySystem(DeviceData<T>         *deviceData,
                          cudaGraphicsResource **pgres,
                          unsigned int           currentRead,
                          float                  deltaTime,
                          float                  damping,
                          unsigned int           numBodies,
                          unsigned int           numDevices,
                          int                    blockSize,
                          bool                   bUsePBO)
{
    if (bUsePBO) {
        checkCudaErrors(cudaGraphicsResourceSetMapFlags(pgres[currentRead], cudaGraphicsMapFlagsReadOnly));
        checkCudaErrors(cudaGraphicsResourceSetMapFlags(pgres[1 - currentRead], cudaGraphicsMapFlagsWriteDiscard));
        checkCudaErrors(cudaGraphicsMapResources(2, pgres, 0));
        size_t bytes;
        checkCudaErrors(cudaGraphicsResourceGetMappedPointer(
            (void **)&(deviceData[0].dPos[currentRead]), &bytes, pgres[currentRead]));
        checkCudaErrors(cudaGraphicsResourceGetMappedPointer(
            (void **)&(deviceData[0].dPos[1 - currentRead]), &bytes, pgres[1 - currentRead]));
    }

    for (unsigned int dev = 0; dev != numDevices; dev++) {
        if (numDevices > 1) {
            cudaSetDevice(dev);
        }

        int numBlocks     = (deviceData[dev].numBodies + blockSize - 1) / blockSize;
        int numTiles      = (numBodies + blockSize - 1) / blockSize;
        int sharedMemSize = blockSize * 4 * sizeof(T); // 4 floats for pos

        integrateBodies<T>
            <<<numBlocks, blockSize, sharedMemSize>>>((typename vec4<T>::Type *)deviceData[dev].dPos[1 - currentRead],
                                                      (typename vec4<T>::Type *)deviceData[dev].dPos[currentRead],
                                                      (typename vec4<T>::Type *)deviceData[dev].dVel,
                                                      deviceData[dev].offset,
                                                      deviceData[dev].numBodies,
                                                      deltaTime,
                                                      damping,
                                                      numTiles);

        if (numDevices > 1) {
            checkCudaErrors(cudaEventRecord(deviceData[dev].event));
            // MJH: Hack on older driver versions to force kernel launches to flush!
            cudaStreamQuery(0);
        }

        // check if kernel invocation generated an error
        getLastCudaError("Kernel execution failed");
    }

    if (numDevices > 1) {
        for (unsigned int dev = 0; dev < numDevices; dev++) {
            checkCudaErrors(cudaEventSynchronize(deviceData[dev].event));
        }
    }

    if (bUsePBO) {
        checkCudaErrors(cudaGraphicsUnmapResources(2, pgres, 0));
    }
}

// Explicit specializations needed to generate code
template void integrateNbodySystem<float>(DeviceData<float>     *deviceData,
                                          cudaGraphicsResource **pgres,
                                          unsigned int           currentRead,
                                          float                  deltaTime,
                                          float                  damping,
                                          unsigned int           numBodies,
                                          unsigned int           numDevices,
                                          int                    blockSize,
                                          bool                   bUsePBO);

template void integrateNbodySystem<double>(DeviceData<double>    *deviceData,
                                           cudaGraphicsResource **pgres,
                                           unsigned int           currentRead,
                                           float                  deltaTime,
                                           float                  damping,
                                           unsigned int           numBodies,
                                           unsigned int           numDevices,
                                           int                    blockSize,
                                           bool                   bUsePBO);

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/nbody_opengles/bodysystemcuda.cu`.

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

- **Total Lines**: 274
- **Approximate Size**: 10835 bytes

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
