# Documentation for Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu`
- **Type**: .cu
- **Location**: Samples/4_CUDA_Libraries/lineOfSight
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

// This sample is an implementation of a simple line-of-sight algorithm:
// Given a height map and a ray originating at some observation point,
// it computes all the points along the ray that are visible from the
// observation point.
// It is based on the description made in "Guy E. Blelloch.  Vector models
// for data-parallel computing. MIT Press, 1990" and uses open source CUDA
// Thrust Library

#ifdef _WIN32
#define NOMINMAX
#endif

// includes, system
#include <float.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project
#include <helper_cuda.h>
#include <helper_functions.h>
#include <helper_math.h>

// includes, library
#include <thrust/copy.h>
#include <thrust/device_vector.h>
#include <thrust/host_vector.h>
#include <thrust/scan.h>

////////////////////////////////////////////////////////////////////////////////
// declaration, types

// Boolean
typedef unsigned char Bool;
enum { False = 0, True = 1 };

// 2D height field
struct HeightField
{
    int    width;
    float *height;
};

// Ray
struct Ray
{
    float3 origin;
    float2 dir;
    int    length;
    float  oneOverLength;
};

////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////
// declaration, forward
int                        runTest(int argc, char **argv);
__global__ void            computeAngles_kernel(const Ray, float *, cudaTextureObject_t);
__global__ void            computeVisibilities_kernel(const float *, const float *, int, Bool *);
void                       lineOfSight_gold(const HeightField, const Ray, Bool *);
__device__ __host__ float2 getLocation(const Ray, int);
__device__ __host__ float  getAngle(const Ray, float2, float);

////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    int res = runTest(argc, argv);

    if (res != 1) {
        printf("Test failed!\n");
        exit(EXIT_FAILURE);
    }

    printf("Test passed\n");
    exit(EXIT_SUCCESS);
}

////////////////////////////////////////////////////////////////////////////////
//! Run a line-of-sight test for CUDA
////////////////////////////////////////////////////////////////////////////////
int runTest(int argc, char **argv)
{
    ////////////////////////////////////////////////////////////////////////////
    // Device initialization

    printf("[%s] - Starting...\n", argv[0]);

    // use command-line specified CUDA device, otherwise use device with highest
    // Gflops/s
    findCudaDevice(argc, (const char **)argv);

    ////////////////////////////////////////////////////////////////////////////
    // Timer

    // Create
    StopWatchInterface *timer;
    sdkCreateTimer(&timer);

    // Number of iterations to get accurate timing
    uint numIterations = 100;

    ////////////////////////////////////////////////////////////////////////////
    // Height field

    HeightField heightField;

    // Allocate in host memory
    int2 dim          = make_int2(10000, 100);
    heightField.width = dim.x;
    thrust::host_vector<float> height(dim.x * dim.y);
    heightField.height = (float *)&height[0];

    //
    // Fill in with an arbitrary sine surface
    for (int x = 0; x < dim.x; ++x)
        for (int y = 0; y < dim.y; ++y) {
            float amp    = 0.1f * (x + y);
            float period = 2.0f + amp;
            *(heightField.height + dim.x * y + x) =
                amp * (sinf(sqrtf((float)(x * x + y * y)) * 2.0f * 3.1416f / period) + 1.0f);
        }

    // Allocate CUDA array in device memory
    cudaChannelFormatDesc channelDesc = cudaCreateChannelDesc(32, 0, 0, 0, cudaChannelFormatKindFloat);
    cudaArray            *heightFieldArray;
    checkCudaErrors(cudaMallocArray(&heightFieldArray, &channelDesc, dim.x, dim.y));

    // Initialize device memory
    checkCudaErrors(cudaMemcpy2DToArray(heightFieldArray,
                                        0,
                                        0,
                                        heightField.height,
                                        dim.x * sizeof(float),
                                        dim.x * sizeof(float),
                                        dim.y,
                                        cudaMemcpyHostToDevice));

    cudaTextureObject_t heightFieldTex;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType         = cudaResourceTypeArray;
    texRes.res.array.array = heightFieldArray;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));
    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModePoint;
    texDescr.addressMode[0]   = cudaAddressModeClamp;
    texDescr.addressMode[1]   = cudaAddressModeClamp;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&heightFieldTex, &texRes, &texDescr, NULL));

    //////////////////////////////////////////////////////////////////////////////
    // Ray (starts at origin and traverses the height field diagonally)

    Ray ray;
    ray.origin        = make_float3(0, 0, 2.0f);
    int2 dir          = make_int2(dim.x - 1, dim.y - 1);
    ray.dir           = make_float2((float)dir.x, (float)dir.y);
    ray.length        = max(abs(dir.x), abs(dir.y));
    ray.oneOverLength = 1.0f / ray.length;

    //////////////////////////////////////////////////////////////////////////////
    // View angles

    // Allocate view angles for each point along the ray
    thrust::device_vector<float> d_angles(ray.length);

    // Allocate result of max-scan operation on the array of view angles
    thrust::device_vector<float> d_scannedAngles(ray.length);

    //////////////////////////////////////////////////////////////////////////////
    // Visibility results

    // Allocate visibility results for each point along the ray
    thrust::device_vector<Bool> d_visibilities(ray.length);
    thrust::host_vector<Bool>   h_visibilities(ray.length);
    thrust::host_vector<Bool>   h_visibilitiesRef(ray.length);

    //////////////////////////////////////////////////////////////////////////////
    // Reference solution
    lineOfSight_gold(heightField, ray, (Bool *)&h_visibilitiesRef[0]);

    //////////////////////////////////////////////////////////////////////////////
    // Device solution

    // Execution configuration
    dim3 block(256);
    dim3 grid((uint)ceil(ray.length / (double)block.x));

    // Compute device solution
    printf("Line of sight\n");
    sdkStartTimer(&timer);

    for (uint i = 0; i < numIterations; ++i) {
        // Compute view angle for each point along the ray
        computeAngles_kernel<<<grid, block>>>(ray, thrust::raw_pointer_cast(&d_angles[0]), heightFieldTex);
        getLastCudaError("Kernel execution failed");

        // Perform a max-scan operation on the array of view angles
        thrust::inclusive_scan(d_angles.begin(), d_angles.end(), d_scannedAngles.begin(), thrust::maximum<float>());
        getLastCudaError("Kernel execution failed");

        // Compute visibility results based on the array of view angles
        // and its scanned version
        computeVisibilities_kernel<<<grid, block>>>(thrust::raw_pointer_cast(&d_angles[0]),
                                                    thrust::raw_pointer_cast(&d_scannedAngles[0]),
                                                    ray.length,
                                                    thrust::raw_pointer_cast(&d_visibilities[0]));
        getLastCudaError("Kernel execution failed");
    }

    cudaDeviceSynchronize();
    sdkStopTimer(&timer);
    getLastCudaError("Kernel execution failed");

    // Copy visibility results back to the host
    thrust::copy(d_visibilities.begin(), d_visibilities.end(), h_visibilities.begin());

    // Compare device visibility results against reference results
    bool res = compareData(thrust::raw_pointer_cast(&h_visibilitiesRef[0]),
                           thrust::raw_pointer_cast(&h_visibilities[0]),
                           ray.length,
                           0.0f,
                           0.0f);
    printf("Average time: %f ms\n\n", sdkGetTimerValue(&timer) / numIterations);
    sdkResetTimer(&timer);

    // Cleanup memory
    checkCudaErrors(cudaFreeArray(heightFieldArray));
    return res;
}

