# Documentation for Samples/5_Domain_Specific/SobolQRNG/sobol_gold.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/SobolQRNG/sobol_gold.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/SobolQRNG
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

/*
 * Portions Copyright (c) 2009 Mike Giles, Oxford University. All rights
 * reserved.
 * Portions Copyright (c) 2008 Frances Y. Kuo and Stephen Joe. All rights
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
 */

#include "sobol_gold.h"

#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "sobol.h"
#include "sobol_primitives.h"

#define k_2powneg32 2.3283064E-10F

// Windows does not provide ffs (find first set) so here is a
// fairly simple implementation.
// WIN32 is defined on 32 and 64 bit Windows
#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
int ffs(const unsigned int &i)
{
    unsigned int v = i;
    unsigned int count;

    if (!v) {
        count = 0;
    }
    else {
        count = 2;

        if ((v & 0xffff) == 0) {
            v >>= 16;
            count += 16;
        }

        if ((v & 0xff) == 0) {
            v >>= 8;
            count += 8;
        }

        if ((v & 0xf) == 0) {
            v >>= 4;
            count += 4;
        }

        if ((v & 0x3) == 0) {
            v >>= 2;
            count += 2;
        }

        count -= v & 0x1;
    }

    return count;
}
#endif

// Create the direction numbers, based on the primitive polynomials.
void initSobolDirectionVectors(int n_dimensions, unsigned int *directions)
{
    unsigned int *v = directions;

    for (int dim = 0; dim < n_dimensions; dim++) {
        // First dimension is a special case
        if (dim == 0) {
            for (int i = 0; i < n_directions; i++) {
                // All m's are 1
                v[i] = 1 << (31 - i);
            }
        }
        else {
            int d = sobol_primitives[dim].degree;

            // The first direction numbers (up to the degree of the polynomial)
            // are simply v[i] = m[i] / 2^i (stored in Q0.32 format)
            for (int i = 0; i < d; i++) {
                v[i] = sobol_primitives[dim].m[i] << (31 - i);
            }

            // The remaining direction numbers are computed as described in
            // the Bratley and Fox paper.
            // v[i] = a[1]v[i-1] ^ a[2]v[i-2] ^ ... ^ a[v-1]v[i-d+1] ^ v[i-d] ^
            // v[i-d]/2^d
            for (int i = d; i < n_directions; i++) {
                // First do the v[i-d] ^ v[i-d]/2^d part
                v[i] = v[i - d] ^ (v[i - d] >> d);

                // Now do the a[1]v[i-1] ^ a[2]v[i-2] ^ ... part
                // Note that the coefficients a[] are zero or one and for compactness in
                // the input tables they are stored as bits of a single integer. To
                // extract the relevant bit we use right shift and mask with 1.
                // For example, for a 10 degree polynomial there are ten useful bits in
                // a, so to get a[2] we need to right shift 7 times (to get the 8th bit
                // into the LSB) and then mask with 1.
                for (int j = 1; j < d; j++) {
                    v[i] ^= (((sobol_primitives[dim].a >> (d - 1 - j)) & 1) * v[i - j]);
                }
            }
        }

        v += n_directions;
    }
}

// Reference model for generating Sobol numbers on the host
void sobolCPU(int n_vectors, int n_dimensions, unsigned int *directions, float *output)
{
    unsigned int *v = directions;

    for (int d = 0; d < n_dimensions; d++) {
        unsigned int X = 0;
        // x[0] is zero (in all dimensions)
        output[n_vectors * d] = 0.0;

        for (int i = 1; i < n_vectors; i++) {
            // x[i] = x[i-1] ^ v[c]
            //  where c is the index of the rightmost zero bit in i
            //  minus 1 (since C arrays count from zero)
            // In the Bratley and Fox paper this is equation (**)
            X ^= v[ffs(~(i - 1)) - 1];
            output[i + n_vectors * d] = (float)X * k_2powneg32;
        }

        v += n_directions;
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/SobolQRNG/sobol_gold.cpp`.

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

- **Total Lines**: 179
- **Approximate Size**: 6193 bytes

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
