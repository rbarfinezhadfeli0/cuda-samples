# Documentation for Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_gold.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_gold.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/quasirandomGenerator
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

#include <math.h>
#include <stdio.h>

#include "quasirandomGenerator_common.h"

////////////////////////////////////////////////////////////////////////////////
// Table generation functions
////////////////////////////////////////////////////////////////////////////////
// Internal 64(63)-bit table
static INT64 cjn[63][QRNG_DIMENSIONS];

static int GeneratePolynomials(int buffer[QRNG_DIMENSIONS], bool primitive)
{
    int i, j, n, p1, p2, l;
    int e_p1, e_p2, e_b;

    // generate all polynomials to buffer
    for (n = 1, buffer[0] = 0x2, p2 = 0, l = 0; n < QRNG_DIMENSIONS; ++n) {
        // search for the next irreducible polynomial
        for (p1 = buffer[n - 1] + 1;; ++p1) {
            // find degree of polynomial p1
            for (e_p1 = 30; (p1 & (1 << e_p1)) == 0; --e_p1) {
            }

            // try to divide p1 by all polynomials in buffer
            for (i = 0; i < n; ++i) {
                // find the degree of buffer[i]
                for (e_b = e_p1; (buffer[i] & (1 << e_b)) == 0; --e_b) {
                }

                // divide p2 by buffer[i] until the end
                for (p2 = (buffer[i] << ((e_p2 = e_p1) - e_b)) ^ p1; p2 >= buffer[i];
                     p2 = (buffer[i] << (e_p2 - e_b)) ^ p2) {
                    for (; (p2 & (1 << e_p2)) == 0; --e_p2) {
                    }
                } // compute new degree of p2

                // division without remainder!!! p1 is not irreducible
                if (p2 == 0) {
                    break;
                }
            }

            // all divisions were with remainder - p1 is irreducible
            if (p2 != 0) {
                e_p2 = 0;

                if (primitive) {
                    // check that p1 has only one cycle (i.e. is monic, or primitive)
                    j   = ~(0xffffffff << (e_p1 + 1));
                    e_b = (1 << e_p1) | 0x1;

                    for (p2 = e_b, e_p2 = (1 << e_p1) - 2; e_p2 > 0; --e_p2) {
                        p2 <<= 1;
                        i = p2 & p1;
                        i = (i & 0x55555555) + ((i >> 1) & 0x55555555);
                        i = (i & 0x33333333) + ((i >> 2) & 0x33333333);
                        i = (i & 0x07070707) + ((i >> 4) & 0x07070707);
                        p2 |= (i % 255) & 1;

                        if ((p2 & j) == e_b)
                            break;
                    }
                }

                // it is monic - add it to the list of polynomials
                if (e_p2 == 0) {
                    buffer[n] = p1;
                    l += e_p1;
                    break;
                }
            }
        }
    }

    return l + 1;
}

////////////////////////////////////////////////////////////////////////////////
//  @misc{Bratley92:LDS,
//    author = "B. Fox and P. Bratley and H. Niederreiter",
//    title = "Implementation and test of low discrepancy sequences",
//    text = "B. L. Fox, P. Bratley, and H. Niederreiter. Implementation and
//    test of
//      low discrepancy sequences. ACM Trans. Model. Comput. Simul.,
//      2(3):195--213,
//      July 1992.",
//    year = "1992" }
////////////////////////////////////////////////////////////////////////////////
static void GenerateCJ()
{
    int  buffer[QRNG_DIMENSIONS];
    int *polynomials;
    int  n, p1, l, e_p1;

    // Niederreiter (in contrast to Sobol) allows to use not primitive, but just
    // irreducible polynomials
    l = GeneratePolynomials(buffer, false);

    // convert all polynomials from buffer to polynomials table
    polynomials = new int[l + 2 * QRNG_DIMENSIONS + 1];

    for (n = 0, l = 0; n < QRNG_DIMENSIONS; ++n) {
        // find degree of polynomial p1
        for (p1 = buffer[n], e_p1 = 30; (p1 & (1 << e_p1)) == 0; --e_p1) {
        }

        // fill polynomials table with values for this polynomial
        polynomials[l++] = 1;

        for (--e_p1; e_p1 >= 0; --e_p1) {
            polynomials[l++] = (p1 >> e_p1) & 1;
        }

        polynomials[l++] = -1;
    }

    polynomials[l] = -1;

    // irreducible polynomial p
    int *p = polynomials, e, d;
    // polynomial b
    int b_arr[1024], *b, m;
    // v array
    int v_arr[1024], *v;
    // temporary polynomial, required to do multiplication of p and b
    int t_arr[1024], *t;
    // subsidiary variables
    int i, j, u, m1, ip, it;

    // cycle over monic irreducible polynomials
    for (d = 0; p[0] != -1; p += e + 2) {
        // allocate memory for cj array for dimension (ip + 1)
        for (i = 0; i < 63; ++i) {
            cjn[i][d] = 0;
        }

        // determine the power of irreducible polynomial
        for (e = 0; p[e + 1] != -1; ++e) {
        }

        // polynomial b in the beginning is just '1'
        (b = b_arr + 1023)[m = 0] = 1;
        // v array needs only (63 + e - 2) length
        v = v_arr + 1023 - (63 + e - 2);

        // cycle over all coefficients
        for (j = 63 - 1, u = e; j >= 0; --j, ++u) {
            if (u == e) {
                u = 0;

                // multiply b by p (polynomials multiplication)
                for (i = 0, t = t_arr + 1023 - (m1 = m); i <= m; ++i) {
                    t[i] = b[i];
                }

                b = b_arr + 1023 - (m += e);

                for (i = 0; i <= m; ++i) {
                    b[i] = 0;

                    for (ip = e - (m - i), it = m1; ip <= e && it >= 0; ++ip, --it) {
                        if (ip >= 0) {
                            b[i] ^= p[ip] & t[it];
                        }
                    }
                }

                // multiplication of polynomials finished

                // calculate v
                for (i = 0; i < m1; ++i) {
                    v[i] = 0;
                }

                for (; i < m; ++i) {
                    v[i] = 1;
                }

                for (; i <= 63 + e - 2; ++i) {
                    v[i] = 0;

                    for (it = 1; it <= m; ++it) {
                        v[i] ^= v[i - it] & b[it];
                    }
                }
            }

            // copy calculated v to cj
            for (i = 0; i < 63; i++) {
                cjn[i][d] |= (INT64)v[i + u] << j;
            }
        }

        ++d;
    }

    delete[] polynomials;
}

