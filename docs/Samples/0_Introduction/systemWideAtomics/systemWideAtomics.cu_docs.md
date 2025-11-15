# Documentation for Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu

## File Metadata

- **Path**: `Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/systemWideAtomics
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

/* A program demonstrating trivial use of system-wide atomics on migratable
 * memory.
 */

#include <cstdio>
#include <ctime>
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <math.h>
#include <stdint.h>

#define min(a, b) (a) < (b) ? (a) : (b)
#define max(a, b) (a) > (b) ? (a) : (b)

#define LOOP_NUM 50

__global__ void atomicKernel(int *atom_arr)
{
    unsigned int tid = blockDim.x * blockIdx.x + threadIdx.x;

    for (int i = 0; i < LOOP_NUM; i++) {
        // Atomic addition
        atomicAdd_system(&atom_arr[0], 10);

        // Atomic exchange
        atomicExch_system(&atom_arr[1], tid);

        // Atomic maximum
        atomicMax_system(&atom_arr[2], tid);

        // Atomic minimum
        atomicMin_system(&atom_arr[3], tid);

        // Atomic increment (modulo 17+1)
        atomicInc_system((unsigned int *)&atom_arr[4], 17);

        // Atomic decrement
        atomicDec_system((unsigned int *)&atom_arr[5], 137);

        // Atomic compare-and-swap
        atomicCAS_system(&atom_arr[6], tid - 1, tid);

        // Bitwise atomic instructions

        // Atomic AND
        atomicAnd_system(&atom_arr[7], 2 * tid + 7);

        // Atomic OR
        atomicOr_system(&atom_arr[8], 1 << tid);

        // Atomic XOR
        atomicXor_system(&atom_arr[9], tid);
    }
}

void atomicKernel_CPU(int *atom_arr, int no_of_threads)
{
    for (int i = no_of_threads; i < 2 * no_of_threads; i++) {
        for (int j = 0; j < LOOP_NUM; j++) {
            // Atomic addition
            __sync_fetch_and_add(&atom_arr[0], 10);

            // Atomic exchange
            __sync_lock_test_and_set(&atom_arr[1], i);

            // Atomic maximum
            int old, expected;
            do {
                expected = atom_arr[2];
                old      = __sync_val_compare_and_swap(&atom_arr[2], expected, max(expected, i));
            } while (old != expected);

            // Atomic minimum
            do {
                expected = atom_arr[3];
                old      = __sync_val_compare_and_swap(&atom_arr[3], expected, min(expected, i));
            } while (old != expected);

            // Atomic increment (modulo 17+1)
            int limit = 17;
            do {
                expected = atom_arr[4];
                old      = __sync_val_compare_and_swap(&atom_arr[4], expected, (expected >= limit) ? 0 : expected + 1);
            } while (old != expected);

            // Atomic decrement
            limit = 137;
            do {
                expected = atom_arr[5];
                old      = __sync_val_compare_and_swap(
                    &atom_arr[5], expected, ((expected == 0) || (expected > limit)) ? limit : expected - 1);
            } while (old != expected);

            // Atomic compare-and-swap
            __sync_val_compare_and_swap(&atom_arr[6], i - 1, i);

            // Bitwise atomic instructions

            // Atomic AND
            __sync_fetch_and_and(&atom_arr[7], 2 * i + 7);

            // Atomic OR
            __sync_fetch_and_or(&atom_arr[8], 1 << i);

            // Atomic XOR
            // 11th element should be 0xff
            __sync_fetch_and_xor(&atom_arr[9], i);
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
//! Compute reference data set
//! Each element is multiplied with the number of threads / array length
//! @param reference  reference data, computed but preallocated
//! @param idata      input data as provided to device
//! @param len        number of elements in reference / idata
////////////////////////////////////////////////////////////////////////////////
int verify(int *testData, const int len)
{
    int val = 0;

    for (int i = 0; i < len * LOOP_NUM; ++i) {
        val += 10;
    }

    if (val != testData[0]) {
        printf("atomicAdd failed val = %d testData = %d\n", val, testData[0]);
        return false;
    }

    val = 0;

    bool found = false;

    for (int i = 0; i < len; ++i) {
        // second element should be a member of [0, len)
        if (i == testData[1]) {
            found = true;
            break;
        }
    }

    if (!found) {
        printf("atomicExch failed\n");
        return false;
    }

    val = -(1 << 8);

    for (int i = 0; i < len; ++i) {
        // third element should be len-1
        val = max(val, i);
    }

    if (val != testData[2]) {
        printf("atomicMax failed\n");
        return false;
    }

    val = 1 << 8;

    for (int i = 0; i < len; ++i) {
        val = min(val, i);
    }

    if (val != testData[3]) {
        printf("atomicMin failed\n");
        return false;
    }

    int limit = 17;
    val       = 0;

    for (int i = 0; i < len * LOOP_NUM; ++i) {
        val = (val >= limit) ? 0 : val + 1;
    }

    if (val != testData[4]) {
        printf("atomicInc failed\n");
        return false;
    }

    limit = 137;
    val   = 0;

    for (int i = 0; i < len * LOOP_NUM; ++i) {
        val = ((val == 0) || (val > limit)) ? limit : val - 1;
    }

    if (val != testData[5]) {
        printf("atomicDec failed\n");
        return false;
    }

    found = false;

    for (int i = 0; i < len; ++i) {
        // seventh element should be a member of [0, len)
        if (i == testData[6]) {
            found = true;
            break;
        }
    }

    if (!found) {
        printf("atomicCAS failed\n");
        return false;
    }

    val = 0xff;

    for (int i = 0; i < len; ++i) {
        // 8th element should be 1
        val &= (2 * i + 7);
    }

    if (val != testData[7]) {
        printf("atomicAnd failed\n");
        return false;
    }

    val = 0;

    for (int i = 0; i < len; ++i) {
        // 9th element should be 0xff
        val |= (1 << i);
    }

    if (val != testData[8]) {
        printf("atomicOr failed\n");
        return false;
    }

    val = 0xff;

    for (int i = 0; i < len; ++i) {
        // 11th element should be 0xff
        val ^= i;
    }

    if (val != testData[9]) {
        printf("atomicXor failed\n");
        return false;
    }

    return true;
}

int main(int argc, char **argv)
{
    // set device
    cudaDeviceProp device_prop;
    int            dev_id = findCudaDevice(argc, (const char **)argv);
    checkCudaErrors(cudaGetDeviceProperties(&device_prop, dev_id));

    if (!device_prop.managedMemory) {
        // This samples requires being run on a device that supports Unified Memory
        fprintf(stderr, "Unified Memory not supported on this device\n");
        exit(EXIT_WAIVED);
    }

    int computeMode;
    checkCudaErrors(cudaDeviceGetAttribute(&computeMode, cudaDevAttrComputeMode, dev_id));
    if (computeMode == cudaComputeModeProhibited) {
        // This sample requires being run with a default or process exclusive mode
        fprintf(stderr,
                "This sample requires a device in either default or process "
                "exclusive mode\n");
        exit(EXIT_WAIVED);
    }

    if (device_prop.major < 6) {
        printf("%s: requires a minimum CUDA compute 6.0 capability, waiving "
               "testing.\n",
               argv[0]);
        exit(EXIT_WAIVED);
    }

    unsigned int numThreads = 256;
    unsigned int numBlocks  = 64;
    unsigned int numData    = 10;

    int *atom_arr;

    if (device_prop.pageableMemoryAccess) {
        printf("CAN access pageable memory\n");
        atom_arr = (int *)malloc(sizeof(int) * numData);
    }
    else {
        printf("CANNOT access pageable memory\n");
        checkCudaErrors(cudaMallocManaged(&atom_arr, sizeof(int) * numData));
    }

    for (unsigned int i = 0; i < numData; i++)
        atom_arr[i] = 0;

    // To make the AND and XOR tests generate something other than 0...
    atom_arr[7] = atom_arr[9] = 0xff;

    atomicKernel<<<numBlocks, numThreads>>>(atom_arr);
    atomicKernel_CPU(atom_arr, numBlocks * numThreads);

    checkCudaErrors(cudaDeviceSynchronize());

    // Compute & verify reference solution
    int testResult = verify(atom_arr, 2 * numThreads * numBlocks);

    if (device_prop.pageableMemoryAccess) {
        free(atom_arr);
    }
    else {
        cudaFree(atom_arr);
    }

    printf("systemWideAtomics completed, returned %s \n", testResult ? "OK" : "ERROR!");
    exit(testResult ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu`.

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

- **Total Lines**: 346
- **Approximate Size**: 9858 bytes

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
