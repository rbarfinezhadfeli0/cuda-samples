# Documentation for Samples/5_Domain_Specific/simpleVulkanMMAP/MonteCarloPi.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/simpleVulkanMMAP/MonteCarloPi.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/simpleVulkanMMAP
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

/*
 * See: https://www.piday.org/million/
 */

#include <algorithm>

#include "MonteCarloPi.h"
#define CUDA_DRIVER_API
#include <helper_cuda.h>
#include <iostream>

#define ROUND_UP_TO_GRANULARITY(x, n) (((x + n - 1) / n) * n)

// `ipcHandleTypeFlag` specifies the platform specific handle type this sample
// uses for importing and exporting memory allocation. On Linux this sample
// specifies the type as CU_MEM_HANDLE_TYPE_POSIX_FILE_DESCRIPTOR meaning that
// file descriptors will be used. On Windows this sample specifies the type as
// CU_MEM_HANDLE_TYPE_WIN32 meaning that NT HANDLEs will be used. The
// ipcHandleTypeFlag variable is a convenience variable and is passed by value
// to individual requests.
#if defined(__linux__)
CUmemAllocationHandleType ipcHandleTypeFlag = CU_MEM_HANDLE_TYPE_POSIX_FILE_DESCRIPTOR;
#else
CUmemAllocationHandleType ipcHandleTypeFlag = CU_MEM_HANDLE_TYPE_WIN32;
#endif

// Windows-specific LPSECURITYATTRIBUTES
void getDefaultSecurityDescriptor(CUmemAllocationProp *prop)
{
#if defined(__linux__)
    return;
#elif defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
    static const char        sddl[] = "D:P(OA;;GARCSDWDWOCCDCLCSWLODTWPRPCRFA;;;WD)";
    static OBJECT_ATTRIBUTES objAttributes;
    static bool              objAttributesConfigured = false;

    if (!objAttributesConfigured) {
        PSECURITY_DESCRIPTOR secDesc;
        BOOL result = ConvertStringSecurityDescriptorToSecurityDescriptorA(sddl, SDDL_REVISION_1, &secDesc, NULL);
        if (result == 0) {
            printf("IPC failure: getDefaultSecurityDescriptor Failed! (%d)\n", GetLastError());
        }

        InitializeObjectAttributes(&objAttributes, NULL, 0, NULL, secDesc);

        objAttributesConfigured = true;
    }

    prop->win32HandleMetaData = &objAttributes;
    return;
#endif
}

__global__ void monte_carlo_kernel(vec2        *xyVector,
                                   float       *pointsInsideCircle,
                                   float       *numPointsInCircle,
                                   unsigned int numPoints,
                                   float        time)
{
    const size_t stride = gridDim.x * blockDim.x;
    size_t       tid    = blockIdx.x * blockDim.x + threadIdx.x;
    float        count  = 0.0f;

    curandState rgnState;
    curand_init((unsigned long long)time, tid, 0, &rgnState);

    for (; tid < numPoints; tid += stride) {
        float x          = curand_uniform(&rgnState);
        float y          = curand_uniform(&rgnState);
        x                = (2.0f * x) - 1.0f;
        y                = (2.0f * y) - 1.0f;
        xyVector[tid][0] = x;
        xyVector[tid][1] = y;

        // Compute the distance of this point form the center(0, 0)
        float dist = sqrtf((x * x) + (y * y));

        // If distance is less than the radius of the unit circle, the point lies in
        // the circle.
        pointsInsideCircle[tid] = (dist <= 1.0f);
        count += (dist <= 1.0f);
    }
    atomicAdd(numPointsInCircle, count);
}

MonteCarloPiSimulation::MonteCarloPiSimulation(size_t num_points)
    : m_xyVector(nullptr)
    , m_pointsInsideCircle(nullptr)
    , m_totalPointsInsideCircle(0)
    , m_totalPointsSimulated(0)
    , m_numPoints(num_points)
{
}

MonteCarloPiSimulation::~MonteCarloPiSimulation()
{
    if (m_numPointsInCircle) {
        checkCudaErrors(cudaFree(m_numPointsInCircle));
        m_numPointsInCircle = nullptr;
    }
    if (m_hostNumPointsInCircle) {
        checkCudaErrors(cudaFreeHost(m_hostNumPointsInCircle));
        m_hostNumPointsInCircle = nullptr;
    }

    cleanupSimulationAllocations();
}

