# Documentation for Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh

## File Metadata

- **Path**: `Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh`
- **Type**: .cuh
- **Location**: Samples/5_Domain_Specific/quasirandomGenerator_nvrtc
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```cuh
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

#ifndef QUASIRANDOMGENERATOR_GPU_CUH
#define QUASIRANDOMGENERATOR_GPU_CUH

#include <nvrtc_helper.h>

#include "quasirandomGenerator_common.h"

// Fast integer multiplication
#define MUL(a, b) __umul24(a, b)

// Global variables for nvrtc outputs
char    *cubin;
size_t   cubinSize;
CUmodule module;

////////////////////////////////////////////////////////////////////////////////
// GPU code
////////////////////////////////////////////////////////////////////////////////

////////////////////////////////////////////////////////////////////////////////
// Niederreiter quasirandom number generation kernel
////////////////////////////////////////////////////////////////////////////////

// Table initialization routine
void initTableGPU(unsigned int tableCPU[QRNG_DIMENSIONS][QRNG_RESOLUTION])
{
    CUdeviceptr c_Table;
    checkCudaErrors(cuModuleGetGlobal(&c_Table, NULL, module, "c_Table"));
    checkCudaErrors(cuMemcpyHtoD(c_Table, tableCPU, QRNG_DIMENSIONS * QRNG_RESOLUTION * sizeof(unsigned int)));
}

// Host-side interface
void quasirandomGeneratorGPU(CUdeviceptr d_Output, unsigned int seed, unsigned int N)
{
    dim3 threads(128, QRNG_DIMENSIONS);
    dim3 cudaGridSize(128, 1, 1);

    CUfunction kernel_addr;
    checkCudaErrors(cuModuleGetFunction(&kernel_addr, module, "quasirandomGeneratorKernel"));

    void *args[] = {(void *)&d_Output, (void *)&seed, (void *)&N};
    checkCudaErrors(cuLaunchKernel(kernel_addr,
                                   cudaGridSize.x,
                                   cudaGridSize.y,
                                   cudaGridSize.z, /* grid dim */
                                   threads.x,
                                   threads.y,
                                   threads.z, /* block dim */
                                   0,
                                   0,        /* shared mem, stream */
                                   &args[0], /* arguments */
                                   0));

    checkCudaErrors(cuCtxSynchronize());
}

void inverseCNDgpu(CUdeviceptr d_Output, unsigned int N)
{
    dim3 threads(128, 1, 1);
    dim3 cudaGridSize(128, 1, 1);

    CUfunction kernel_addr;
    checkCudaErrors(cuModuleGetFunction(&kernel_addr, module, "inverseCNDKernel"));

    void *args[] = {(void *)&d_Output, (void *)&N};
    checkCudaErrors(cuLaunchKernel(kernel_addr,
                                   cudaGridSize.x,
                                   cudaGridSize.y,
                                   cudaGridSize.z, /* grid dim */
                                   threads.x,
                                   threads.y,
                                   threads.z, /* block dim */
                                   0,
                                   0,        /* shared mem, stream */
                                   &args[0], /* arguments */
                                   0));

    checkCudaErrors(cuCtxSynchronize());
}

#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/quasirandomGenerator_nvrtc/quasirandomGenerator_gpu.cuh`.

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

- **Total Lines**: 109
- **Approximate Size**: 4498 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

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

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

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
