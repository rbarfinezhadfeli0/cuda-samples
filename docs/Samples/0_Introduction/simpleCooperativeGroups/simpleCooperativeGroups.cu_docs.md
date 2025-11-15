# Documentation: Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu`
- **Filename**: `simpleCooperativeGroups.cu`
- **Language**: cuda
- **Size**: 6314 bytes
- **Lines**: 178
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```cuda
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

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 3 function(s) in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cooperative_groups.h`
- `stdio.h`

### CUDA Kernels
#### `cgkernel`
- **Return Type**: `void`
- **Parameters**: ``
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `int sumReduction(thread_group g, int *x, int val)`
- Function in Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu

#### `void cgkernel()`
- Function in Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu

#### `int main()`
- Function in Samples/0_Introduction/simpleCooperativeGroups/simpleCooperativeGroups.cu


---
## Usage Examples
This is a CUDA source file. Typical usage involves:
1. Compiling with nvcc (NVIDIA CUDA Compiler)
2. Linking with CUDA runtime libraries
3. Executing on NVIDIA GPU hardware


---
## Performance & Security Notes
### Performance Considerations
- CUDA kernel execution is asynchronous
- Memory transfers between host and device can be a bottleneck
- Thread block and grid dimensions affect performance

### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

