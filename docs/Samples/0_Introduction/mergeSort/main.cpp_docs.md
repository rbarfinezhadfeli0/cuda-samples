# Documentation for Samples/0_Introduction/mergeSort/main.cpp

## File Metadata

- **Path**: `Samples/0_Introduction/mergeSort/main.cpp`
- **Type**: .cpp
- **Location**: Samples/0_Introduction/mergeSort
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp
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

#include <assert.h>
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_functions.h>
#include <stdio.h>
#include <stdlib.h>

#include "mergeSort_common.h"

////////////////////////////////////////////////////////////////////////////////
// Test driver
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    uint               *h_SrcKey, *h_SrcVal, *h_DstKey, *h_DstVal;
    uint               *d_SrcKey, *d_SrcVal, *d_BufKey, *d_BufVal, *d_DstKey, *d_DstVal;
    StopWatchInterface *hTimer = NULL;

    const uint N         = 4 * 1048576;
    const uint DIR       = 1;
    const uint numValues = 65536;

    printf("%s Starting...\n\n", argv[0]);

    int dev = findCudaDevice(argc, (const char **)argv);

    if (dev == -1) {
        return EXIT_FAILURE;
    }

    printf("Allocating and initializing host arrays...\n\n");
    sdkCreateTimer(&hTimer);
    h_SrcKey = (uint *)malloc(N * sizeof(uint));
    h_SrcVal = (uint *)malloc(N * sizeof(uint));
    h_DstKey = (uint *)malloc(N * sizeof(uint));
    h_DstVal = (uint *)malloc(N * sizeof(uint));

    srand(2009);

    for (uint i = 0; i < N; i++) {
        h_SrcKey[i] = rand() % numValues;
    }

    fillValues(h_SrcVal, N);

    printf("Allocating and initializing CUDA arrays...\n\n");
    checkCudaErrors(cudaMalloc((void **)&d_DstKey, N * sizeof(uint)));
    checkCudaErrors(cudaMalloc((void **)&d_DstVal, N * sizeof(uint)));
    checkCudaErrors(cudaMalloc((void **)&d_BufKey, N * sizeof(uint)));
    checkCudaErrors(cudaMalloc((void **)&d_BufVal, N * sizeof(uint)));
    checkCudaErrors(cudaMalloc((void **)&d_SrcKey, N * sizeof(uint)));
    checkCudaErrors(cudaMalloc((void **)&d_SrcVal, N * sizeof(uint)));
    checkCudaErrors(cudaMemcpy(d_SrcKey, h_SrcKey, N * sizeof(uint), cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(d_SrcVal, h_SrcVal, N * sizeof(uint), cudaMemcpyHostToDevice));

    printf("Initializing GPU merge sort...\n");
    initMergeSort();

    printf("Running GPU merge sort...\n");
    checkCudaErrors(cudaDeviceSynchronize());
    sdkResetTimer(&hTimer);
    sdkStartTimer(&hTimer);
    mergeSort(d_DstKey, d_DstVal, d_BufKey, d_BufVal, d_SrcKey, d_SrcVal, N, DIR);
    checkCudaErrors(cudaDeviceSynchronize());
    sdkStopTimer(&hTimer);
    printf("Time: %f ms\n", sdkGetTimerValue(&hTimer));

    printf("Reading back GPU merge sort results...\n");
    checkCudaErrors(cudaMemcpy(h_DstKey, d_DstKey, N * sizeof(uint), cudaMemcpyDeviceToHost));
    checkCudaErrors(cudaMemcpy(h_DstVal, d_DstVal, N * sizeof(uint), cudaMemcpyDeviceToHost));

    printf("Inspecting the results...\n");
    uint keysFlag = validateSortedKeys(h_DstKey, h_SrcKey, 1, N, numValues, DIR);

    uint valuesFlag = validateSortedValues(h_DstKey, h_DstVal, h_SrcKey, 1, N);

    printf("Shutting down...\n");
    closeMergeSort();
    sdkDeleteTimer(&hTimer);
    checkCudaErrors(cudaFree(d_SrcVal));
    checkCudaErrors(cudaFree(d_SrcKey));
    checkCudaErrors(cudaFree(d_BufVal));
    checkCudaErrors(cudaFree(d_BufKey));
    checkCudaErrors(cudaFree(d_DstVal));
    checkCudaErrors(cudaFree(d_DstKey));
    free(h_DstVal);
    free(h_DstKey);
    free(h_SrcVal);
    free(h_SrcKey);

    exit((keysFlag && valuesFlag) ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/mergeSort/main.cpp`.

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

- **Total Lines**: 120
- **Approximate Size**: 4867 bytes

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
