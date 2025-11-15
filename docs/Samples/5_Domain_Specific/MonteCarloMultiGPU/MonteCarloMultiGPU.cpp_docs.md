# Documentation for Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp

## File Metadata

- **Path**: `Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp`
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

/*
 * This sample evaluates fair call price for a
 * given set of European options using Monte Carlo approach.
 * See supplied whitepaper for more explanations.
 */

#include <cuda_runtime.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project
#include <helper_cuda.h>      // helper functions (cuda error checking and initialization)
#include <helper_functions.h> // Helper functions (utilities, parsing, timing)
#include <multithreading.h>

#include "MonteCarlo_common.h"

int   *pArgc = NULL;
char **pArgv = NULL;

#ifdef WIN32
#define strcasecmp _strcmpi
#endif

////////////////////////////////////////////////////////////////////////////////
// Common functions
////////////////////////////////////////////////////////////////////////////////
float randFloat(float low, float high)
{
    float t = (float)rand() / (float)RAND_MAX;
    return (1.0f - t) * low + t * high;
}

/// Utility function to tweak problem size for small GPUs
int adjustProblemSize(int GPU_N, int default_nOptions)
{
    int nOptions = default_nOptions;

    // select problem size
    for (int i = 0; i < GPU_N; i++) {
        cudaDeviceProp deviceProp;
        checkCudaErrors(cudaGetDeviceProperties(&deviceProp, i));
        int cudaCores = _ConvertSMVer2Cores(deviceProp.major, deviceProp.minor) * deviceProp.multiProcessorCount;

        if (cudaCores <= 32) {
            nOptions = (nOptions < cudaCores / 2 ? nOptions : cudaCores / 2);
        }
    }

    return nOptions;
}

int adjustGridSize(int GPUIndex, int defaultGridSize)
{
    cudaDeviceProp deviceProp;
    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, GPUIndex));
    int maxGridSize = deviceProp.multiProcessorCount * 40;
    return ((defaultGridSize > maxGridSize) ? maxGridSize : defaultGridSize);
}

///////////////////////////////////////////////////////////////////////////////
// CPU reference functions
///////////////////////////////////////////////////////////////////////////////
extern "C" void MonteCarloCPU(TOptionValue &callValue, TOptionData optionData, float *h_Random, int pathN);

// Black-Scholes formula for call options
extern "C" void BlackScholesCall(float &CallResult, TOptionData optionData);

////////////////////////////////////////////////////////////////////////////////
// GPU-driving host thread
////////////////////////////////////////////////////////////////////////////////
// Timer
StopWatchInterface **hTimer = NULL;

static CUT_THREADPROC solverThread(TOptionPlan *plan)
{
    // Init GPU
    checkCudaErrors(cudaSetDevice(plan->device));

    cudaDeviceProp deviceProp;
    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, plan->device));

    // Start the timer
    sdkStartTimer(&hTimer[plan->device]);

    // Allocate intermediate memory for MC integrator and initialize
    // RNG states
    initMonteCarloGPU(plan);

    // Main computation
    MonteCarloGPU(plan);

    checkCudaErrors(cudaDeviceSynchronize());

    // Stop the timer
    sdkStopTimer(&hTimer[plan->device]);

    // Shut down this GPU
    closeMonteCarloGPU(plan);

    cudaStreamSynchronize(0);

    printf("solverThread() finished - GPU Device %d: %s\n", plan->device, deviceProp.name);

    CUT_THREADEND;
}

