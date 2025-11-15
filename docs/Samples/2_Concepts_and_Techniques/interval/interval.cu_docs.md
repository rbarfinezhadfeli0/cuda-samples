# Documentation for Samples/2_Concepts_and_Techniques/interval/interval.cu

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/interval/interval.cu`
- **Type**: .cu
- **Location**: Samples/2_Concepts_and_Techniques/interval
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

/* Example of program using the interval_gpu<T> template class and operators:
 * Search for roots of a function using an interval Newton method.
 *
 * Use the command-line argument "--n=<N>" to select which GPU implementation to
 * use,
 * otherwise the naive implementation will be used by default.
 * 0: the naive implementation
 * 1: the optimized implementation
 * 2: the recursive implementation
 *
 */

const static char *sSDKsample = "Interval Computing";

#include <iostream>
#include <stdio.h>

#include "cpu_interval.h"
#include "cuda_interval.h"
#include "helper_cuda.h"
#include "interval.h"

int main(int argc, char *argv[])
{
    int implementation_choice = 0;

    printf("[%s]  starting ...\n\n", sSDKsample);

    if (checkCmdLineFlag(argc, (const char **)argv, "n")) {
        implementation_choice = getCmdLineArgumentInt(argc, (const char **)argv, "n");
    }

    // Pick the best GPU available, or if the developer selects one at the command
    // line
    int            devID = findCudaDevice(argc, (const char **)argv);
    cudaDeviceProp deviceProp;
    cudaGetDeviceProperties(&deviceProp, devID);
    printf("> GPU Device has Compute Capabilities SM %d.%d\n\n", deviceProp.major, deviceProp.minor);

    switch (implementation_choice) {
    case 0:
        printf("GPU naive implementation\n");
        break;

    case 1:
        printf("GPU optimized implementation\n");
        break;

    case 2:
        printf("GPU recursive implementation (requires Compute SM 2.0+)\n");
        break;

    default:
        printf("GPU naive implementation\n");
    }

    interval_gpu<T> *d_result;
    int             *d_nresults;
    int             *h_nresults = new int[THREADS];
    cudaEvent_t      start, stop;

    CHECKED_CALL(cudaSetDevice(devID));
    CHECKED_CALL(cudaMalloc((void **)&d_result, THREADS * DEPTH_RESULT * sizeof(*d_result)));
    CHECKED_CALL(cudaMalloc((void **)&d_nresults, THREADS * sizeof(*d_nresults)));
    CHECKED_CALL(cudaEventCreate(&start));
    CHECKED_CALL(cudaEventCreate(&stop));

    // We need L1 cache to store the stack (only applicable to sm_20 and higher)
    CHECKED_CALL(cudaFuncSetCacheConfig(test_interval_newton<T>, cudaFuncCachePreferL1));

    // Increase the stack size large enough for the non-inlined and recursive
    // function calls (only applicable to sm_20 and higher)
    CHECKED_CALL(cudaDeviceSetLimit(cudaLimitStackSize, 8192));

    interval_gpu<T> i(0.01f, 4.0f);
    std::cout << "Searching for roots in [" << i.lower() << ", " << i.upper() << "]...\n";

    CHECKED_CALL(cudaEventRecord(start, 0));

    for (int it = 0; it < NUM_RUNS; ++it) {
        test_interval_newton<T><<<GRID_SIZE, BLOCK_SIZE>>>(d_result, d_nresults, i, implementation_choice);
        CHECKED_CALL(cudaGetLastError());
    }

    CHECKED_CALL(cudaEventRecord(stop, 0));
    CHECKED_CALL(cudaDeviceSynchronize());

    I_CPU *h_result = new I_CPU[THREADS * DEPTH_RESULT];
    CHECKED_CALL(cudaMemcpy(h_result, d_result, THREADS * DEPTH_RESULT * sizeof(*d_result), cudaMemcpyDeviceToHost));
    CHECKED_CALL(cudaMemcpy(h_nresults, d_nresults, THREADS * sizeof(*d_nresults), cudaMemcpyDeviceToHost));

    std::cout << "Found " << h_nresults[0] << " intervals that may contain the root(s)\n";
    std::cout.precision(15);

    for (int i = 0; i != h_nresults[0]; ++i) {
        std::cout << " i[" << i << "] ="
                  << " [" << h_result[THREADS * i + 0].lower() << ", " << h_result[THREADS * i + 0].upper() << "]\n";
    }

    float time;
    CHECKED_CALL(cudaEventElapsedTime(&time, start, stop));
    std::cout << "Number of equations solved: " << THREADS << "\n";
    std::cout << "Time per equation: " << 1000000.0f * (time / (float)(THREADS)) / NUM_RUNS << " us\n";

    CHECKED_CALL(cudaEventDestroy(start));
    CHECKED_CALL(cudaEventDestroy(stop));
    CHECKED_CALL(cudaFree(d_result));
    CHECKED_CALL(cudaFree(d_nresults));

    // Compute the results using a CPU implementation based on the Boost library
    I_CPU  i_cpu(0.01f, 4.0f);
    I_CPU *h_result_cpu   = new I_CPU[THREADS * DEPTH_RESULT];
    int   *h_nresults_cpu = new int[THREADS];
    test_interval_newton_cpu<I_CPU>(h_result_cpu, h_nresults_cpu, i_cpu);

    // Compare the CPU and GPU results
    bool bTestResult = checkAgainstHost(h_nresults, h_nresults_cpu, h_result, h_result_cpu);

    delete[] h_result_cpu;
    delete[] h_nresults_cpu;
    delete[] h_result;
    delete[] h_nresults;

    exit(bTestResult ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/interval/interval.cu`.

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

- **Total Lines**: 153
- **Approximate Size**: 6065 bytes

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
