# Documentation for Samples/2_Concepts_and_Techniques/eigenvalues/bisect_large.cu

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/eigenvalues/bisect_large.cu`
- **Type**: .cu
- **Location**: Samples/2_Concepts_and_Techniques/eigenvalues
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

/* Computation of eigenvalues of a large symmetric, tridiagonal matrix */

// includes, system
#include <float.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project
#include "bisect_large.cuh"
#include "config.h"
#include "helper_cuda.h"
#include "helper_functions.h"
#include "matlab.h"
#include "structs.h"
#include "util.h"

// includes, kernels
#include "bisect_kernel_large.cuh"
#include "bisect_kernel_large_multi.cuh"
#include "bisect_kernel_large_onei.cuh"

////////////////////////////////////////////////////////////////////////////////
//! Initialize variables and memory for result
//! @param  result handles to memory
//! @param  matrix_size  size of the matrix
////////////////////////////////////////////////////////////////////////////////
void initResultDataLargeMatrix(ResultDataLarge &result, const unsigned int mat_size)
{
    // helper variables to initialize memory
    unsigned int zero        = 0;
    unsigned int mat_size_f  = sizeof(float) * mat_size;
    unsigned int mat_size_ui = sizeof(unsigned int) * mat_size;

    float        *tempf  = (float *)malloc(mat_size_f);
    unsigned int *tempui = (unsigned int *)malloc(mat_size_ui);

    for (unsigned int i = 0; i < mat_size; ++i) {
        tempf[i]  = 0.0f;
        tempui[i] = 0;
    }

    // number of intervals containing only one eigenvalue after the first step
    checkCudaErrors(cudaMalloc((void **)&result.g_num_one, sizeof(unsigned int)));
    checkCudaErrors(cudaMemcpy(result.g_num_one, &zero, sizeof(unsigned int), cudaMemcpyHostToDevice));

    // number of (thread) blocks of intervals with multiple eigenvalues after
    // the first iteration
    checkCudaErrors(cudaMalloc((void **)&result.g_num_blocks_mult, sizeof(unsigned int)));
    checkCudaErrors(cudaMemcpy(result.g_num_blocks_mult, &zero, sizeof(unsigned int), cudaMemcpyHostToDevice));

    checkCudaErrors(cudaMalloc((void **)&result.g_left_one, mat_size_f));
    checkCudaErrors(cudaMalloc((void **)&result.g_right_one, mat_size_f));
    checkCudaErrors(cudaMalloc((void **)&result.g_pos_one, mat_size_ui));

    checkCudaErrors(cudaMalloc((void **)&result.g_left_mult, mat_size_f));
    checkCudaErrors(cudaMalloc((void **)&result.g_right_mult, mat_size_f));
    checkCudaErrors(cudaMalloc((void **)&result.g_left_count_mult, mat_size_ui));
    checkCudaErrors(cudaMalloc((void **)&result.g_right_count_mult, mat_size_ui));

    checkCudaErrors(cudaMemcpy(result.g_left_one, tempf, mat_size_f, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(result.g_right_one, tempf, mat_size_f, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(result.g_pos_one, tempui, mat_size_ui, cudaMemcpyHostToDevice));

    checkCudaErrors(cudaMemcpy(result.g_left_mult, tempf, mat_size_f, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(result.g_right_mult, tempf, mat_size_f, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(result.g_left_count_mult, tempui, mat_size_ui, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(result.g_right_count_mult, tempui, mat_size_ui, cudaMemcpyHostToDevice));

    checkCudaErrors(cudaMalloc((void **)&result.g_blocks_mult, mat_size_ui));
    checkCudaErrors(cudaMemcpy(result.g_blocks_mult, tempui, mat_size_ui, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMalloc((void **)&result.g_blocks_mult_sum, mat_size_ui));
    checkCudaErrors(cudaMemcpy(result.g_blocks_mult_sum, tempui, mat_size_ui, cudaMemcpyHostToDevice));

    checkCudaErrors(cudaMalloc((void **)&result.g_lambda_mult, mat_size_f));
    checkCudaErrors(cudaMemcpy(result.g_lambda_mult, tempf, mat_size_f, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMalloc((void **)&result.g_pos_mult, mat_size_ui));
    checkCudaErrors(cudaMemcpy(result.g_pos_mult, tempf, mat_size_ui, cudaMemcpyHostToDevice));
}

////////////////////////////////////////////////////////////////////////////////
//! Cleanup result memory
//! @param result  handles to memory
////////////////////////////////////////////////////////////////////////////////
void cleanupResultDataLargeMatrix(ResultDataLarge &result)
{
    checkCudaErrors(cudaFree(result.g_num_one));
    checkCudaErrors(cudaFree(result.g_num_blocks_mult));
    checkCudaErrors(cudaFree(result.g_left_one));
    checkCudaErrors(cudaFree(result.g_right_one));
    checkCudaErrors(cudaFree(result.g_pos_one));
    checkCudaErrors(cudaFree(result.g_left_mult));
    checkCudaErrors(cudaFree(result.g_right_mult));
    checkCudaErrors(cudaFree(result.g_left_count_mult));
    checkCudaErrors(cudaFree(result.g_right_count_mult));
    checkCudaErrors(cudaFree(result.g_blocks_mult));
    checkCudaErrors(cudaFree(result.g_blocks_mult_sum));
    checkCudaErrors(cudaFree(result.g_lambda_mult));
    checkCudaErrors(cudaFree(result.g_pos_mult));
}

////////////////////////////////////////////////////////////////////////////////
//! Run the kernels to compute the eigenvalues for large matrices
//! @param  input   handles to input data
//! @param  result  handles to result data
//! @param  mat_size  matrix size
//! @param  precision  desired precision of eigenvalues
//! @param  lg  lower limit of Gerschgorin interval
//! @param  ug  upper limit of Gerschgorin interval
//! @param  iterations  number of iterations (for timing)
////////////////////////////////////////////////////////////////////////////////
void computeEigenvaluesLargeMatrix(const InputData       &input,
                                   const ResultDataLarge &result,
                                   const unsigned int     mat_size,
                                   const float            precision,
                                   const float            lg,
                                   const float            ug,
                                   const unsigned int     iterations)
{
    dim3 blocks(1, 1, 1);
    dim3 threads(MAX_THREADS_BLOCK, 1, 1);

    StopWatchInterface *timer_step1      = NULL;
    StopWatchInterface *timer_step2_one  = NULL;
    StopWatchInterface *timer_step2_mult = NULL;
    StopWatchInterface *timer_total      = NULL;
    sdkCreateTimer(&timer_step1);
    sdkCreateTimer(&timer_step2_one);
    sdkCreateTimer(&timer_step2_mult);
    sdkCreateTimer(&timer_total);

    sdkStartTimer(&timer_total);

    // do for multiple iterations to improve timing accuracy
    for (unsigned int iter = 0; iter < iterations; ++iter) {
        sdkStartTimer(&timer_step1);
        bisectKernelLarge<<<blocks, threads>>>(input.g_a,
                                               input.g_b,
                                               mat_size,
                                               lg,
                                               ug,
                                               0,
                                               mat_size,
                                               precision,
                                               result.g_num_one,
                                               result.g_num_blocks_mult,
                                               result.g_left_one,
                                               result.g_right_one,
                                               result.g_pos_one,
                                               result.g_left_mult,
                                               result.g_right_mult,
                                               result.g_left_count_mult,
                                               result.g_right_count_mult,
                                               result.g_blocks_mult,
                                               result.g_blocks_mult_sum);

        getLastCudaError("Kernel launch failed.");
        checkCudaErrors(cudaDeviceSynchronize());
        sdkStopTimer(&timer_step1);

        // get the number of intervals containing one eigenvalue after the first
        // processing step
        unsigned int num_one_intervals;
        checkCudaErrors(cudaMemcpy(&num_one_intervals, result.g_num_one, sizeof(unsigned int), cudaMemcpyDeviceToHost));

        dim3 grid_onei;
        grid_onei.x = getNumBlocksLinear(num_one_intervals, MAX_THREADS_BLOCK);
        dim3 threads_onei;
        // use always max number of available threads to better balance load times
        // for matrix data
        threads_onei.x = MAX_THREADS_BLOCK;

        // compute eigenvalues for intervals that contained only one eigenvalue
        // after the first processing step
        sdkStartTimer(&timer_step2_one);

        bisectKernelLarge_OneIntervals<<<grid_onei, threads_onei>>>(input.g_a,
                                                                    input.g_b,
                                                                    mat_size,
                                                                    num_one_intervals,
                                                                    result.g_left_one,
                                                                    result.g_right_one,
                                                                    result.g_pos_one,
                                                                    precision);

        getLastCudaError("bisectKernelLarge_OneIntervals() FAILED.");
        checkCudaErrors(cudaDeviceSynchronize());
        sdkStopTimer(&timer_step2_one);

        // process intervals that contained more than one eigenvalue after
        // the first processing step

        // get the number of blocks of intervals that contain, in total when
        // each interval contains only one eigenvalue, not more than
        // MAX_THREADS_BLOCK threads
        unsigned int num_blocks_mult = 0;
        checkCudaErrors(
            cudaMemcpy(&num_blocks_mult, result.g_num_blocks_mult, sizeof(unsigned int), cudaMemcpyDeviceToHost));

        // setup the execution environment
        dim3 grid_mult(num_blocks_mult, 1, 1);
        dim3 threads_mult(MAX_THREADS_BLOCK, 1, 1);

        sdkStartTimer(&timer_step2_mult);

        bisectKernelLarge_MultIntervals<<<grid_mult, threads_mult>>>(input.g_a,
                                                                     input.g_b,
                                                                     mat_size,
                                                                     result.g_blocks_mult,
                                                                     result.g_blocks_mult_sum,
                                                                     result.g_left_mult,
                                                                     result.g_right_mult,
                                                                     result.g_left_count_mult,
                                                                     result.g_right_count_mult,
                                                                     result.g_lambda_mult,
                                                                     result.g_pos_mult,
                                                                     precision);

        getLastCudaError("bisectKernelLarge_MultIntervals() FAILED.");
        checkCudaErrors(cudaDeviceSynchronize());
        sdkStopTimer(&timer_step2_mult);
    }

    sdkStopTimer(&timer_total);

    printf("Average time step 1: %f ms\n", sdkGetTimerValue(&timer_step1) / (float)iterations);
    printf("Average time step 2, one intervals: %f ms\n", sdkGetTimerValue(&timer_step2_one) / (float)iterations);
    printf("Average time step 2, mult intervals: %f ms\n", sdkGetTimerValue(&timer_step2_mult) / (float)iterations);

    printf("Average time TOTAL: %f ms\n", sdkGetTimerValue(&timer_total) / (float)iterations);

    sdkDeleteTimer(&timer_step1);
    sdkDeleteTimer(&timer_step2_one);
    sdkDeleteTimer(&timer_step2_mult);
    sdkDeleteTimer(&timer_total);
}

////////////////////////////////////////////////////////////////////////////////
//! Process the result, that is obtain result from device and do simple sanity
//! checking
//! @param  input   handles to input data
//! @param  result  handles to result data
//! @param  mat_size  matrix size
//! @param  filename  output filename
////////////////////////////////////////////////////////////////////////////////
bool processResultDataLargeMatrix(const InputData       &input,
                                  const ResultDataLarge &result,
                                  const unsigned int     mat_size,
                                  const char            *filename,
                                  const unsigned int     user_defined,
                                  char                  *exec_path)
{
    bool               bCompareResult = false;
    const unsigned int mat_size_ui    = sizeof(unsigned int) * mat_size;
    const unsigned int mat_size_f     = sizeof(float) * mat_size;

    // copy data from intervals that contained more than one eigenvalue after
    // the first processing step
    float *lambda_mult = (float *)malloc(sizeof(float) * mat_size);
    checkCudaErrors(cudaMemcpy(lambda_mult, result.g_lambda_mult, sizeof(float) * mat_size, cudaMemcpyDeviceToHost));
    unsigned int *pos_mult = (unsigned int *)malloc(sizeof(unsigned int) * mat_size);
    checkCudaErrors(cudaMemcpy(pos_mult, result.g_pos_mult, sizeof(unsigned int) * mat_size, cudaMemcpyDeviceToHost));

    unsigned int *blocks_mult_sum = (unsigned int *)malloc(sizeof(unsigned int) * mat_size);
    checkCudaErrors(
        cudaMemcpy(blocks_mult_sum, result.g_blocks_mult_sum, sizeof(unsigned int) * mat_size, cudaMemcpyDeviceToHost));

    unsigned int num_one_intervals;
    checkCudaErrors(cudaMemcpy(&num_one_intervals, result.g_num_one, sizeof(unsigned int), cudaMemcpyDeviceToHost));

    unsigned int sum_blocks_mult = mat_size - num_one_intervals;

    // copy data for intervals that contained one eigenvalue after the first
    // processing step
    float        *left_one  = (float *)malloc(mat_size_f);
    float        *right_one = (float *)malloc(mat_size_f);
    unsigned int *pos_one   = (unsigned int *)malloc(mat_size_ui);
    checkCudaErrors(cudaMemcpy(left_one, result.g_left_one, mat_size_f, cudaMemcpyDeviceToHost));
    checkCudaErrors(cudaMemcpy(right_one, result.g_right_one, mat_size_f, cudaMemcpyDeviceToHost));
    checkCudaErrors(cudaMemcpy(pos_one, result.g_pos_one, mat_size_ui, cudaMemcpyDeviceToHost));

    // extract eigenvalues
    float *eigenvals = (float *)malloc(mat_size_f);

    // singleton intervals generated in the second step
    for (unsigned int i = 0; i < sum_blocks_mult; ++i) {
        eigenvals[pos_mult[i] - 1] = lambda_mult[i];
    }

    // singleton intervals generated in the first step
    unsigned int index = 0;

    for (unsigned int i = 0; i < num_one_intervals; ++i, ++index) {
        eigenvals[pos_one[i] - 1] = left_one[i];
    }

    if (1 == user_defined) {
        // store result
        writeTridiagSymMatlab(filename, input.a, input.b + 1, eigenvals, mat_size);
        // getLastCudaError( sdkWriteFilef( filename, eigenvals, mat_size, 0.0f));

        printf("User requests non-default argument(s), skipping self-check!\n");
        bCompareResult = true;
    }
    else {
        // compare with reference solution

        float       *reference       = NULL;
        unsigned int input_data_size = 0;

        char *ref_path = sdkFindFilePath("reference.dat", exec_path);
        assert(NULL != ref_path);
        sdkReadFile(ref_path, &reference, &input_data_size, false);
        assert(input_data_size == mat_size);

        // there's an imprecision of Sturm count computation which makes an
        // additional offset necessary
        float tolerance = 1.0e-5f + 5.0e-6f;

        if (sdkCompareL2fe(reference, eigenvals, mat_size, tolerance) == true) {
            bCompareResult = true;
        }
        else {
            bCompareResult = false;
        }

        free(ref_path);
        free(reference);
    }

    freePtr(eigenvals);
    freePtr(lambda_mult);
    freePtr(pos_mult);
    freePtr(blocks_mult_sum);
    freePtr(left_one);
    freePtr(right_one);
    freePtr(pos_one);

    return bCompareResult;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/eigenvalues/bisect_large.cu`.

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

- **Total Lines**: 369
- **Approximate Size**: 17707 bytes

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
