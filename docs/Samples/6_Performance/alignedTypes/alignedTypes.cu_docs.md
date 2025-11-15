# Documentation for Samples/6_Performance/alignedTypes/alignedTypes.cu

## File Metadata

- **Path**: `Samples/6_Performance/alignedTypes/alignedTypes.cu`
- **Type**: .cu
- **Location**: Samples/6_Performance/alignedTypes
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
 * This is a simple test showing huge access speed gap
 * between aligned and misaligned structures
 * (those having/missing __align__ keyword).
 * It measures per-element copy throughput for
 * aligned and misaligned structures on
 * big chunks of data.
 */

// includes, system
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project
#include <helper_cuda.h>      // helper functions for CUDA error checking and initialization
#include <helper_functions.h> // helper utility functions

////////////////////////////////////////////////////////////////////////////////
// Misaligned types
////////////////////////////////////////////////////////////////////////////////
typedef unsigned char uint8;

typedef unsigned short int uint16;

typedef struct
{
    unsigned char r, g, b, a;
} RGBA8_misaligned;

typedef struct
{
    unsigned int l, a;
} LA32_misaligned;

typedef struct
{
    unsigned int r, g, b;
} RGB32_misaligned;

typedef struct
{
    unsigned int r, g, b, a;
} RGBA32_misaligned;

////////////////////////////////////////////////////////////////////////////////
// Aligned types
////////////////////////////////////////////////////////////////////////////////
typedef struct __align__(4)
{
    unsigned char r, g, b, a;
} RGBA8;

typedef unsigned int I32;

typedef struct __align__(8)
{
    unsigned int l, a;
} LA32;

typedef struct __align__(16)
{
    unsigned int r, g, b;
} RGB32;

typedef struct __align__(16)
{
    unsigned int r, g, b, a;
} RGBA32;

////////////////////////////////////////////////////////////////////////////////
// Because G80 class hardware natively supports global memory operations
// only with data elements of 4, 8 and 16 bytes, if structure size
// exceeds 16 bytes, it can't be efficiently read or written,
// since more than one global memory non-coalescable load/store instructions
// will be generated, even if __align__ option is supplied.
// "Structure of arrays" storage strategy offers best performance
// in general case. See section 5.1.2 of the Programming Guide.
////////////////////////////////////////////////////////////////////////////////
typedef struct __align__(16)
{
    RGBA32 c1, c2;
} RGBA32_2;

////////////////////////////////////////////////////////////////////////////////
// Common host and device functions
////////////////////////////////////////////////////////////////////////////////
// Round a / b to nearest higher integer value
int iDivUp(int a, int b) { return (a % b != 0) ? (a / b + 1) : (a / b); }

// Round a / b to nearest lower integer value
int iDivDown(int a, int b) { return a / b; }

// Align a to nearest higher multiple of b
int iAlignUp(int a, int b) { return (a % b != 0) ? (a - a % b + b) : a; }

// Align a to nearest lower multiple of b
int iAlignDown(int a, int b) { return a - a % b; }

////////////////////////////////////////////////////////////////////////////////
// Simple CUDA kernel.
// Copy is carried out on per-element basis,
// so it's not per-byte in case of padded structures.
////////////////////////////////////////////////////////////////////////////////
template <class TData> __global__ void testKernel(TData *d_odata, TData *d_idata, int numElements)
{
    const int tid        = blockDim.x * blockIdx.x + threadIdx.x;
    const int numThreads = blockDim.x * gridDim.x;

    for (int pos = tid; pos < numElements; pos += numThreads) {
        d_odata[pos] = d_idata[pos];
    }
}

////////////////////////////////////////////////////////////////////////////////
// Validation routine for simple copy kernel.
// We must know "packed" size of TData (number_of_fields * sizeof(simple_type))
// and compare only these "packed" parts of the structure,
// containing actual user data. The compiler behavior with padding bytes
// is undefined, since padding is merely a placeholder
// and doesn't contain any user data.
////////////////////////////////////////////////////////////////////////////////
template <class TData> int testCPU(TData *h_odata, TData *h_idata, int numElements, int packedElementSize)
{
    for (int pos = 0; pos < numElements; pos++) {
        TData src = h_idata[pos];
        TData dst = h_odata[pos];

        for (int i = 0; i < packedElementSize; i++)
            if (((char *)&src)[i] != ((char *)&dst)[i]) {
                return 0;
            }
    }

    return 1;
}

////////////////////////////////////////////////////////////////////////////////
// Data configuration
////////////////////////////////////////////////////////////////////////////////
// Memory chunk size in bytes. Reused for test
const int MEM_SIZE       = 50000000;
const int NUM_ITERATIONS = 32;

// GPU input and output data
unsigned char *d_idata, *d_odata;
// CPU input data and instance of GPU output data
unsigned char      *h_idataCPU, *h_odataGPU;
StopWatchInterface *hTimer = NULL;