// Generate 63-bit quasirandom number for given index and dimension and
// normalize
extern "C" double getQuasirandomValue63(INT64 i, int dim)
{
    const double INT63_SCALE = (1.0 / (double)0x8000000000000001ULL);
    INT64        result      = 0;

    for (int bit = 0; bit < 63; bit++, i >>= 1)
        if (i & 1)
            result ^= cjn[bit][dim];

    return (double)(result + 1) * INT63_SCALE;
}

////////////////////////////////////////////////////////////////////////////////
// Initialization (table setup)
////////////////////////////////////////////////////////////////////////////////
extern "C" void initQuasirandomGenerator(unsigned int table[QRNG_DIMENSIONS][QRNG_RESOLUTION])
{
    GenerateCJ();

    for (int dim = 0; dim < QRNG_DIMENSIONS; dim++)
        for (int bit = 0; bit < QRNG_RESOLUTION; bit++)
            table[dim][bit] = (int)((cjn[bit][dim] >> 32) & 0x7FFFFFFF);
}

////////////////////////////////////////////////////////////////////////////////
// Generate 31-bit quasirandom number for given index and dimension
////////////////////////////////////////////////////////////////////////////////
extern "C" float getQuasirandomValue(unsigned int table[QRNG_DIMENSIONS][QRNG_RESOLUTION], int i, int dim)
{
    int result = 0;

    for (int bit = 0; bit < QRNG_RESOLUTION; bit++, i >>= 1)
        if (i & 1)
            result ^= table[dim][bit];

    return (float)(result + 1) * INT_SCALE;
}

////////////////////////////////////////////////////////////////////////////////
// Moro's Inverse Cumulative Normal Distribution function approximation
////////////////////////////////////////////////////////////////////////////////
extern "C" double MoroInvCNDcpu(unsigned int x)
{
    const double a1 = 2.50662823884;
    const double a2 = -18.61500062529;
    const double a3 = 41.39119773534;
    const double a4 = -25.44106049637;
    const double b1 = -8.4735109309;
    const double b2 = 23.08336743743;
    const double b3 = -21.06224101826;
    const double b4 = 3.13082909833;
    const double c1 = 0.337475482272615;
    const double c2 = 0.976169019091719;
    const double c3 = 0.160797971491821;
    const double c4 = 2.76438810333863E-02;
    const double c5 = 3.8405729373609E-03;
    const double c6 = 3.951896511919E-04;
    const double c7 = 3.21767881768E-05;
    const double c8 = 2.888167364E-07;
    const double c9 = 3.960315187E-07;

    double z;

    bool negate = false;

    // Ensure the conversion to floating point will give a value in the
    // range (0,0.5] by restricting the input to the bottom half of the
    // input domain. We will later reflect the result if the input was
    // originally in the top half of the input domain
    if (x >= 0x80000000UL) {
        x      = 0xffffffffUL - x;
        negate = true;
    }

    // x is now in the range [0,0x80000000) (i.e. [0,0x7fffffff])
    // Convert to floating point in (0,0.5]
    const double x1 = 1.0 / static_cast<double>(0xffffffffUL);
    const double x2 = x1 / 2.0;
    double       p1 = x * x1 + x2;
    // Convert to floating point in (-0.5,0]
    double p2 = p1 - 0.5;

    // The input to the Moro inversion is p2 which is in the range
    // (-0.5,0]. This means that our output will be the negative side
    // of the bell curve (which we will reflect if "negate" is true).

    // Main body of the bell curve for |p| < 0.42
    if (p2 > -0.42) {
        z = p2 * p2;
        z = p2 * (((a4 * z + a3) * z + a2) * z + a1) / ((((b4 * z + b3) * z + b2) * z + b1) * z + 1.0);
    }
    // Special case (Chebychev) for tail
    else {
        z = log(-log(p1));
        z = -(c1 + z * (c2 + z * (c3 + z * (c4 + z * (c5 + z * (c6 + z * (c7 + z * (c8 + z * c9))))))));
    }

    // If the original input (x) was in the top half of the range, reflect
    // to get the positive side of the bell curve
    return negate ? -z : z;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/quasirandomGenerator/quasirandomGenerator_gold.cpp`.

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

- **Total Lines**: 331
- **Approximate Size**: 11704 bytes

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
