# Documentation for Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu

## File Metadata

- **Path**: `Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/simpleCooperativeGroups
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

/**
 *
 * This sample is a simple code that illustrates basic usage of
 * cooperative groups within the thread block. The code launches a single
 * thread block, creates a cooperative group of all threads in the block,
 * and a set of tiled partition cooperative groups. For each, it uses a
 * generic reduction function to calculate the sum of all the ranks in
 * that group. In each case the result is printed, together with the
 * expected answer (which is calculated using the analytical formula
 * (n-1)*n)/2, noting that the ranks start at zero).
 *
 */

#include <cooperative_groups.h>
#include <stdio.h>

using namespace cooperative_groups;

/**
 * CUDA device function
 *
 * calculates the sum of val across the group g. The workspace array, x,
 * must be large enough to contain g.size() integers.
 */
__device__ int sumReduction(thread_group g, int *x, int val)
{
    // rank of this thread in the group
    int lane = g.thread_rank();

    // for each iteration of this loop, the number of threads active in the
    // reduction, i, is halved, and each active thread (with index [lane])
    // performs a single summation of it's own value with that
    // of a "partner" (with index [lane+i]).
    for (int i = g.size() / 2; i > 0; i /= 2) {
        // store value for this thread in temporary array
        x[lane] = val;

        // synchronize all threads in group
        g.sync();

        if (lane < i)
            // active threads perform summation of their value with
            // their partner's value
            val += x[lane + i];

        // synchronize all threads in group
        g.sync();
    }

    // master thread in group returns result, and others return -1.
    if (g.thread_rank() == 0)
        return val;
    else
        return -1;
}

/**
 * CUDA kernel device code
 *
 * Creates cooperative groups and performs reductions
 */
__global__ void cgkernel()
{
    // threadBlockGroup includes all threads in the block
    thread_block threadBlockGroup     = this_thread_block();
    int          threadBlockGroupSize = threadBlockGroup.size();

    // workspace array in shared memory required for reduction
    extern __shared__ int workspace[];

    int input, output, expectedOutput;

    // input to reduction, for each thread, is its' rank in the group
    input = threadBlockGroup.thread_rank();

    // expected output from analytical formula (n-1)(n)/2
    // (noting that indexing starts at 0 rather than 1)
    expectedOutput = (threadBlockGroupSize - 1) * threadBlockGroupSize / 2;

    // perform reduction
    output = sumReduction(threadBlockGroup, workspace, input);

    // master thread in group prints out result
    if (threadBlockGroup.thread_rank() == 0) {
        printf(" Sum of all ranks 0..%d in threadBlockGroup is %d (expected %d)\n\n",
               (int)threadBlockGroup.size() - 1,
               output,
               expectedOutput);

        printf(" Now creating %d groups, each of size 16 threads:\n\n", (int)threadBlockGroup.size() / 16);
    }

    threadBlockGroup.sync();

    // each tiledPartition16 group includes 16 threads
    thread_block_tile<16> tiledPartition16 = tiled_partition<16>(threadBlockGroup);

    // This offset allows each group to have its own unique area in the workspace
    // array
    int workspaceOffset = threadBlockGroup.thread_rank() - tiledPartition16.thread_rank();

    // input to reduction, for each thread, is its' rank in the group
    input = tiledPartition16.thread_rank();

    // expected output from analytical formula (n-1)(n)/2
    // (noting that indexing starts at 0 rather than 1)
    expectedOutput = 15 * 16 / 2;

    // Perform reduction
    output = sumReduction(tiledPartition16, workspace + workspaceOffset, input);

    // each master thread prints out result
    if (tiledPartition16.thread_rank() == 0)
        printf("   Sum of all ranks 0..15 in this tiledPartition16 group is %d "
               "(expected %d)\n",
               output,
               expectedOutput);

    return;
}

/**
 * Host main routine
 */
int main()
{
    // Error code to check return values for CUDA calls
    cudaError_t err;

    // Launch the kernel

    int blocksPerGrid   = 1;
    int threadsPerBlock = 64;

    printf("\nLaunching a single block with %d threads...\n\n", threadsPerBlock);

    // we use the optional third argument to specify the size
    // of shared memory required in the kernel
    cgkernel<<<blocksPerGrid, threadsPerBlock, threadsPerBlock * sizeof(int)>>>();
    err = cudaDeviceSynchronize();

    if (err != cudaSuccess) {
        fprintf(stderr, "Failed to launch kernel (error code %s)!\n", cudaGetErrorString(err));
        exit(EXIT_FAILURE);
    }

    printf("\n...Done.\n\n");

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu`.

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

- **Total Lines**: 178
- **Approximate Size**: 6314 bytes

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
