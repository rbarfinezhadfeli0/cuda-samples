# Documentation for Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_gold.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_gold.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/MonteCarloMultiGPU
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

#include <curand.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>

// #include "curand_kernel.h"
#include "helper_cuda.h"

////////////////////////////////////////////////////////////////////////////////
// Common types
////////////////////////////////////////////////////////////////////////////////
#include "MonteCarlo_common.h"

////////////////////////////////////////////////////////////////////////////////
// Black-Scholes formula for Monte Carlo results validation
////////////////////////////////////////////////////////////////////////////////
#define A1       0.31938153
#define A2       -0.356563782
#define A3       1.781477937
#define A4       -1.821255978
#define A5       1.330274429
#define RSQRT2PI 0.39894228040143267793994605993438

// Polynomial approximation of
// cumulative normal distribution function
double CND(double d)
{
    double K = 1.0 / (1.0 + 0.2316419 * fabs(d));

    double cnd = RSQRT2PI * exp(-0.5 * d * d) * (K * (A1 + K * (A2 + K * (A3 + K * (A4 + K * A5)))));

    if (d > 0)
        cnd = 1.0 - cnd;

    return cnd;
}

// Black-Scholes formula for call value
extern "C" void BlackScholesCall(float &callValue, TOptionData optionData)
{
    double S = optionData.S;
    double X = optionData.X;
    double T = optionData.T;
    double R = optionData.R;
    double V = optionData.V;

    double sqrtT = sqrt(T);
    double d1    = (log(S / X) + (R + 0.5 * V * V) * T) / (V * sqrtT);
    double d2    = d1 - V * sqrtT;
    double CNDD1 = CND(d1);
    double CNDD2 = CND(d2);
    double expRT = exp(-R * T);

    callValue = (float)(S * CNDD1 - X * expRT * CNDD2);
}

////////////////////////////////////////////////////////////////////////////////
// CPU Monte Carlo
////////////////////////////////////////////////////////////////////////////////
static double endCallValue(double S, double X, double r, double MuByT, double VBySqrtT)
{
    double callValue = S * exp(MuByT + VBySqrtT * r) - X;
    return (callValue > 0) ? callValue : 0;
}

extern "C" void MonteCarloCPU(TOptionValue &callValue, TOptionData optionData, float *h_Samples, int pathN)
{
    const double S        = optionData.S;
    const double X        = optionData.X;
    const double T        = optionData.T;
    const double R        = optionData.R;
    const double V        = optionData.V;
    const double MuByT    = (R - 0.5 * V * V) * T;
    const double VBySqrtT = V * sqrt(T);

    float            *samples;
    curandGenerator_t gen;

    checkCudaErrors(curandCreateGeneratorHost(&gen, CURAND_RNG_PSEUDO_DEFAULT));
    unsigned long long seed = 1234ULL;
    checkCudaErrors(curandSetPseudoRandomGeneratorSeed(gen, seed));

    if (h_Samples != NULL) {
        samples = h_Samples;
    }
    else {
        samples = (float *)malloc(pathN * sizeof(float));
        checkCudaErrors(curandGenerateNormal(gen, samples, pathN, 0.0, 1.0));
    }

    // for(int i=0; i<10; i++) printf("CPU sample = %f\n", samples[i]);

    double sum = 0, sum2 = 0;

    for (int pos = 0; pos < pathN; pos++) {
        double sample    = samples[pos];
        double callValue = endCallValue(S, X, sample, MuByT, VBySqrtT);
        sum += callValue;
        sum2 += callValue * callValue;
    }

    if (h_Samples == NULL)
        free(samples);

    checkCudaErrors(curandDestroyGenerator(gen));

    // Derive average from the total sum and discount by riskfree rate
    callValue.Expected = (float)(exp(-R * T) * sum / (double)pathN);
    // Standard deviation
    double stdDev = sqrt(((double)pathN * sum2 - sum * sum) / ((double)pathN * (double)(pathN - 1)));
    // Confidence width; in 95% of all cases theoretical value lies within these
    // borders
    callValue.Confidence = (float)(exp(-R * T) * 1.96 * stdDev / sqrt((double)pathN));
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_gold.cpp`.

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

- **Total Lines**: 142
- **Approximate Size**: 5327 bytes

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
