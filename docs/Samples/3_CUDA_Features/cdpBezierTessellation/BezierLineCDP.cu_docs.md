# Documentation for Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/cdpBezierTessellation
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

#include <cuda_runtime_api.h>
#include <helper_cuda.h>
#include <stdio.h>
#include <string.h>

__forceinline__ __device__ float2 operator+(float2 a, float2 b)
{
    float2 c;
    c.x = a.x + b.x;
    c.y = a.y + b.y;
    return c;
}

__forceinline__ __device__ float2 operator-(float2 a, float2 b)
{
    float2 c;
    c.x = a.x - b.x;
    c.y = a.y - b.y;
    return c;
}

__forceinline__ __device__ float2 operator*(float a, float2 b)
{
    float2 c;
    c.x = a * b.x;
    c.y = a * b.y;
    return c;
}

__forceinline__ __device__ float length(float2 a) { return sqrtf(a.x * a.x + a.y * a.y); }

#define MAX_TESSELLATION 32
struct BezierLine
{
    float2  CP[3];
    float2 *vertexPos;
    int     nVertices;
};

__global__ void computeBezierLinePositions(int lidx, BezierLine *bLines, int nTessPoints)
{
    int idx = threadIdx.x + blockDim.x * blockIdx.x;

    if (idx < nTessPoints) {
        float u   = (float)idx / (float)(nTessPoints - 1);
        float omu = 1.0f - u;

        float B3u[3];

        B3u[0] = omu * omu;
        B3u[1] = 2.0f * u * omu;
        B3u[2] = u * u;

        float2 position = {0, 0};

        for (int i = 0; i < 3; i++) {
            position = position + B3u[i] * bLines[lidx].CP[i];
        }

        bLines[lidx].vertexPos[idx] = position;
    }
}

__global__ void computeBezierLinesCDP(BezierLine *bLines, int nLines)
{
    int lidx = threadIdx.x + blockDim.x * blockIdx.x;

    if (lidx < nLines) {
        float curvature = length(bLines[lidx].CP[1] - 0.5f * (bLines[lidx].CP[0] + bLines[lidx].CP[2]))
                        / length(bLines[lidx].CP[2] - bLines[lidx].CP[0]);
        int nTessPoints = min(max((int)(curvature * 16.0f), 4), MAX_TESSELLATION);

        if (bLines[lidx].vertexPos == NULL) {
            bLines[lidx].nVertices = nTessPoints;
            cudaMalloc((void **)&bLines[lidx].vertexPos, nTessPoints * sizeof(float2));
        }

        computeBezierLinePositions<<<ceilf((float)bLines[lidx].nVertices / 32.0f), 32>>>(
            lidx, bLines, bLines[lidx].nVertices);
    }
}

__global__ void freeVertexMem(BezierLine *bLines, int nLines)
{
    int lidx = threadIdx.x + blockDim.x * blockIdx.x;

    if (lidx < nLines)
        cudaFree(bLines[lidx].vertexPos);
}

unsigned int checkCapableSM35Device(int argc, char **argv)
{
    // Get device properties
    cudaDeviceProp properties;
    int            device_count = 0, device = -1;

    if (checkCmdLineFlag(argc, (const char **)argv, "device")) {
        device = getCmdLineArgumentInt(argc, (const char **)argv, "device");

        cudaDeviceProp properties;
        checkCudaErrors(cudaGetDeviceProperties(&properties, device));

        if (properties.major > 3 || (properties.major == 3 && properties.minor >= 5)) {
            printf("Running on GPU  %d (%s)\n", device, properties.name);
        }
        else {
            printf("cdpBezierTessellation requires GPU devices with compute SM 3.5 or "
                   "higher.");
            printf("Current GPU device has compute SM %d.%d. Exiting...\n", properties.major, properties.minor);
            return EXIT_FAILURE;
        }
    }
    else {
        checkCudaErrors(cudaGetDeviceCount(&device_count));

        for (int i = 0; i < device_count; ++i) {
            checkCudaErrors(cudaGetDeviceProperties(&properties, i));

            if (properties.major > 3 || (properties.major == 3 && properties.minor >= 5)) {
                device = i;
                printf("Running on GPU %d (%s)\n", i, properties.name);
                break;
            }

            printf("GPU %d %s does not support CUDA Dynamic Parallelism\n", i, properties.name);
        }
    }
    if (device == -1) {
        fprintf(stderr,
                "cdpBezierTessellation requires GPU devices with compute SM 3.5 or "
                "higher.  Exiting...\n");
        return EXIT_WAIVED;
    }

    return EXIT_SUCCESS;
}

#define N_LINES   256
#define BLOCK_DIM 64
int main(int argc, char **argv)
{
    BezierLine *bLines_h = new BezierLine[N_LINES];

    float2 last = {0, 0};

    for (int i = 0; i < N_LINES; i++) {
        bLines_h[i].CP[0] = last;

        for (int j = 1; j < 3; j++) {
            bLines_h[i].CP[j].x = (float)rand() / (float)RAND_MAX;
            bLines_h[i].CP[j].y = (float)rand() / (float)RAND_MAX;
        }

        last                  = bLines_h[i].CP[2];
        bLines_h[i].vertexPos = NULL;
        bLines_h[i].nVertices = 0;
    }

    unsigned int sm35Ret = checkCapableSM35Device(argc, argv);
    if (sm35Ret != EXIT_SUCCESS) {
        exit(sm35Ret);
    }

    BezierLine *bLines_d;
    checkCudaErrors(cudaMalloc((void **)&bLines_d, N_LINES * sizeof(BezierLine)));
    checkCudaErrors(cudaMemcpy(bLines_d, bLines_h, N_LINES * sizeof(BezierLine), cudaMemcpyHostToDevice));
    printf("Computing Bezier Lines (CUDA Dynamic Parallelism Version) ... ");
    computeBezierLinesCDP<<<(unsigned int)ceil((float)N_LINES / (float)BLOCK_DIM), BLOCK_DIM>>>(bLines_d, N_LINES);
    printf("Done!\n");

    // Do something to draw the lines here

    freeVertexMem<<<(unsigned int)ceil((float)N_LINES / (float)BLOCK_DIM), BLOCK_DIM>>>(bLines_d, N_LINES);
    checkCudaErrors(cudaFree(bLines_d));
    delete[] bLines_h;

    exit(EXIT_SUCCESS);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu`.

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

- **Total Lines**: 206
- **Approximate Size**: 6818 bytes

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