////////////////////////////////////////////////////////////////////////////////
//! Compute view angles for each point along the ray
//! @param ray         ray
//! @param angles      view angles
////////////////////////////////////////////////////////////////////////////////
__global__ void computeAngles_kernel(const Ray ray, float *angles, cudaTextureObject_t HeightFieldTex)
{
    uint i = blockDim.x * blockIdx.x + threadIdx.x;

    if (i < ray.length) {
        float2 location = getLocation(ray, i + 1);
        float  height   = tex2D<float>(HeightFieldTex, location.x, location.y);
        float  angle    = getAngle(ray, location, height);
        angles[i]       = angle;
    }
}

////////////////////////////////////////////////////////////////////////////////
//! Compute visibility for each point along the ray
//! @param angles          view angles
//! @param scannedAngles   max-scanned view angles
//! @param numAngles       number of view angles
//! @param visibilities    boolean array indicating the visibility of each point
//!                        along the ray
////////////////////////////////////////////////////////////////////////////////
__global__ void
computeVisibilities_kernel(const float *angles, const float *scannedAngles, int numAngles, Bool *visibilities)
{
    uint i = blockDim.x * blockIdx.x + threadIdx.x;

    if (i < numAngles) {
        visibilities[i] = scannedAngles[i] <= angles[i];
    }
}

////////////////////////////////////////////////////////////////////////////////
//! Compute reference data set
//! @param heightField     height field
//! @param ray             ray
//! @param visibilities    boolean array indicating the visibility of each point
//!                        along the ray
////////////////////////////////////////////////////////////////////////////////
void lineOfSight_gold(const HeightField heightField, const Ray ray, Bool *visibilities)
{
    float angleMax = asinf(-1.0f);

    for (int i = 0; i < ray.length; ++i) {
        float2 location = getLocation(ray, i + 1);
        float  height   = *(heightField.height + heightField.width * (int)floorf(location.y) + (int)floorf(location.x));
        float  angle    = getAngle(ray, location, height);

        if (angle > angleMax) {
            angleMax        = angle;
            visibilities[i] = True;
        }
        else {
            visibilities[i] = False;
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
//! Compute the 2D coordinates of the point located at i steps from the origin
//! of the ray
//! @param ray      ray
//! @param i        integer offset along the ray
////////////////////////////////////////////////////////////////////////////////
__device__ __host__ float2 getLocation(const Ray ray, int i)
{
    float step = i * ray.oneOverLength;
    return make_float2(ray.origin.x, ray.origin.y) + ray.dir * step;
}

////////////////////////////////////////////////////////////////////////////////
//! Compute the angle of view between a 3D point and the origin of the ray
//! @param ray        ray
//! @param location   2D coordinates of the input point
//! @param height     height of the input point
////////////////////////////////////////////////////////////////////////////////
__device__ __host__ float getAngle(const Ray ray, float2 location, float height)
{
    float2 dir = location - make_float2(ray.origin.x, ray.origin.y);
    return atanf((height - ray.origin.z) / length(dir));
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu`.

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

- **Total Lines**: 349
- **Approximate Size**: 13587 bytes

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