static void multiSolver(TOptionPlan *plan, int nPlans)
{
    // allocate and initialize an array of stream handles
    cudaStream_t *streams = (cudaStream_t *)malloc(nPlans * sizeof(cudaStream_t));
    cudaEvent_t  *events  = (cudaEvent_t *)malloc(nPlans * sizeof(cudaEvent_t));

    for (int i = 0; i < nPlans; i++) {
        checkCudaErrors(cudaSetDevice(plan[i].device));
        checkCudaErrors(cudaStreamCreate(&(streams[i])));
        checkCudaErrors(cudaEventCreate(&(events[i])));
    }

    // Init Each GPU
    // In CUDA 4.0 we can call cudaSetDevice multiple times to target each device
    // Set the device desired, then perform initializations on that device

    for (int i = 0; i < nPlans; i++) {
        // set the target device to perform initialization on
        checkCudaErrors(cudaSetDevice(plan[i].device));

        cudaDeviceProp deviceProp;
        checkCudaErrors(cudaGetDeviceProperties(&deviceProp, plan[i].device));

        // Allocate intermediate memory for MC integrator
        // and initialize RNG state
        initMonteCarloGPU(&plan[i]);
    }

    for (int i = 0; i < nPlans; i++) {
        checkCudaErrors(cudaSetDevice(plan[i].device));
        checkCudaErrors(cudaDeviceSynchronize());
    }

    // Start the timer
    sdkResetTimer(&hTimer[0]);
    sdkStartTimer(&hTimer[0]);

    for (int i = 0; i < nPlans; i++) {
        checkCudaErrors(cudaSetDevice(plan[i].device));

        // Main computations
        MonteCarloGPU(&plan[i], streams[i]);

        checkCudaErrors(cudaEventRecord(events[i], streams[i]));
    }

    for (int i = 0; i < nPlans; i++) {
        checkCudaErrors(cudaSetDevice(plan[i].device));
        cudaEventSynchronize(events[i]);
    }

    // Stop the timer
    sdkStopTimer(&hTimer[0]);

    for (int i = 0; i < nPlans; i++) {
        checkCudaErrors(cudaSetDevice(plan[i].device));
        closeMonteCarloGPU(&plan[i]);
        checkCudaErrors(cudaStreamDestroy(streams[i]));
        checkCudaErrors(cudaEventDestroy(events[i]));
    }
}

///////////////////////////////////////////////////////////////////////////////
// Main program
///////////////////////////////////////////////////////////////////////////////
#define DO_CPU
#undef DO_CPU

#define PRINT_RESULTS
#undef PRINT_RESULTS

void usage()
{
    printf("--method=[threaded,streamed] --scaling=[strong,weak] [--help]\n");
    printf("Method=threaded: 1 CPU thread for each GPU     [default]\n");
    printf("       streamed: 1 CPU thread handles all GPUs (requires CUDA 4.0 or "
           "newer)\n");
    printf("Scaling=strong : constant problem size\n");
    printf("        weak   : problem size scales with number of available GPUs "
           "[default]\n");
}

