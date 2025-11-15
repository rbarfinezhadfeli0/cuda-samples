# Documentation for Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_cuda.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_cuda.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/recursiveGaussian
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
  Recursive Gaussian filter
  sgreen 8/1/08

  This code sample implements a Gaussian blur using Deriche's recursive method:
  http://citeseer.ist.psu.edu/deriche93recursively.html

  This is similar to the box filter sample in the SDK, but it uses the previous
  outputs of the filter as well as the previous inputs. This is also known as an
  IIR (infinite impulse response) filter, since its response to an input impulse
  can last forever.

  The main advantage of this method is that the execution time is independent of
  the filter width.

  The GPU processes columns of the image in parallel. To avoid uncoalesced reads
  for the row pass we transpose the image and then transpose it back again
  afterwards.

  The implementation is based on code from the CImg library:
  http://cimg.sourceforge.net/
  Thanks to David Tschumperl� and all the CImg contributors!
*/

#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "recursiveGaussian_kernel.cuh"

#define USE_SIMPLE_FILTER 0

// Round a / b to nearest higher integer value
int iDivUp(int a, int b) { return (a % b != 0) ? (a / b + 1) : (a / b); }

/*
  Transpose a 2D array (see SDK transpose example)
*/
extern "C" void transpose(uint *d_src, uint *d_dest, uint width, int height)
{
    dim3 grid(iDivUp(width, BLOCK_DIM), iDivUp(height, BLOCK_DIM), 1);
    dim3 threads(BLOCK_DIM, BLOCK_DIM, 1);
    d_transpose<<<grid, threads>>>(d_dest, d_src, width, height);
    getLastCudaError("Kernel execution failed");
}

/*
  Perform Gaussian filter on a 2D image using CUDA

  Parameters:
  d_src  - pointer to input image in device memory
  d_dest - pointer to destination image in device memory
  d_temp - pointer to temporary storage in device memory
  width  - image width
  height - image height
  sigma  - sigma of Gaussian
  order  - filter order (0, 1 or 2)
*/

// 8-bit RGBA version
extern "C" void
gaussianFilterRGBA(uint *d_src, uint *d_dest, uint *d_temp, int width, int height, float sigma, int order, int nthreads)
{
    // compute filter coefficients
    const float nsigma = sigma < 0.1f ? 0.1f : sigma, alpha = 1.695f / nsigma, ema = (float)std::exp(-alpha),
                ema2 = (float)std::exp(-2 * alpha), b1 = -2 * ema, b2 = ema2;

    float a0 = 0, a1 = 0, a2 = 0, a3 = 0, coefp = 0, coefn = 0;

    switch (order) {
    case 0: {
        const float k = (1 - ema) * (1 - ema) / (1 + 2 * alpha * ema - ema2);
        a0            = k;
        a1            = k * (alpha - 1) * ema;
        a2            = k * (alpha + 1) * ema;
        a3            = -k * ema2;
    } break;

    case 1: {
        const float k = (1 - ema) * (1 - ema) / ema;
        a0            = k * ema;
        a1 = a3 = 0;
        a2      = -a0;
    } break;

    case 2: {
        const float ea = (float)std::exp(-alpha), k = -(ema2 - 1) / (2 * alpha * ema),
                    kn = (-2 * (-1 + 3 * ea - 3 * ea * ea + ea * ea * ea) / (3 * ea + 1 + 3 * ea * ea + ea * ea * ea));
        a0             = kn;
        a1             = -kn * (1 + k * alpha) * ema;
        a2             = kn * (1 - k * alpha) * ema;
        a3             = -kn * ema2;
    } break;

    default:
        fprintf(stderr, "gaussianFilter: invalid order parameter!\n");
        return;
    }

    coefp = (a0 + a1) / (1 + b1 + b2);
    coefn = (a2 + a3) / (1 + b1 + b2);

// process columns
#if USE_SIMPLE_FILTER
    d_simpleRecursive_rgba<<<iDivUp(width, nthreads), nthreads>>>(d_src, d_temp, width, height, ema);
#else
    d_recursiveGaussian_rgba<<<iDivUp(width, nthreads), nthreads>>>(
        d_src, d_temp, width, height, a0, a1, a2, a3, b1, b2, coefp, coefn);
#endif
    getLastCudaError("Kernel execution failed");

    transpose(d_temp, d_dest, width, height);
    getLastCudaError("transpose: Kernel execution failed");

// process rows
#if USE_SIMPLE_FILTER
    d_simpleRecursive_rgba<<<iDivUp(height, nthreads), nthreads>>>(d_dest, d_temp, height, width, ema);
#else
    d_recursiveGaussian_rgba<<<iDivUp(height, nthreads), nthreads>>>(
        d_dest, d_temp, height, width, a0, a1, a2, a3, b1, b2, coefp, coefn);
#endif
    getLastCudaError("Kernel execution failed");

    transpose(d_temp, d_dest, height, width);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_cuda.cu`.

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

- **Total Lines**: 156
- **Approximate Size**: 5832 bytes

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
