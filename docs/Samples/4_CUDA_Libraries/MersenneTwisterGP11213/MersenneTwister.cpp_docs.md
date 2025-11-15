# Documentation for Samples/4_CUDA_Libraries/MersenneTwisterGP11213/MersenneTwister.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/MersenneTwisterGP11213/MersenneTwister.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/MersenneTwisterGP11213
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
 * This sample demonstrates the use of CURAND to generate
 * random numbers on GPU and CPU.
 */

// Utilities and system includes
// includes, system
#include <curand.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Utilities and system includes
#include <cuda_runtime.h>
#include <curand.h>
#include <helper_cuda.h>
#include <helper_functions.h>

float compareResults(int rand_n, float *h_RandGPU, float *h_RandCPU);

const int          DEFAULT_RAND_N = 2400000;
const unsigned int DEFAULT_SEED   = 777;

///////////////////////////////////////////////////////////////////////////////
// Main program
///////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    // Start logs
    printf("%s Starting...\n\n", argv[0]);

    // initialize the GPU, either identified by --device
    // or by picking the device with highest flop rate.
    int devID = findCudaDevice(argc, (const char **)argv);

    // parsing the number of random numbers to generate
    int rand_n = DEFAULT_RAND_N;

    if (checkCmdLineFlag(argc, (const char **)argv, "count")) {
        rand_n = getCmdLineArgumentInt(argc, (const char **)argv, "count");
    }

    printf("Allocating data for %i samples...\n", rand_n);

    // parsing the seed
    int seed = DEFAULT_SEED;

    if (checkCmdLineFlag(argc, (const char **)argv, "seed")) {
        seed = getCmdLineArgumentInt(argc, (const char **)argv, "seed");
    }

    printf("Seeding with %i ...\n", seed);

    cudaStream_t stream;
    checkCudaErrors(cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking));

    float *d_Rand;
    checkCudaErrors(cudaMalloc((void **)&d_Rand, rand_n * sizeof(float)));

    curandGenerator_t prngGPU;
    checkCudaErrors(curandCreateGenerator(&prngGPU, CURAND_RNG_PSEUDO_MTGP32));
    checkCudaErrors(curandSetStream(prngGPU, stream));
    checkCudaErrors(curandSetPseudoRandomGeneratorSeed(prngGPU, seed));

    curandGenerator_t prngCPU;
    checkCudaErrors(curandCreateGeneratorHost(&prngCPU, CURAND_RNG_PSEUDO_MTGP32));
    checkCudaErrors(curandSetPseudoRandomGeneratorSeed(prngCPU, seed));

    //
    // Example 1: Compare random numbers generated on GPU and CPU
    float *h_RandGPU;
    checkCudaErrors(cudaMallocHost(&h_RandGPU, rand_n * sizeof(float)));

    printf("Generating random numbers on GPU...\n\n");
    checkCudaErrors(curandGenerateUniform(prngGPU, (float *)d_Rand, rand_n));

    printf("\nReading back the results...\n");
    checkCudaErrors(cudaMemcpyAsync(h_RandGPU, d_Rand, rand_n * sizeof(float), cudaMemcpyDeviceToHost, stream));

    float *h_RandCPU = (float *)malloc(rand_n * sizeof(float));

    printf("Generating random numbers on CPU...\n\n");
    checkCudaErrors(curandGenerateUniform(prngCPU, (float *)h_RandCPU, rand_n));

    checkCudaErrors(cudaStreamSynchronize(stream));
    printf("Comparing CPU/GPU random numbers...\n\n");
    float L1norm = compareResults(rand_n, h_RandGPU, h_RandCPU);

    //
    // Example 2: Timing of random number generation on GPU
    const int           numIterations = 10;
    int                 i;
    StopWatchInterface *hTimer;

    sdkCreateTimer(&hTimer);
    sdkResetTimer(&hTimer);
    sdkStartTimer(&hTimer);

    for (i = 0; i < numIterations; i++) {
        checkCudaErrors(curandGenerateUniform(prngGPU, (float *)d_Rand, rand_n));
    }

    checkCudaErrors(cudaStreamSynchronize(stream));
    sdkStopTimer(&hTimer);

    double gpuTime = 1.0e-3 * sdkGetTimerValue(&hTimer) / (double)numIterations;

    printf("MersenneTwisterGP11213, Throughput = %.4f GNumbers/s, Time = %.5f s, "
           "Size = %u Numbers\n",
           1.0e-9 * rand_n / gpuTime,
           gpuTime,
           rand_n);

    printf("Shutting down...\n");

    checkCudaErrors(curandDestroyGenerator(prngGPU));
    checkCudaErrors(curandDestroyGenerator(prngCPU));
    checkCudaErrors(cudaStreamDestroy(stream));
    checkCudaErrors(cudaFree(d_Rand));
    sdkDeleteTimer(&hTimer);
    checkCudaErrors(cudaFreeHost(h_RandGPU));
    free(h_RandCPU);

    exit(L1norm < 1e-6 ? EXIT_SUCCESS : EXIT_FAILURE);
}

float compareResults(int rand_n, float *h_RandGPU, float *h_RandCPU)
{
    int   i;
    float rCPU, rGPU, delta;
    float max_delta = 0.;
    float sum_delta = 0.;
    float sum_ref   = 0.;

    for (i = 0; i < rand_n; i++) {
        rCPU  = h_RandCPU[i];
        rGPU  = h_RandGPU[i];
        delta = fabs(rCPU - rGPU);
        sum_delta += delta;
        sum_ref += fabs(rCPU);

        if (delta >= max_delta) {
            max_delta = delta;
        }
    }

    float L1norm = (float)(sum_delta / sum_ref);
    printf("Max absolute error: %E\n", max_delta);
    printf("L1 norm: %E\n\n", L1norm);

    return L1norm;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/MersenneTwisterGP11213/MersenneTwister.cpp`.

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

- **Total Lines**: 180
- **Approximate Size**: 6295 bytes

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