void MonteCarloPiSimulation::initSimulation(int cudaDevice, cudaStream_t stream)
{
    m_cudaDevice = cudaDevice;
    getIdealExecutionConfiguration();

    // Allocate a position buffer that contains random location of the points in
    // XY cartesian plane.
    // Allocate a bitmap buffer which holds information of whether a point in the
    // position buffer is inside the unit circle or not.
    setupSimulationAllocations();

    checkCudaErrors(cudaMalloc((float **)&m_numPointsInCircle, sizeof(*m_numPointsInCircle)));
    checkCudaErrors(cudaMallocHost((float **)&m_hostNumPointsInCircle, sizeof(*m_hostNumPointsInCircle)));
}

void MonteCarloPiSimulation::stepSimulation(float time, cudaStream_t stream)
{
    checkCudaErrors(cudaMemsetAsync(m_numPointsInCircle, 0, sizeof(*m_numPointsInCircle), stream));

    monte_carlo_kernel<<<m_blocks, m_threads, 0, stream>>>(
        m_xyVector, m_pointsInsideCircle, m_numPointsInCircle, m_numPoints, time);
    getLastCudaError("Failed to launch CUDA simulation");

    checkCudaErrors(cudaMemcpyAsync(
        m_hostNumPointsInCircle, m_numPointsInCircle, sizeof(*m_numPointsInCircle), cudaMemcpyDeviceToHost, stream));

    // Queue up a stream callback to compute and print the PI value.
    checkCudaErrors(cudaLaunchHostFunc(stream, this->computePiCallback, (void *)this));
}

void MonteCarloPiSimulation::computePiCallback(void *args)
{
    MonteCarloPiSimulation *cbData = (MonteCarloPiSimulation *)args;
    cbData->m_totalPointsInsideCircle += *(cbData->m_hostNumPointsInCircle);
    cbData->m_totalPointsSimulated += cbData->m_numPoints;
    double piValue = 4.0 * ((double)cbData->m_totalPointsInsideCircle / (double)cbData->m_totalPointsSimulated);
    printf("Approximate Pi value for %zd data points: %lf \n", cbData->m_totalPointsSimulated, piValue);
}

void MonteCarloPiSimulation::getIdealExecutionConfiguration()
{
    int warpSize            = 0;
    int multiProcessorCount = 0;

    checkCudaErrors(cudaSetDevice(m_cudaDevice));
    checkCudaErrors(cudaDeviceGetAttribute(&warpSize, cudaDevAttrWarpSize, m_cudaDevice));

    // We don't need large block sizes, since there's not much inter-thread
    // communication
    m_threads = warpSize;

    // Use the occupancy calculator and fill the gpu as best as we can
    checkCudaErrors(cudaOccupancyMaxActiveBlocksPerMultiprocessor(&m_blocks, monte_carlo_kernel, warpSize, 0));

    checkCudaErrors(cudaDeviceGetAttribute(&multiProcessorCount, cudaDevAttrMultiProcessorCount, m_cudaDevice));
    m_blocks *= multiProcessorCount;

    // Go ahead and the clamp the blocks to the minimum needed for this
    // height/width
    m_blocks = std::min(m_blocks, (int)((m_numPoints + m_threads - 1) / m_threads));
}

