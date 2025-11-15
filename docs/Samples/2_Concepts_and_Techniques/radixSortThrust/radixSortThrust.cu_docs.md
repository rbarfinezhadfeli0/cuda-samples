# Documentation for Samples/2_Concepts_and_Techniques/radixSortThrust/radixSortThrust.cu

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/radixSortThrust/radixSortThrust.cu`
- **Type**: .cu
- **Location**: Samples/2_Concepts_and_Techniques/radixSortThrust
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

#include <algorithm>
#include <helper_cuda.h>
#include <limits.h>
#include <thrust/copy.h>
#include <thrust/detail/type_traits.h>
#include <thrust/device_vector.h>
#include <thrust/generate.h>
#include <thrust/host_vector.h>
#include <thrust/random.h>
#include <thrust/sequence.h>
#include <thrust/sort.h>
#include <time.h>

template <typename T, bool floatKeys> bool testSort(int argc, char **argv)
{
    int cmdVal;
    int keybits = 32;

    unsigned int numElements = 1048576;
    bool         keysOnly    = checkCmdLineFlag(argc, (const char **)argv, "keysonly");
    bool         quiet       = checkCmdLineFlag(argc, (const char **)argv, "quiet");

    if (checkCmdLineFlag(argc, (const char **)argv, "n")) {
        cmdVal      = getCmdLineArgumentInt(argc, (const char **)argv, "n");
        numElements = cmdVal;

        if (cmdVal < 0) {
            printf("Error: elements must be > 0, elements=%d is invalid\n", cmdVal);
            exit(EXIT_SUCCESS);
        }
    }

    if (checkCmdLineFlag(argc, (const char **)argv, "keybits")) {
        cmdVal  = getCmdLineArgumentInt(argc, (const char **)argv, "keybits");
        keybits = cmdVal;

        if (keybits <= 0) {
            printf("Error: keybits must be > 0, keybits=%d is invalid\n", keybits);
            exit(EXIT_SUCCESS);
        }
    }

    unsigned int numIterations = (numElements >= 16777216) ? 10 : 100;

    if (checkCmdLineFlag(argc, (const char **)argv, "iterations")) {
        cmdVal        = getCmdLineArgumentInt(argc, (const char **)argv, "iterations");
        numIterations = cmdVal;
    }

    if (checkCmdLineFlag(argc, (const char **)argv, "help")) {
        printf("Command line:\nradixSortThrust [-option]\n");
        printf("Valid options:\n");
        printf("-n=<N>        : number of elements to sort\n");
        printf("-keybits=bits : keybits must be > 0\n");
        printf("-keysonly     : only sort an array of keys (default sorts key-value "
               "pairs)\n");
        printf("-float        : use 32-bit float keys (default is 32-bit unsigned "
               "int)\n");
        printf("-quiet        : Output only the number of elements and the time to "
               "sort\n");
        printf("-help         : Output a help message\n");
        exit(EXIT_SUCCESS);
    }

    if (!quiet)
        printf("\nSorting %d %d-bit %s keys %s\n\n",
               numElements,
               keybits,
               floatKeys ? "float" : "unsigned int",
               keysOnly ? "(only)" : "and values");

    int deviceID = -1;

    if (cudaSuccess == cudaGetDevice(&deviceID)) {
        cudaDeviceProp devprop;
        cudaGetDeviceProperties(&devprop, deviceID);
        unsigned int totalMem = (keysOnly ? 2 : 4) * numElements * sizeof(T);

        if (devprop.totalGlobalMem < totalMem) {
            printf("Error: insufficient amount of memory to sort %d elements.\n", numElements);
            printf("%d bytes needed, %d bytes available\n", (int)totalMem, (int)devprop.totalGlobalMem);
            exit(EXIT_SUCCESS);
        }
    }

    thrust::host_vector<T>            h_keys(numElements);
    thrust::host_vector<T>            h_keysSorted(numElements);
    thrust::host_vector<unsigned int> h_values;

    if (!keysOnly)
        h_values = thrust::host_vector<unsigned int>(numElements);

    // Fill up with some random data
    thrust::default_random_engine rng(clock());

    if (floatKeys) {
        thrust::uniform_real_distribution<float> u01(0, 1);

        for (int i = 0; i < (int)numElements; i++)
            h_keys[i] = u01(rng);
    }
    else {
        thrust::uniform_int_distribution<unsigned int> u(0, UINT_MAX);

        for (int i = 0; i < (int)numElements; i++)
            h_keys[i] = u(rng);
    }

    if (!keysOnly)
        thrust::sequence(h_values.begin(), h_values.end());

    // Copy data onto the GPU
    thrust::device_vector<T>            d_keys;
    thrust::device_vector<unsigned int> d_values;

    // run multiple iterations to compute an average sort time
    cudaEvent_t start_event, stop_event;
    checkCudaErrors(cudaEventCreate(&start_event));
    checkCudaErrors(cudaEventCreate(&stop_event));

    float totalTime = 0;

    for (unsigned int i = 0; i < numIterations; i++) {
        // reset data before sort
        d_keys = h_keys;

        if (!keysOnly)
            d_values = h_values;

        checkCudaErrors(cudaEventRecord(start_event, 0));

        if (keysOnly)
            thrust::sort(d_keys.begin(), d_keys.end());
        else
            thrust::sort_by_key(d_keys.begin(), d_keys.end(), d_values.begin());

        checkCudaErrors(cudaEventRecord(stop_event, 0));
        checkCudaErrors(cudaEventSynchronize(stop_event));

        float time = 0;
        checkCudaErrors(cudaEventElapsedTime(&time, start_event, stop_event));
        totalTime += time;
    }

    totalTime /= (1.0e3f * numIterations);
    printf("radixSortThrust, Throughput = %.4f MElements/s, Time = %.5f s, Size = "
           "%u elements\n",
           1.0e-6f * numElements / totalTime,
           totalTime,
           numElements);

    getLastCudaError("after radixsort");

    // Get results back to host for correctness checking
    thrust::copy(d_keys.begin(), d_keys.end(), h_keysSorted.begin());

    if (!keysOnly)
        thrust::copy(d_values.begin(), d_values.end(), h_values.begin());

    getLastCudaError("copying results to host memory");

    // Check results
    bool bTestResult = thrust::is_sorted(h_keysSorted.begin(), h_keysSorted.end());

    checkCudaErrors(cudaEventDestroy(start_event));
    checkCudaErrors(cudaEventDestroy(stop_event));

    if (!bTestResult && !quiet) {
        return false;
    }

    return bTestResult;
}

int main(int argc, char **argv)
{
    // Start logs
    printf("%s Starting...\n\n", argv[0]);

    findCudaDevice(argc, (const char **)argv);

    bool bTestResult = false;

    if (checkCmdLineFlag(argc, (const char **)argv, "float"))
        bTestResult = testSort<float, true>(argc, argv);
    else
        bTestResult = testSort<unsigned int, false>(argc, argv);

    printf(bTestResult ? "Test passed\n" : "Test failed!\n");
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/radixSortThrust/radixSortThrust.cu`.

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

- **Total Lines**: 218
- **Approximate Size**: 7719 bytes

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
