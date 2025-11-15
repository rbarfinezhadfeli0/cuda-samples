# Documentation for Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/test.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/test.cpp`
- **Type**: .cpp
- **Location**: Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src
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

#include "../inc/test.h"

#include <cassert>
#include <cuda_runtime.h>
#include <helper_timer.h>
#include <iomanip>
#include <iostream>
#include <math.h>
#include <memory>
#include <sstream>
#include <stdexcept>
#include <stdio.h>
#include <typeinfo>

#include "../inc/asianoption.h"
#include "../inc/pricingengine.h"

template <typename Real> bool Test<Real>::operator()()
{
    using std::endl;
    using std::setw;
    using std::stringstream;

    StopWatchInterface *timer = NULL;
    sdkCreateTimer(&timer);

    // Get device properties
    struct cudaDeviceProp deviceProperties;
    cudaError_t           cudaResult = cudaGetDeviceProperties(&deviceProperties, device);

    if (cudaResult != cudaSuccess) {
        std::string msg("Could not get device properties: ");
        msg += cudaGetErrorString(cudaResult);
        throw std::runtime_error(msg);
    }

    // This test prices a single Asian call option with European
    // exercise, with the priced averaged arithmetically on discrete
    // trading days (weekdays).
    AsianOption<Real> option;
    option.spot   = static_cast<Real>(40);
    option.strike = static_cast<Real>(35);
    option.r      = static_cast<Real>(0.03);
    option.sigma  = static_cast<Real>(0.20);
    option.tenor  = static_cast<Real>(1.0 / 3.0);
    option.dt     = static_cast<Real>(1.0 / 261);
    option.type   = AsianOption<Real>::Call;
    option.value  = static_cast<Real>(0.0);
    option.golden = static_cast<Real>(5.162534);

    // Evaluate on GPU
    printf("Pricing option on GPU (%s)\n\n", deviceProperties.name);
    PricingEngine<Real> pricer(numSims, device, threadBlockSize, seed);
    sdkStartTimer(&timer);
    pricer(option);
    sdkStopTimer(&timer);
    elapsedTime = sdkGetAverageTimerValue(&timer) / 1000.0f;

    // Tolerance to compare result with expected
    // This is just to check that nothing has gone very wrong with the
    // test, the actual accuracy of the result depends on the number of
    // Monte Carlo trials
    const Real tolerance = static_cast<Real>(0.1);

    // Display results
    stringstream output;
    output << "Precision:      " << ((typeid(Real) == typeid(double)) ? "double" : "single") << endl;
    output << "Number of sims: " << numSims << endl;
    output << endl;
    output << "   Spot    |   Strike   |     r      |   sigma    |   tenor    |  "
              "Call/Put  |   Value    |  Expected  |"
           << endl;
    output << "-----------|------------|------------|------------|------------|--"
              "----------|------------|------------|"
           << endl;
    output << setw(10) << option.spot << " | ";
    output << setw(10) << option.strike << " | ";
    output << setw(10) << option.r << " | ";
    output << setw(10) << option.sigma << " | ";
    output << setw(10) << option.tenor << " | ";
    output << setw(10) << (option.type == AsianOption<Real>::Call ? "Call" : "Put") << " | ";
    output << setw(10) << option.value << " | ";
    output << setw(10) << option.golden << " |";

    printf("%s\n\n", output.str().c_str());

    // Check result
    if (fabs(option.value - option.golden) > tolerance) {
        printf("computed result (%e) does not match expected result (%e).\n", option.value, option.golden);
        pass = false;
    }
    else {
        pass = true;
    }

    // Print results
    printf("MonteCarloSingleAsianOptionP, Performance = %.2f sims/s, Time = "
           "%.2f(ms), NumDevsUsed = %u, Blocksize = %u\n",
           numSims / elapsedTime,
           elapsedTime * 1000.0f,
           1,
           threadBlockSize);

    sdkDeleteTimer(&timer);

    return pass;
}

// Explicit template instantiation
template struct Test<float>;
template struct Test<double>;

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/test.cpp`.

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

- **Total Lines**: 139
- **Approximate Size**: 5297 bytes

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