void MonteCarloPiSimulation::setupSimulationAllocations()
{
    CUdeviceptr                  d_ptr       = 0U;
    size_t                       granularity = 0;
    CUmemGenericAllocationHandle cudaPositionHandle, cudaInCircleHandle;

    CUmemAllocationProp allocProp  = {};
    allocProp.type                 = CU_MEM_ALLOCATION_TYPE_PINNED;
    allocProp.location.type        = CU_MEM_LOCATION_TYPE_DEVICE;
    allocProp.location.id          = m_cudaDevice;
    allocProp.win32HandleMetaData  = NULL;
    allocProp.requestedHandleTypes = ipcHandleTypeFlag;

    // Windows-specific LPSECURITYATTRIBUTES is required when
    // CU_MEM_HANDLE_TYPE_WIN32 is used. The security attribute defines the scope
    // of which exported allocations may be tranferred to other processes. For all
    // other handle types, pass NULL.
    getDefaultSecurityDescriptor(&allocProp);

    // Get the recommended granularity for m_cudaDevice.
    checkCudaErrors(cuMemGetAllocationGranularity(&granularity, &allocProp, CU_MEM_ALLOC_GRANULARITY_RECOMMENDED));

    size_t xyPositionVecSize = m_numPoints * sizeof(*m_xyVector);
    size_t inCircleVecSize   = m_numPoints * sizeof(*m_pointsInsideCircle);

    size_t xyPositionSize = ROUND_UP_TO_GRANULARITY(xyPositionVecSize, granularity);
    size_t inCircleSize   = ROUND_UP_TO_GRANULARITY(inCircleVecSize, granularity);
    m_totalAllocationSize = (xyPositionSize + inCircleSize);

    // Reserve the required contiguous VA space for the allocations
    checkCudaErrors(cuMemAddressReserve(&d_ptr, m_totalAllocationSize, granularity, 0U, 0));

    // Create the allocations as a pinned allocation on this device.
    // Create an allocation to store all the positions of points on the xy plane
    // and a second allocation which stores information if the corresponding
    // position is inside the unit circle or not.
    checkCudaErrors(cuMemCreate(&cudaPositionHandle, xyPositionSize, &allocProp, 0));
    checkCudaErrors(cuMemCreate(&cudaInCircleHandle, inCircleSize, &allocProp, 0));

    // Export the allocation to a platform-specific handle. The type of handle
    // requested here must match the requestedHandleTypes field in the prop
    // structure passed to cuMemCreate. The handle obtained here will be passed to
    // vulkan to import the allocation.
    checkCudaErrors(
        cuMemExportToShareableHandle((void *)&m_posShareableHandle, cudaPositionHandle, ipcHandleTypeFlag, 0));
    checkCudaErrors(
        cuMemExportToShareableHandle((void *)&m_inCircleShareableHandle, cudaInCircleHandle, ipcHandleTypeFlag, 0));

    CUdeviceptr va_position = d_ptr;
    CUdeviceptr va_InCircle = va_position + xyPositionSize;
    m_pointsInsideCircle    = (float *)va_InCircle;
    m_xyVector              = (vec2 *)va_position;

    // Assign the chunk to the appropriate VA range
    checkCudaErrors(cuMemMap(va_position, xyPositionSize, 0, cudaPositionHandle, 0));
    checkCudaErrors(cuMemMap(va_InCircle, inCircleSize, 0, cudaInCircleHandle, 0));

    // Release the handles for the allocation. Since the allocation is currently
    // mapped to a VA range with a previous call to cuMemMap the actual freeing of
    // memory allocation will happen on an eventual call to cuMemUnmap. Thus the
    // allocation will be kept live until it is unmapped.
    checkCudaErrors(cuMemRelease(cudaPositionHandle));
    checkCudaErrors(cuMemRelease(cudaInCircleHandle));

    CUmemAccessDesc accessDescriptor = {};
    accessDescriptor.location.id     = m_cudaDevice;
    accessDescriptor.location.type   = CU_MEM_LOCATION_TYPE_DEVICE;
    accessDescriptor.flags           = CU_MEM_ACCESS_FLAGS_PROT_READWRITE;

    // Apply the access descriptor to the whole VA range. Essentially enables
    // Read-Write access to the range.
    checkCudaErrors(cuMemSetAccess(d_ptr, m_totalAllocationSize, &accessDescriptor, 1));
}

void MonteCarloPiSimulation::cleanupSimulationAllocations()
{
    if (m_xyVector && m_pointsInsideCircle) {
        // Unmap the mapped virtual memory region
        // Since the handles to the mapped backing stores have already been released
        // by cuMemRelease, and these are the only/last mappings referencing them,
        // The backing stores will be freed.
        checkCudaErrors(cuMemUnmap((CUdeviceptr)m_xyVector, m_totalAllocationSize));

        checkIpcErrors(ipcCloseShareableHandle(m_posShareableHandle));
        checkIpcErrors(ipcCloseShareableHandle(m_inCircleShareableHandle));

        // Free the virtual address region.
        checkCudaErrors(cuMemAddressFree((CUdeviceptr)m_xyVector, m_totalAllocationSize));

        m_xyVector           = nullptr;
        m_pointsInsideCircle = nullptr;
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/simpleVulkanMMAP/MonteCarloPi.cu`.

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

- **Total Lines**: 291
- **Approximate Size**: 12646 bytes

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
