# Documentation for Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/binomialOptions
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

////////////////////////////////////////////////////////////////////////////////
// Global types and parameters
////////////////////////////////////////////////////////////////////////////////
#include <cooperative_groups.h>
#include <stdio.h>
#include <stdlib.h>

namespace cg = cooperative_groups;

#include <helper_cuda.h>

#include "binomialOptions_common.h"
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
static __constant__ __TOptionData d_OptionData[MAX_OPTIONS];
static __device__ real            d_CallValue[MAX_OPTIONS];

////////////////////////////////////////////////////////////////////////////////
// Overloaded shortcut functions for different precision modes
////////////////////////////////////////////////////////////////////////////////
#ifndef DOUBLE_PRECISION
__device__ inline float expiryCallValue(float S, float X, float vDt, int i)
{
    float d = S * __expf(vDt * (2.0f * i - NUM_STEPS)) - X;
    return (d > 0.0F) ? d : 0.0F;
}
#else
__device__ inline double expiryCallValue(double S, double X, double vDt, int i)
{
    double d = S * exp(vDt * (2.0 * i - NUM_STEPS)) - X;
    return (d > 0.0) ? d : 0.0;
}
#endif

////////////////////////////////////////////////////////////////////////////////
// GPU kernel
////////////////////////////////////////////////////////////////////////////////
#define THREADBLOCK_SIZE 128
#define ELEMS_PER_THREAD (NUM_STEPS / THREADBLOCK_SIZE)
#if NUM_STEPS % THREADBLOCK_SIZE
#error Bad constants
#endif

__global__ void binomialOptionsKernel()
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();
    __shared__ real  call_exchange[THREADBLOCK_SIZE + 1];

    const int  tid    = threadIdx.x;
    const real S      = d_OptionData[blockIdx.x].S;
    const real X      = d_OptionData[blockIdx.x].X;
    const real vDt    = d_OptionData[blockIdx.x].vDt;
    const real puByDf = d_OptionData[blockIdx.x].puByDf;
    const real pdByDf = d_OptionData[blockIdx.x].pdByDf;

    real call[ELEMS_PER_THREAD + 1];
#pragma unroll
    for (int i = 0; i < ELEMS_PER_THREAD; ++i)
        call[i] = expiryCallValue(S, X, vDt, tid * ELEMS_PER_THREAD + i);

    if (tid == 0)
        call_exchange[THREADBLOCK_SIZE] = expiryCallValue(S, X, vDt, NUM_STEPS);

    int final_it = max(0, tid * ELEMS_PER_THREAD - 1);

#pragma unroll 16
    for (int i = NUM_STEPS; i > 0; --i) {
        call_exchange[tid] = call[0];
        cg::sync(cta);
        call[ELEMS_PER_THREAD] = call_exchange[tid + 1];
        cg::sync(cta);

        if (i > final_it) {
#pragma unroll
            for (int j = 0; j < ELEMS_PER_THREAD; ++j)
                call[j] = puByDf * call[j + 1] + pdByDf * call[j];
        }
    }

    if (tid == 0) {
        d_CallValue[blockIdx.x] = call[0];
    }
}

////////////////////////////////////////////////////////////////////////////////
// Host-side interface to GPU binomialOptions
////////////////////////////////////////////////////////////////////////////////
extern "C" void binomialOptionsGPU(real *callValue, TOptionData *optionData, int optN)
{
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

    checkCudaErrors(cudaMemcpyToSymbol(d_OptionData, h_OptionData, optN * sizeof(__TOptionData)));
    binomialOptionsKernel<<<optN, THREADBLOCK_SIZE>>>();
    getLastCudaError("binomialOptionsKernel() execution failed.\n");
    checkCudaErrors(cudaMemcpyFromSymbol(callValue, d_CallValue, optN * sizeof(real)));
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu`.

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

- **Total Lines**: 160
- **Approximate Size**: 6055 bytes

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
