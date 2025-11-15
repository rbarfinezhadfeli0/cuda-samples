# Documentation for Samples/4_CUDA_Libraries/randomFog/rng.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/randomFog/rng.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/randomFog
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

// Utilities and System includes

// Includes
#include "rng.h"

#include <curand.h>
#include <sstream>
#include <stdexcept>

// Shared Library Test Functions
#include <helper_cuda.h>
#include <helper_timer.h>

const unsigned int RNG::s_maxQrngDimensions = 20000;

RNG::RNG(unsigned long prngSeed, unsigned int qrngDimensions, unsigned int nSamples)
    : m_prngSeed(prngSeed)
    , m_qrngDimensions(qrngDimensions)
    , m_nSamplesBatchTarget(nSamples)
    , m_nSamplesRemaining(0)
{
    using std::invalid_argument;
    using std::runtime_error;
    using std::string;

    if (m_prngSeed == 0) {
        throw invalid_argument("PRNG seed must be non-zero");
    }

    if (m_qrngDimensions == 0) {
        throw invalid_argument("QRNG dimensions must be non-zero");
    }

    if (m_nSamplesBatchTarget == 0) {
        throw invalid_argument("RNG batch size must be non-zero");
    }

    if (m_nSamplesBatchTarget < s_maxQrngDimensions) {
        throw invalid_argument("RNG batch size must be greater than RNG::s_maxQrngDimensions");
    }

    curandStatus_t curandResult;
    cudaError_t    cudaResult;

    // Allocate sample array in host mem
    m_h_samples = (float *)malloc(m_nSamplesBatchTarget * sizeof(float));

    if (m_h_samples == NULL) {
        throw runtime_error("Could not allocate host memory for RNG::m_h_samples");
    }

    // Allocate sample array in device mem
    cudaResult = cudaMalloc((void **)&m_d_samples, m_nSamplesBatchTarget * sizeof(float));

    if (cudaResult != cudaSuccess) {
        string msg("Could not allocate device memory for RNG::m_d_samples: ");
        msg += cudaGetErrorString(cudaResult);
        throw runtime_error(msg);
    }

    // Create the Random Number Generators
    curandResult = curandCreateGenerator(&m_prng, CURAND_RNG_PSEUDO_XORWOW);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        string msg("Could not create pseudo-random number generator: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    curandResult = curandCreateGenerator(&m_qrng, CURAND_RNG_QUASI_SOBOL32);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        string msg("Could not create quasi-random number generator: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    curandResult = curandCreateGenerator(&m_sqrng, CURAND_RNG_QUASI_SCRAMBLED_SOBOL32);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        string msg("Could not create scrambled quasi-random number generator: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    // Setup initial parameters
    resetSeed();
    updateDimensions();
    setBatchSize();

    // Set default RNG to be pseudo-random (XORWOW)
    m_pCurrent = &m_prng;
}

RNG::~RNG()
{
    curandDestroyGenerator(m_prng);
    curandDestroyGenerator(m_qrng);
    curandDestroyGenerator(m_sqrng);

    if (m_d_samples) {
        cudaFree(m_d_samples);
    }

    if (m_h_samples) {
        free(m_h_samples);
    }
}

void RNG::generateBatch(void)
{
    using std::runtime_error;
    using std::string;

    cudaError_t    cudaResult;
    curandStatus_t curandResult;

    // Generate random numbers
    curandResult = curandGenerateUniform(*m_pCurrent, m_d_samples, m_nSamplesBatchActual);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        string msg("Could not generate random numbers: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    // Copy random numbers to host
    cudaResult = cudaMemcpy(m_h_samples, m_d_samples, m_nSamplesBatchActual * sizeof(float), cudaMemcpyDeviceToHost);

    if (cudaResult != cudaSuccess) {
        string msg("Could not copy random numbers to host: ");
        msg += cudaGetErrorString(cudaResult);
        throw runtime_error(msg);
    }
}

float RNG::getNextU01(void)
{
    if (m_nSamplesRemaining == 0) {
        generateBatch();
        m_nSamplesRemaining = m_nSamplesBatchActual;
    }

    if (m_pCurrent == &m_prng) {
        return m_h_samples[m_nSamplesBatchActual - m_nSamplesRemaining--];
    }
    else {
        unsigned int index         = m_nSamplesBatchActual - m_nSamplesRemaining--;
        unsigned int samplesPerDim = m_nSamplesBatchActual / m_qrngDimensions;
        unsigned int dimOffset     = (index % m_qrngDimensions) * samplesPerDim;
        unsigned int drawOffset    = index / m_qrngDimensions;
        return m_h_samples[dimOffset + drawOffset];
    }
}

void RNG::getInfoString(std::string &msg)
{
    using std::stringstream;

    stringstream ss;

    if (m_pCurrent == &m_prng) {
        ss << "XORWOW (seed=" << m_prngSeed << ")";
    }
    else if (m_pCurrent == &m_qrng) {
        ss << "Sobol (dimensions=" << m_qrngDimensions << ")";
    }
    else if (m_pCurrent == &m_sqrng) {
        ss << "Scrambled Sobol (dimensions=" << m_qrngDimensions << ")";
    }
    else {
        ss << "Invalid RNG";
    }

    msg.assign(ss.str());
}

void RNG::selectRng(RNG::RngType type)
{
    switch (type) {
    case Quasi:
        m_pCurrent = &m_qrng;
        break;

    case ScrambledQuasi:
        m_pCurrent = &m_sqrng;
        break;

    case Pseudo:
    default:
        m_pCurrent = &m_prng;
        break;
    }

    setBatchSize();
}

void RNG::resetSeed(void)
{
    using std::runtime_error;

    curandStatus_t curandResult;
    curandResult = curandSetPseudoRandomGeneratorSeed(m_prng, m_prngSeed);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        std::string msg("Could not set pseudo-random number generator seed: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    curandResult = curandSetGeneratorOffset(m_prng, 0);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        std::string msg("Could not set pseudo-random number generator offset: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    setBatchSize();
}

void RNG::resetDimensions(void)
{
    m_qrngDimensions = 1;
    updateDimensions();
    setBatchSize();
}

void RNG::incrementDimensions(void)
{
    if (++m_qrngDimensions > s_maxQrngDimensions) {
        m_qrngDimensions = 1;
    }

    updateDimensions();
    setBatchSize();
}

void RNG::updateDimensions(void)
{
    using std::runtime_error;

    curandStatus_t curandResult;
    curandResult = curandSetQuasiRandomGeneratorDimensions(m_qrng, m_qrngDimensions);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        std::string msg("Could not set quasi-random number generator dimensions: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    curandResult = curandSetGeneratorOffset(m_qrng, 0);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        std::string msg("Could not set quasi-random number generator offset: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    curandResult = curandSetQuasiRandomGeneratorDimensions(m_sqrng, m_qrngDimensions);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        std::string msg("Could not set scrambled quasi-random number generator dimensions: ");
        msg += curandResult;
        throw runtime_error(msg);
    }

    curandResult = curandSetGeneratorOffset(m_sqrng, 0);

    if (curandResult != CURAND_STATUS_SUCCESS) {
        std::string msg("Could not set scrambled quasi-random number generator offset: ");
        msg += curandResult;
        throw runtime_error(msg);
    }
}

void RNG::setBatchSize(void)
{
    if (m_pCurrent == &m_prng) {
        m_nSamplesBatchActual = m_nSamplesBatchTarget;
    }
    else {
        m_nSamplesBatchActual = (m_nSamplesBatchTarget / m_qrngDimensions) * m_qrngDimensions;
    }

    m_nSamplesRemaining = 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/randomFog/rng.cpp`.

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

- **Total Lines**: 315
- **Approximate Size**: 9186 bytes

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