template <class TData> int runTest(int packedElementSize, int memory_size)
{
    const int totalMemSizeAligned = iAlignDown(memory_size, sizeof(TData));
    const int numElements         = iDivDown(memory_size, sizeof(TData));

    // Clean output buffer before current test
    checkCudaErrors(cudaMemset(d_odata, 0, memory_size));
    // Run test
    checkCudaErrors(cudaDeviceSynchronize());
    sdkResetTimer(&hTimer);
    sdkStartTimer(&hTimer);

    for (int i = 0; i < NUM_ITERATIONS; i++) {
        testKernel<TData><<<64, 256>>>((TData *)d_odata, (TData *)d_idata, numElements);
        getLastCudaError("testKernel() execution failed\n");
    }

    checkCudaErrors(cudaDeviceSynchronize());
    sdkStopTimer(&hTimer);
    double gpuTime = sdkGetTimerValue(&hTimer) / NUM_ITERATIONS;
    printf("Avg. time: %f ms / Copy throughput: %f GB/s.\n",
           gpuTime,
           (double)totalMemSizeAligned / (gpuTime * 0.001 * 1073741824.0));

    // Read back GPU results and run validation
    checkCudaErrors(cudaMemcpy(h_odataGPU, d_odata, memory_size, cudaMemcpyDeviceToHost));
    int flag = testCPU((TData *)h_odataGPU, (TData *)h_idataCPU, numElements, packedElementSize);

    printf(flag ? "\tTEST OK\n" : "\tTEST FAILURE\n");

    return !flag;
}

int main(int argc, char **argv)
{
    int i, nTotalFailures = 0;

    int            devID;
    cudaDeviceProp deviceProp;
    printf("[%s] - Starting...\n", argv[0]);

    // find first CUDA device
    devID = findCudaDevice(argc, (const char **)argv);

    // get number of SMs on this GPU
    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, devID));
    printf("[%s] has %d MP(s) x %d (Cores/MP) = %d (Cores)\n",
           deviceProp.name,
           deviceProp.multiProcessorCount,
           _ConvertSMVer2Cores(deviceProp.major, deviceProp.minor),
           _ConvertSMVer2Cores(deviceProp.major, deviceProp.minor) * deviceProp.multiProcessorCount);

    // Anything that is less than 192 Cores will have a scaled down workload
    float scale_factor = max(
        (192.0f / (_ConvertSMVer2Cores(deviceProp.major, deviceProp.minor) * (float)deviceProp.multiProcessorCount)),
        1.0f);

    int MemorySize = (int)(MEM_SIZE / scale_factor) & 0xffffff00; // force multiple of 256 bytes

    printf("> Compute scaling value = %4.2f\n", scale_factor);
    printf("> Memory Size = %d\n", MemorySize);

    sdkCreateTimer(&hTimer);

    printf("Allocating memory...\n");
    h_idataCPU = (unsigned char *)malloc(MemorySize);
    h_odataGPU = (unsigned char *)malloc(MemorySize);
    checkCudaErrors(cudaMalloc((void **)&d_idata, MemorySize));
    checkCudaErrors(cudaMalloc((void **)&d_odata, MemorySize));

    printf("Generating host input data array...\n");

    for (i = 0; i < MemorySize; i++) {
        h_idataCPU[i] = (i & 0xFF) + 1;
    }

    printf("Uploading input data to GPU memory...\n");
    checkCudaErrors(cudaMemcpy(d_idata, h_idataCPU, MemorySize, cudaMemcpyHostToDevice));

    printf("Testing misaligned types...\n");
    printf("uint8...\n");
    nTotalFailures += runTest<uint8>(1, MemorySize);

    printf("uint16...\n");
    nTotalFailures += runTest<uint16>(2, MemorySize);

    printf("RGBA8_misaligned...\n");
    nTotalFailures += runTest<RGBA8_misaligned>(4, MemorySize);

    printf("LA32_misaligned...\n");
    nTotalFailures += runTest<LA32_misaligned>(8, MemorySize);

    printf("RGB32_misaligned...\n");
    nTotalFailures += runTest<RGB32_misaligned>(12, MemorySize);

    printf("RGBA32_misaligned...\n");
    nTotalFailures += runTest<RGBA32_misaligned>(16, MemorySize);

    printf("Testing aligned types...\n");
    printf("RGBA8...\n");
    nTotalFailures += runTest<RGBA8>(4, MemorySize);

    printf("I32...\n");
    nTotalFailures += runTest<I32>(4, MemorySize);

    printf("LA32...\n");
    nTotalFailures += runTest<LA32>(8, MemorySize);

    printf("RGB32...\n");
    nTotalFailures += runTest<RGB32>(12, MemorySize);

    printf("RGBA32...\n");
    nTotalFailures += runTest<RGBA32>(16, MemorySize);

    printf("RGBA32_2...\n");
    nTotalFailures += runTest<RGBA32_2>(32, MemorySize);

    printf("\n[alignedTypes] -> Test Results: %d Failures\n", nTotalFailures);

    printf("Shutting down...\n");
    checkCudaErrors(cudaFree(d_idata));
    checkCudaErrors(cudaFree(d_odata));
    free(h_odataGPU);
    free(h_idataCPU);

    sdkDeleteTimer(&hTimer);

    if (nTotalFailures != 0) {
        printf("Test failed!\n");
        exit(EXIT_FAILURE);
    }

    printf("Test passed\n");
    exit(EXIT_SUCCESS);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/6_Performance/alignedTypes/alignedTypes.cu`.

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

- **Total Lines**: 314
- **Approximate Size**: 10961 bytes

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
