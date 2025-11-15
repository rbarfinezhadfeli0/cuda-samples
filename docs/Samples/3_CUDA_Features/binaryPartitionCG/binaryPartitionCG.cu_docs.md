# Documentation for Samples/3_CUDA_Features/binaryPartitionCG/binaryPartitionCG.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/binaryPartitionCG/binaryPartitionCG.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/binaryPartitionCG
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

/*
 * This sample illustrates basic usage of binary partition cooperative groups
 * within the thread block tile when divergent path exists.
 * 1.) Each thread loads a value from random array.
 * 2.) then checks if it is odd or even.
 * 3.) create binary partition group based on the above predicate
 * 4.) we count the number of odd/even in the group based on size of the binary
       groups
 * 5.) write it global counter of odd.
 * 6.) sum the values loaded by individual threads(using reduce) and write it to
       global even & odd elements sum.
 *
 * **NOTE** :
 *    binary_partition results in splitting warp into divergent thread groups
 *    this is not good from performance perspective, but in cases where warp
 *    divergence is inevitable one can use binary_partition group.
*/

#include <cooperative_groups.h>
#include <cooperative_groups/reduce.h>
#include <helper_cuda.h>
#include <stdio.h>

namespace cg = cooperative_groups;

void initOddEvenArr(int *inputArr, unsigned int size)
{
    for (int i = 0; i < size; i++) {
        inputArr[i] = rand() % 50;
    }
}

/**
 * CUDA kernel device code
 *
 * Creates cooperative groups and performs odd/even counting & summation.
 */
__global__ void oddEvenCountAndSumCG(int *inputArr, int *numOfOdds, int *sumOfOddAndEvens, unsigned int size)
{
    cg::thread_block          cta    = cg::this_thread_block();
    cg::grid_group            grid   = cg::this_grid();
    cg::thread_block_tile<32> tile32 = cg::tiled_partition<32>(cta);

    for (int i = grid.thread_rank(); i < size; i += grid.size()) {
        int  elem    = inputArr[i];
        auto subTile = cg::binary_partition(tile32, elem & 1);
        if (elem & 1) // Odd numbers group
        {
            int oddGroupSum = cg::reduce(subTile, elem, cg::plus<int>());

            if (subTile.thread_rank() == 0) {
                // Add number of odds present in this group of Odds.
                atomicAdd(numOfOdds, subTile.size());

                // Add local reduction of odds present in this group of Odds.
                atomicAdd(&sumOfOddAndEvens[0], oddGroupSum);
            }
        }
        else // Even numbers group
        {
            int evenGroupSum = cg::reduce(subTile, elem, cg::plus<int>());

            if (subTile.thread_rank() == 0) {
                // Add local reduction of even present in this group of evens.
                atomicAdd(&sumOfOddAndEvens[1], evenGroupSum);
            }
        }
        // reconverge warp so for next loop iteration we ensure convergence of
        // above diverged threads to perform coalesced loads of inputArr.
        cg::sync(tile32);
    }
}

/**
 * Host main routine
 */
int main(int argc, const char **argv)
{
    int          deviceId = findCudaDevice(argc, argv);
    int         *h_inputArr, *d_inputArr;
    int         *h_numOfOdds, *d_numOfOdds;
    int         *h_sumOfOddEvenElems, *d_sumOfOddEvenElems;
    unsigned int arrSize = 1024 * 100;

    checkCudaErrors(cudaMallocHost(&h_inputArr, sizeof(int) * arrSize));
    checkCudaErrors(cudaMallocHost(&h_numOfOdds, sizeof(int)));
    checkCudaErrors(cudaMallocHost(&h_sumOfOddEvenElems, sizeof(int) * 2));
    initOddEvenArr(h_inputArr, arrSize);

    cudaStream_t stream;
    checkCudaErrors(cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking));
    checkCudaErrors(cudaMalloc(&d_inputArr, sizeof(int) * arrSize));
    checkCudaErrors(cudaMalloc(&d_numOfOdds, sizeof(int)));
    checkCudaErrors(cudaMalloc(&d_sumOfOddEvenElems, sizeof(int) * 2));

    checkCudaErrors(cudaMemcpyAsync(d_inputArr, h_inputArr, sizeof(int) * arrSize, cudaMemcpyHostToDevice, stream));
    checkCudaErrors(cudaMemsetAsync(d_numOfOdds, 0, sizeof(int), stream));
    checkCudaErrors(cudaMemsetAsync(d_sumOfOddEvenElems, 0, 2 * sizeof(int), stream));

    // Launch the kernel
    int threadsPerBlock = 0;
    int blocksPerGrid   = 0;
    checkCudaErrors(cudaOccupancyMaxPotentialBlockSize(&blocksPerGrid, &threadsPerBlock, oddEvenCountAndSumCG, 0, 0));

    printf("\nLaunching %d blocks with %d threads...\n\n", blocksPerGrid, threadsPerBlock);

    oddEvenCountAndSumCG<<<blocksPerGrid, threadsPerBlock, 0, stream>>>(
        d_inputArr, d_numOfOdds, d_sumOfOddEvenElems, arrSize);

    checkCudaErrors(cudaMemcpyAsync(h_numOfOdds, d_numOfOdds, sizeof(int), cudaMemcpyDeviceToHost, stream));
    checkCudaErrors(
        cudaMemcpyAsync(h_sumOfOddEvenElems, d_sumOfOddEvenElems, 2 * sizeof(int), cudaMemcpyDeviceToHost, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));

    printf("Array size = %d Num of Odds = %d Sum of Odds = %d Sum of Evens %d\n",
           arrSize,
           h_numOfOdds[0],
           h_sumOfOddEvenElems[0],
           h_sumOfOddEvenElems[1]);
    printf("\n...Done.\n\n");

    checkCudaErrors(cudaFreeHost(h_inputArr));
    checkCudaErrors(cudaFreeHost(h_numOfOdds));
    checkCudaErrors(cudaFreeHost(h_sumOfOddEvenElems));

    checkCudaErrors(cudaFree(d_inputArr));
    checkCudaErrors(cudaFree(d_numOfOdds));
    checkCudaErrors(cudaFree(d_sumOfOddEvenElems));

    return EXIT_SUCCESS;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/binaryPartitionCG/binaryPartitionCG.cu`.

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

- **Total Lines**: 159
- **Approximate Size**: 6648 bytes

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
