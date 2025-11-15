# Documentation for Samples/5_Domain_Specific/BlackScholes_nvrtc/BlackScholes.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/BlackScholes_nvrtc/BlackScholes.cpp`
- **Type**: .cpp
- **Location**: Samples/5_Domain_Specific/BlackScholes_nvrtc
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
 * This sample evaluates fair call and put prices for a
 * given set of European options by Black-Scholes formula.
 * See supplied whitepaper for more explanations.
 */

#include <cuda_runtime.h>
#include <helper_functions.h> // helper functions for string parsing
#include <nvrtc_helper.h>

////////////////////////////////////////////////////////////////////////////////
// Process an array of optN options on CPU
////////////////////////////////////////////////////////////////////////////////

extern "C" void BlackScholesCPU(float *h_CallResult,
                                float *h_PutResult,
                                float *h_StockPrice,
                                float *h_OptionStrike,
                                float *h_OptionYears,
                                float  Riskfree,
                                float  Volatility,
                                int    optN);

////////////////////////////////////////////////////////////////////////////////
// Process an array of OptN options on GPU
////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////

// Helper function, returning uniformly distributed
// random float in [low, high] range
////////////////////////////////////////////////////////////////////////////////

float RandFloat(float low, float high)
{
    float t = (float)rand() / (float)RAND_MAX;
    return (1.0f - t) * low + t * high;
}

////////////////////////////////////////////////////////////////////////////////
// Data configuration
////////////////////////////////////////////////////////////////////////////////

const int   OPT_N          = 4000000;
const int   NUM_ITERATIONS = 512;
const int   OPT_SZ         = OPT_N * sizeof(float);
const float RISKFREE       = 0.02f;
const float VOLATILITY     = 0.30f;

#define DIV_UP(a, b) (((a) + (b) - 1) / (b))

////////////////////////////////////////////////////////////////////////////////
// Main program
////////////////////////////////////////////////////////////////////////////////

