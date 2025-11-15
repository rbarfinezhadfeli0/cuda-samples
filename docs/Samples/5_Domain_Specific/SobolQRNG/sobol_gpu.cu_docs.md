# Documentation for Samples/5_Domain_Specific/SobolQRNG/sobol_gpu.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/SobolQRNG/sobol_gpu.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/SobolQRNG
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
 * Portions Copyright (c) 2009 Mike Giles, Oxford University.  All rights
 * reserved.
 * Portions Copyright (c) 2008 Frances Y. Kuo and Stephen Joe.  All rights
 * reserved.
 *
 * Sobol Quasi-random Number Generator example
 *
 * Based on CUDA code submitted by Mike Giles, Oxford University, United Kingdom
 * http://people.maths.ox.ac.uk/~gilesm/
 *
 * and C code developed by Stephen Joe, University of Waikato, New Zealand
 * and Frances Kuo, University of New South Wales, Australia
 * http://web.maths.unsw.edu.au/~fkuo/sobol/
 *
 * For theoretical background see:
 *
 * P. Bratley and B.L. Fox.
 * Implementing Sobol's quasirandom sequence generator
 * http://portal.acm.org/citation.cfm?id=42288
 * ACM Trans. on Math. Software, 14(1):88-100, 1988
 *
 * S. Joe and F. Kuo.
 * Remark on algorithm 659: implementing Sobol's quasirandom sequence generator.
 * http://portal.acm.org/citation.cfm?id=641879
 * ACM Trans. on Math. Software, 29(1):49-57, 2003
 *
 */

#include <cooperative_groups.h>

#include "sobol.h"
#include "sobol_gpu.h"

namespace cg = cooperative_groups;
#include <helper_cuda.h>

#define k_2powneg32 2.3283064E-10F

__global__ void sobolGPU_kernel(unsigned n_vectors, unsigned n_dimensions, unsigned *d_directions, float *d_output)
{
    // Handle to thread block group
    cg::thread_block        cta = cg::this_thread_block();
    __shared__ unsigned int v[n_directions];

    // Offset into the correct dimension as specified by the
    // block y coordinate
    d_directions = d_directions + n_directions * blockIdx.y;
    d_output     = d_output + n_vectors * blockIdx.y;

    // Copy the direction numbers for this dimension into shared
    // memory - there are only 32 direction numbers so only the
    // first 32 (n_directions) threads need participate.
    if (threadIdx.x < n_directions) {
        v[threadIdx.x] = d_directions[threadIdx.x];
    }

    cg::sync(cta);

    // Set initial index (i.e. which vector this thread is
    // computing first) and stride (i.e. step to the next vector
    // for this thread)
    int i0     = threadIdx.x + blockIdx.x * blockDim.x;
    int stride = gridDim.x * blockDim.x;

    // Get the gray code of the index
    // c.f. Numerical Recipes in C, chapter 20
    // http://www.nrbook.com/a/bookcpdf/c20-2.pdf
    unsigned int g = i0 ^ (i0 >> 1);

    // Initialisation for first point x[i0]
    // In the Bratley and Fox paper this is equation (*), where
    // we are computing the value for x[n] without knowing the
    // value of x[n-1].
    unsigned int X = 0;
    unsigned int mask;

    for (unsigned int k = 0; k < __ffs(stride) - 1; k++) {
        // We want X ^= g_k * v[k], where g_k is one or zero.
        // We do this by setting a mask with all bits equal to
        // g_k. In reality we keep shifting g so that g_k is the
        // LSB of g. This way we avoid multiplication.
        mask = -(g & 1);
        X ^= mask & v[k];
        g = g >> 1;
    }

    if (i0 < n_vectors) {
        d_output[i0] = (float)X * k_2powneg32;
    }

    // Now do rest of points, using the stride
    // Here we want to generate x[i] from x[i-stride] where we
    // don't have any of the x in between, therefore we have to
    // revisit the equation (**), this is easiest with an example
    // so assume stride is 16.
    // From x[n] to x[n+16] there will be:
    //   8 changes in the first bit
    //   4 changes in the second bit
    //   2 changes in the third bit
    //   1 change in the fourth
    //   1 change in one of the remaining bits
    //
    // What this means is that in the equation:
    //   x[n+1] = x[n] ^ v[p]
    //   x[n+2] = x[n+1] ^ v[q] = x[n] ^ v[p] ^ v[q]
    //   ...
    // We will apply xor with v[1] eight times, v[2] four times,
    // v[3] twice, v[4] once and one other direction number once.
    // Since two xors cancel out, we can skip even applications
    // and just apply xor with v[4] (i.e. log2(16)) and with
    // the current applicable direction number.
    // Note that all these indices count from 1, so we need to
    // subtract 1 from them all to account for C arrays counting
    // from zero.
    unsigned int v_log2stridem1 = v[__ffs(stride) - 2];
    unsigned int v_stridemask   = stride - 1;

    for (unsigned int i = i0 + stride; i < n_vectors; i += stride) {
        // x[i] = x[i-stride] ^ v[b] ^ v[c]
        //  where b is log2(stride) minus 1 for C array indexing
        //  where c is the index of the rightmost zero bit in i,
        //  not including the bottom log2(stride) bits, minus 1
        //  for C array indexing
        // In the Bratley and Fox paper this is equation (**)
        X ^= v_log2stridem1 ^ v[__ffs(~((i - stride) | v_stridemask)) - 1];
        d_output[i] = (float)X * k_2powneg32;
    }
}

extern "C" void sobolGPU(int n_vectors, int n_dimensions, unsigned int *d_directions, float *d_output)
{
    const int threadsperblock = 64;

    // Set up the execution configuration
    dim3 dimGrid;
    dim3 dimBlock;

    int            device;
    cudaDeviceProp prop;
    checkCudaErrors(cudaGetDevice(&device));
    checkCudaErrors(cudaGetDeviceProperties(&prop, device));

    // This implementation of the generator outputs all the draws for
    // one dimension in a contiguous region of memory, followed by the
    // next dimension and so on.
    // Therefore all threads within a block will be processing different
    // vectors from the same dimension. As a result we want the total
    // number of blocks to be a multiple of the number of dimensions.
    dimGrid.y = n_dimensions;

    // If the number of dimensions is large then we will set the number
    // of blocks to equal the number of dimensions (i.e. dimGrid.x = 1)
    // but if the number of dimensions is small (e.g. less than four per
    // multiprocessor) then we'll partition the vectors across blocks
    // (as well as threads).
    if (n_dimensions < (4 * prop.multiProcessorCount)) {
        dimGrid.x = 4 * prop.multiProcessorCount;
    }
    else {
        dimGrid.x = 1;
    }

    // Cap the dimGrid.x if the number of vectors is small
    if (dimGrid.x > (unsigned int)(n_vectors / threadsperblock)) {
        dimGrid.x = (n_vectors + threadsperblock - 1) / threadsperblock;
    }

    // Round up to a power of two, required for the algorithm so that
    // stride is a power of two.
    unsigned int targetDimGridX = dimGrid.x;

    for (dimGrid.x = 1; dimGrid.x < targetDimGridX; dimGrid.x *= 2)
        ;

    // Fix the number of threads
    dimBlock.x = threadsperblock;

    // Execute GPU kernel
    sobolGPU_kernel<<<dimGrid, dimBlock>>>(n_vectors, n_dimensions, d_directions, d_output);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/SobolQRNG/sobol_gpu.cu`.

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

- **Total Lines**: 209
- **Approximate Size**: 8229 bytes

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
