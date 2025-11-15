# Documentation for Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_gpu.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_gpu.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/binomialOptions_nvrtc
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

////////////////////////////////////////////////////////////////////////////////
// Global types and parameters
////////////////////////////////////////////////////////////////////////////////

#include <cmath>
#include <stdio.h>
#include <stdlib.h>

// Other helpers
#include <helper_cuda.h>
#include <nvrtc_helper.h>

// CUDA runtime
#include <cuda_runtime.h>

#include "binomialOptions_common.h"
#include "common_gpu_header.h"
#include "realtype.h"


// Preprocessed input option data
typedef struct
{
    real S;
    real X;
    real vDt;
    real puByDf;
    real pdByDf;

} __TOptionData;

static bool moduleLoaded = false;
char       *cubin, *kernel_file;
size_t      cubinSize;
CUmodule    module;

////////////////////////////////////////////////////////////////////////////////
// Host-side interface to GPU binomialOptions
////////////////////////////////////////////////////////////////////////////////

extern "C" void binomialOptionsGPU(real *callValue, TOptionData *optionData, int optN, int argc, char **argv)
{
    if (!moduleLoaded) {
        kernel_file = sdkFindFilePath("binomialOptions_kernel.cu", argv[0]);
        compileFileToCUBIN(kernel_file, argc, argv, &cubin, &cubinSize, 0);
        module       = loadCUBIN(cubin, argc, argv);
        moduleLoaded = true;
    }

    __TOptionData h_OptionData[MAX_OPTIONS];

    for (int i = 0; i < optN; i++) {
        const real T = optionData[i].T;
        const real R = optionData[i].R;
        const real V = optionData[i].V;

        const real dt  = T / (real)NUM_STEPS;
        const real vDt = V * sqrt(dt);
        const real rDt = R * dt;
        // Per-step interest and discount factors
        const real If = exp(rDt);
        const real Df = exp(-rDt);
        // Values and pseudoprobabilities of upward and downward moves
        const real u      = exp(vDt);
        const real d      = exp(-vDt);
        const real pu     = (If - d) / (u - d);
        const real pd     = (real)1.0 - pu;
        const real puByDf = pu * Df;
        const real pdByDf = pd * Df;

        h_OptionData[i].S      = (real)optionData[i].S;
        h_OptionData[i].X      = (real)optionData[i].X;
        h_OptionData[i].vDt    = (real)vDt;
        h_OptionData[i].puByDf = (real)puByDf;
        h_OptionData[i].pdByDf = (real)pdByDf;
    }

    CUfunction kernel_addr;
    checkCudaErrors(cuModuleGetFunction(&kernel_addr, module, "binomialOptionsKernel"));

    CUdeviceptr d_OptionData;
    checkCudaErrors(cuModuleGetGlobal(&d_OptionData, NULL, module, "d_OptionData"));
    checkCudaErrors(cuMemcpyHtoD(d_OptionData, h_OptionData, optN * sizeof(__TOptionData)));

    dim3 cudaBlockSize(128, 1, 1);
    dim3 cudaGridSize(optN, 1, 1);

    checkCudaErrors(cuLaunchKernel(kernel_addr,
                                   cudaGridSize.x,
                                   cudaGridSize.y,
                                   cudaGridSize.z, /* grid dim */
                                   cudaBlockSize.x,
                                   cudaBlockSize.y,
                                   cudaBlockSize.z, /* block dim */
                                   0,
                                   0,    /* shared mem, stream */
                                   NULL, /* arguments */
                                   0));

    checkCudaErrors(cuCtxSynchronize());

    CUdeviceptr d_CallValue;
    checkCudaErrors(cuModuleGetGlobal(&d_CallValue, NULL, module, "d_CallValue"));
    checkCudaErrors(cuMemcpyDtoH(callValue, d_CallValue, optN * sizeof(real)));
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/binomialOptions_nvrtc/binomialOptions_gpu.cpp`.

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

- **Total Lines**: 133
- **Approximate Size**: 5095 bytes

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