int main(int argc, char **argv)
{
    // Start logs
    printf("[%s] - Starting...\n", argv[0]);

    //'h_' prefix - CPU (host) memory space
    float
        // Results calculated by CPU for reference
        *h_CallResultCPU,
        *h_PutResultCPU,
        // CPU copy of GPU results
        *h_CallResultGPU, *h_PutResultGPU,
        // CPU instance of input data
        *h_StockPrice, *h_OptionStrike, *h_OptionYears;

    //'d_' prefix - GPU (device) memory space
    CUdeviceptr
        // Results calculated by GPU
        d_CallResult,
        d_PutResult,

        // GPU instance of input data
        d_StockPrice, d_OptionStrike, d_OptionYears;

    double delta, ref, sum_delta, sum_ref, max_delta, L1norm, gpuTime;

    StopWatchInterface *hTimer = NULL;
    int                 i;

    sdkCreateTimer(&hTimer);

    printf("Initializing data...\n");
    printf("...allocating CPU memory for options.\n");

    h_CallResultCPU = (float *)malloc(OPT_SZ);
    h_PutResultCPU  = (float *)malloc(OPT_SZ);
    h_CallResultGPU = (float *)malloc(OPT_SZ);
    h_PutResultGPU  = (float *)malloc(OPT_SZ);
    h_StockPrice    = (float *)malloc(OPT_SZ);
    h_OptionStrike  = (float *)malloc(OPT_SZ);
    h_OptionYears   = (float *)malloc(OPT_SZ);

    char  *cubin, *kernel_file;
    size_t cubinSize;
    kernel_file = sdkFindFilePath("BlackScholes_kernel.cuh", argv[0]);

    // Compile the kernel BlackScholes_kernel.
    compileFileToCUBIN(kernel_file, argc, argv, &cubin, &cubinSize, 0);
    CUmodule module = loadCUBIN(cubin, argc, argv);

    CUfunction kernel_addr;
    checkCudaErrors(cuModuleGetFunction(&kernel_addr, module, "BlackScholesGPU"));

    printf("...allocating GPU memory for options.\n");
    checkCudaErrors(cuMemAlloc(&d_CallResult, OPT_SZ));
    checkCudaErrors(cuMemAlloc(&d_PutResult, OPT_SZ));
    checkCudaErrors(cuMemAlloc(&d_StockPrice, OPT_SZ));
    checkCudaErrors(cuMemAlloc(&d_OptionStrike, OPT_SZ));
    checkCudaErrors(cuMemAlloc(&d_OptionYears, OPT_SZ));

    printf("...generating input data in CPU mem.\n");
    srand(5347);

    // Generate options set
    for (i = 0; i < OPT_N; i++) {
        h_CallResultCPU[i] = 0.0f;
        h_PutResultCPU[i]  = -1.0f;
        h_StockPrice[i]    = RandFloat(5.0f, 30.0f);
        h_OptionStrike[i]  = RandFloat(1.0f, 100.0f);
        h_OptionYears[i]   = RandFloat(0.25f, 10.0f);
    }

    printf("...copying input data to GPU mem.\n");
    // Copy options data to GPU memory for further processing
    checkCudaErrors(cuMemcpyHtoD(d_StockPrice, h_StockPrice, OPT_SZ));
    checkCudaErrors(cuMemcpyHtoD(d_OptionStrike, h_OptionStrike, OPT_SZ));
    checkCudaErrors(cuMemcpyHtoD(d_OptionYears, h_OptionYears, OPT_SZ));

    printf("Data init done.\n\n");
    printf("Executing Black-Scholes GPU kernel (%i iterations)...\n", NUM_ITERATIONS);

    sdkResetTimer(&hTimer);
    sdkStartTimer(&hTimer);

    dim3 cudaBlockSize(128, 1, 1);
    dim3 cudaGridSize(DIV_UP(OPT_N / 2, 128), 1, 1);

    float risk       = RISKFREE;
    float volatility = VOLATILITY;
    int   optval     = OPT_N;

    void *arr[] = {(void *)&d_CallResult,
                   (void *)&d_PutResult,
                   (void *)&d_StockPrice,
                   (void *)&d_OptionStrike,
                   (void *)&d_OptionYears,
                   (void *)&risk,
                   (void *)&volatility,
                   (void *)&optval};

    for (i = 0; i < NUM_ITERATIONS; i++) {
        checkCudaErrors(cuLaunchKernel(kernel_addr,
                                       cudaGridSize.x,
                                       cudaGridSize.y,
                                       cudaGridSize.z, /* grid dim */
                                       cudaBlockSize.x,
                                       cudaBlockSize.y,
                                       cudaBlockSize.z, /* block dim */
                                       0,
                                       0,       /* shared mem, stream */
                                       &arr[0], /* arguments */
                                       0));
    }

    checkCudaErrors(cuCtxSynchronize());

    sdkStopTimer(&hTimer);
    gpuTime = sdkGetTimerValue(&hTimer) / NUM_ITERATIONS;

    // Both call and put is calculated
    printf("Options count             : %i     \n", 2 * OPT_N);
    printf("BlackScholesGPU() time    : %f msec\n", gpuTime);
    printf("Effective memory bandwidth: %f GB/s\n", ((double)(5 * OPT_N * sizeof(float)) * 1E-9) / (gpuTime * 1E-3));
    printf("Gigaoptions per second    : %f     \n\n", ((double)(2 * OPT_N) * 1E-9) / (gpuTime * 1E-3));
    printf("BlackScholes, Throughput = %.4f GOptions/s, Time = %.5f s, Size = %u "
           "options, NumDevsUsed = %u, Workgroup = %u\n",
           (((double)(2.0 * OPT_N) * 1.0E-9) / (gpuTime * 1.0E-3)),
           gpuTime * 1e-3,
           (2 * OPT_N),
           1,
           128);

    printf("\nReading back GPU results...\n");

    // Read back GPU results to compare them to CPU results
    checkCudaErrors(cuMemcpyDtoH(h_CallResultGPU, d_CallResult, OPT_SZ));
    checkCudaErrors(cuMemcpyDtoH(h_PutResultGPU, d_PutResult, OPT_SZ));

    printf("Checking the results...\n");
    printf("...running CPU calculations.\n\n");

    // Calculate options values on CPU
    BlackScholesCPU(
        h_CallResultCPU, h_PutResultCPU, h_StockPrice, h_OptionStrike, h_OptionYears, RISKFREE, VOLATILITY, OPT_N);

    printf("Comparing the results...\n");
    // Calculate max absolute difference and L1 distance
    // between CPU and GPU results
    sum_delta = 0;
    sum_ref   = 0;
    max_delta = 0;

    for (i = 0; i < OPT_N; i++) {
        ref   = h_CallResultCPU[i];
        delta = fabs(h_CallResultCPU[i] - h_CallResultGPU[i]);

        if (delta > max_delta) {
            max_delta = delta;
        }

        sum_delta += delta;
        sum_ref += fabs(ref);
    }

    L1norm = sum_delta / sum_ref;
    printf("L1 norm: %E\n", L1norm);
    printf("Max absolute error: %E\n\n", max_delta);

    printf("Shutting down...\n");
    printf("...releasing GPU memory.\n");

    checkCudaErrors(cuMemFree(d_OptionYears));
    checkCudaErrors(cuMemFree(d_OptionStrike));
    checkCudaErrors(cuMemFree(d_StockPrice));
    checkCudaErrors(cuMemFree(d_PutResult));
    checkCudaErrors(cuMemFree(d_CallResult));

    printf("...releasing CPU memory.\n");

    free(h_OptionYears);
    free(h_OptionStrike);
    free(h_StockPrice);
    free(h_PutResultGPU);
    free(h_CallResultGPU);
    free(h_PutResultCPU);
    free(h_CallResultCPU);

    sdkDeleteTimer(&hTimer);
    printf("Shutdown done.\n");

    printf("\n[%s] - Test Summary\n", argv[0]);

    if (L1norm > 1e-6) {
        printf("Test failed!\n");
        exit(EXIT_FAILURE);
    }

    printf("Test passed\n");
    exit(EXIT_SUCCESS);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/BlackScholes_nvrtc/BlackScholes.cpp`.

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

- **Total Lines**: 282
- **Approximate Size**: 10479 bytes

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