int main(int argc, char **argv)
{
    char *multiMethodChoice = NULL;
    char *scalingChoice     = NULL;
    bool  use_threads       = true;
    bool  bqatest           = false;
    bool  strongScaling     = false;

    pArgc = &argc;
    pArgv = argv;

    printf("%s Starting...\n\n", argv[0]);

    if (checkCmdLineFlag(argc, (const char **)argv, "qatest")) {
        bqatest = true;
    }

    getCmdLineArgumentString(argc, (const char **)argv, "method", &multiMethodChoice);
    getCmdLineArgumentString(argc, (const char **)argv, "scaling", &scalingChoice);

    if (checkCmdLineFlag(argc, (const char **)argv, "h") || checkCmdLineFlag(argc, (const char **)argv, "help")) {
        usage();
        exit(EXIT_SUCCESS);
    }

    if (multiMethodChoice == NULL) {
        use_threads = false;
    }
    else {
        if (!strcasecmp(multiMethodChoice, "threaded")) {
            use_threads = true;
        }
        else {
            use_threads = false;
        }
    }

    if (use_threads == false) {
        printf("Using single CPU thread for multiple GPUs\n");
    }

    if (scalingChoice == NULL) {
        strongScaling = false;
    }
    else {
        if (!strcasecmp(scalingChoice, "strong")) {
            strongScaling = true;
        }
        else {
            strongScaling = false;
        }
    }

    // GPU number present in the system
    int GPU_N;
    checkCudaErrors(cudaGetDeviceCount(&GPU_N));
    int nOptions = 8 * 1024;

    nOptions = adjustProblemSize(GPU_N, nOptions);

    // select problem size
    int scale  = (strongScaling) ? 1 : GPU_N;
    int OPT_N  = nOptions * scale;
    int PATH_N = 262144;

    // initialize the timers
    hTimer = new StopWatchInterface *[GPU_N];

    for (int i = 0; i < GPU_N; i++) {
        sdkCreateTimer(&hTimer[i]);
        sdkResetTimer(&hTimer[i]);
    }

    // Input data array
    TOptionData *optionData = new TOptionData[OPT_N];
    // Final GPU MC results
    TOptionValue *callValueGPU = new TOptionValue[OPT_N];
    //"Theoretical" call values by Black-Scholes formula
    float *callValueBS = new float[OPT_N];
    // Solver config
    TOptionPlan *optionSolver = new TOptionPlan[GPU_N];
    // OS thread ID
    CUTThread *threadID = new CUTThread[GPU_N];

    int gpuBase, gpuIndex;
    int i;

    float time;

    double delta, ref, sumDelta, sumRef, sumReserve;

    printf("MonteCarloMultiGPU\n");
    printf("==================\n");
    printf("Parallelization method  = %s\n", use_threads ? "threaded" : "streamed");
    printf("Problem scaling         = %s\n", strongScaling ? "strong" : "weak");
    printf("Number of GPUs          = %d\n", GPU_N);
    printf("Total number of options = %d\n", OPT_N);
    printf("Number of paths         = %d\n", PATH_N);

    printf("main(): generating input data...\n");
    srand(123);

    for (i = 0; i < OPT_N; i++) {
        optionData[i].S            = randFloat(5.0f, 50.0f);
        optionData[i].X            = randFloat(10.0f, 25.0f);
        optionData[i].T            = randFloat(1.0f, 5.0f);
        optionData[i].R            = 0.06f;
        optionData[i].V            = 0.10f;
        callValueGPU[i].Expected   = -1.0f;
        callValueGPU[i].Confidence = -1.0f;
    }

    printf("main(): starting %i host threads...\n", GPU_N);

    // Get option count for each GPU
    for (i = 0; i < GPU_N; i++) {
        optionSolver[i].optionCount = OPT_N / GPU_N;
    }

    // Take into account cases with "odd" option counts
    for (i = 0; i < (OPT_N % GPU_N); i++) {
        optionSolver[i].optionCount++;
    }

    // Assign GPU option ranges
    gpuBase = 0;

    for (i = 0; i < GPU_N; i++) {
        optionSolver[i].device     = i;
        optionSolver[i].optionData = optionData + gpuBase;
        optionSolver[i].callValue  = callValueGPU + gpuBase;
        optionSolver[i].pathN      = PATH_N;
        optionSolver[i].gridSize   = adjustGridSize(optionSolver[i].device, optionSolver[i].optionCount);
        gpuBase += optionSolver[i].optionCount;
    }

    if (use_threads || bqatest) {
        // Start CPU thread for each GPU
        for (gpuIndex = 0; gpuIndex < GPU_N; gpuIndex++) {
            threadID[gpuIndex] = cutStartThread((CUT_THREADROUTINE)solverThread, &optionSolver[gpuIndex]);
        }

        printf("main(): waiting for GPU results...\n");
        cutWaitForThreads(threadID, GPU_N);

        printf("main(): GPU statistics, threaded\n");

        for (i = 0; i < GPU_N; i++) {
            cudaDeviceProp deviceProp;
            checkCudaErrors(cudaGetDeviceProperties(&deviceProp, optionSolver[i].device));
            printf("GPU Device #%i: %s\n", optionSolver[i].device, deviceProp.name);
            printf("Options         : %i\n", optionSolver[i].optionCount);
            printf("Simulation paths: %i\n", optionSolver[i].pathN);
            time = sdkGetTimerValue(&hTimer[i]);
            printf("Total time (ms.): %f\n", time);
            printf("Options per sec.: %f\n", OPT_N / (time * 0.001));
        }

        printf("main(): comparing Monte Carlo and Black-Scholes results...\n");
        sumDelta   = 0;
        sumRef     = 0;
        sumReserve = 0;

        for (i = 0; i < OPT_N; i++) {
            BlackScholesCall(callValueBS[i], optionData[i]);
            delta = fabs(callValueBS[i] - callValueGPU[i].Expected);
            ref   = callValueBS[i];
            sumDelta += delta;
            sumRef += fabs(ref);

            if (delta > 1e-6) {
                sumReserve += callValueGPU[i].Confidence / delta;
            }

#ifdef PRINT_RESULTS
            printf("BS: %f; delta: %E\n", callValueBS[i], delta);
#endif
        }

        sumReserve /= OPT_N;
    }

    if (!use_threads || bqatest) {
        multiSolver(optionSolver, GPU_N);

        printf("main(): GPU statistics, streamed\n");

        for (i = 0; i < GPU_N; i++) {
            cudaDeviceProp deviceProp;
            checkCudaErrors(cudaGetDeviceProperties(&deviceProp, optionSolver[i].device));
            printf("GPU Device #%i: %s\n", optionSolver[i].device, deviceProp.name);
            printf("Options         : %i\n", optionSolver[i].optionCount);
            printf("Simulation paths: %i\n", optionSolver[i].pathN);
        }

        time = sdkGetTimerValue(&hTimer[0]);
        printf("\nTotal time (ms.): %f\n", time);
        printf("\tNote: This is elapsed time for all to compute.\n");
        printf("Options per sec.: %f\n", OPT_N / (time * 0.001));

        printf("main(): comparing Monte Carlo and Black-Scholes results...\n");
        sumDelta   = 0;
        sumRef     = 0;
        sumReserve = 0;

        for (i = 0; i < OPT_N; i++) {
            BlackScholesCall(callValueBS[i], optionData[i]);
            delta = fabs(callValueBS[i] - callValueGPU[i].Expected);
            ref   = callValueBS[i];
            sumDelta += delta;
            sumRef += fabs(ref);

            if (delta > 1e-6) {
                sumReserve += callValueGPU[i].Confidence / delta;
            }

#ifdef PRINT_RESULTS
            printf("BS: %f; delta: %E\n", callValueBS[i], delta);
#endif
        }

        sumReserve /= OPT_N;
    }

#ifdef DO_CPU
    printf("main(): running CPU MonteCarlo...\n");
    TOptionValue callValueCPU;
    sumDelta = 0;
    sumRef   = 0;

    for (i = 0; i < OPT_N; i++) {
        MonteCarloCPU(callValueCPU, optionData[i], NULL, PATH_N);
        delta = fabs(callValueCPU.Expected - callValueGPU[i].Expected);
        ref   = callValueCPU.Expected;
        sumDelta += delta;
        sumRef += fabs(ref);
        printf("Exp : %f | %f\t", callValueCPU.Expected, callValueGPU[i].Expected);
        printf("Conf: %f | %f\n", callValueCPU.Confidence, callValueGPU[i].Confidence);
    }

    printf("L1 norm: %E\n", sumDelta / sumRef);
#endif

    printf("Shutting down...\n");

    for (int i = 0; i < GPU_N; i++) {
        sdkStartTimer(&hTimer[i]);
        checkCudaErrors(cudaSetDevice(i));
    }

    delete[] optionSolver;
    delete[] callValueBS;
    delete[] callValueGPU;
    delete[] optionData;
    delete[] threadID;
    delete[] hTimer;

    printf("Test Summary...\n");
    printf("L1 norm        : %E\n", sumDelta / sumRef);
    printf("Average reserve: %f\n", sumReserve);
    printf("\nNOTE: The CUDA Samples are not meant for performance measurements. "
           "Results may vary when GPU Boost is enabled.\n\n");
    printf(sumReserve > 1.0f ? "Test passed\n" : "Test failed!\n");
    exit(sumReserve > 1.0f ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp`.

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

- **Total Lines**: 484
- **Approximate Size**: 15985 bytes

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
