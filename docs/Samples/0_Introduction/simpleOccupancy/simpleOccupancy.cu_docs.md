# Documentation for Samples/0_Introduction/simpleOccupancy/simpleOccupancy.cu

## File Metadata

- **Path**: `Samples/0_Introduction/simpleOccupancy/simpleOccupancy.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/simpleOccupancy
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

#include <helper_cuda.h> // helper functions for CUDA error check
#include <iostream>

const int manualBlockSize = 32;

////////////////////////////////////////////////////////////////////////////////
// Test kernel
//
// This kernel squares each array element. Each thread addresses
// himself with threadIdx and blockIdx, so that it can handle any
// execution configuration, including anything the launch configurator
// API suggests.
////////////////////////////////////////////////////////////////////////////////
__global__ void square(int *array, int arrayCount)
{
    extern __shared__ int dynamicSmem[];
    int                   idx = threadIdx.x + blockIdx.x * blockDim.x;

    if (idx < arrayCount) {
        array[idx] *= array[idx];
    }
}

////////////////////////////////////////////////////////////////////////////////
// Potential occupancy calculator
//
// The potential occupancy is calculated according to the kernel and
// execution configuration the user desires. Occupancy is defined in
// terms of active blocks per multiprocessor, and the user can convert
// it to other metrics.
//
// This wrapper routine computes the occupancy of kernel, and reports
// it in terms of active warps / maximum warps per SM.
////////////////////////////////////////////////////////////////////////////////
static double reportPotentialOccupancy(void *kernel, int blockSize, size_t dynamicSMem)
{
    int            device;
    cudaDeviceProp prop;

    int numBlocks;
    int activeWarps;
    int maxWarps;

    double occupancy;

    checkCudaErrors(cudaGetDevice(&device));
    checkCudaErrors(cudaGetDeviceProperties(&prop, device));

    checkCudaErrors(cudaOccupancyMaxActiveBlocksPerMultiprocessor(&numBlocks, kernel, blockSize, dynamicSMem));

    activeWarps = numBlocks * blockSize / prop.warpSize;
    maxWarps    = prop.maxThreadsPerMultiProcessor / prop.warpSize;

    occupancy = (double)activeWarps / maxWarps;

    return occupancy;
}

////////////////////////////////////////////////////////////////////////////////
// Occupancy-based launch configurator
//
// The launch configurator, cudaOccupancyMaxPotentialBlockSize and
// cudaOccupancyMaxPotentialBlockSizeVariableSMem, suggests a block
// size that achieves the best theoretical occupancy. It also returns
// the minimum number of blocks needed to achieve the occupancy on the
// whole device.
//
// This launch configurator is purely occupancy-based. It doesn't
// translate directly to performance, but the suggestion should
// nevertheless be a good starting point for further optimizations.
//
// This function configures the launch based on the "automatic"
// argument, records the runtime, and reports occupancy and runtime.
////////////////////////////////////////////////////////////////////////////////
static int launchConfig(int *array, int arrayCount, bool automatic)
{
    int    blockSize;
    int    minGridSize;
    int    gridSize;
    size_t dynamicSMemUsage = 0;

    cudaEvent_t start;
    cudaEvent_t end;

    float elapsedTime;

    double potentialOccupancy;

    checkCudaErrors(cudaEventCreate(&start));
    checkCudaErrors(cudaEventCreate(&end));

    if (automatic) {
        checkCudaErrors(
            cudaOccupancyMaxPotentialBlockSize(&minGridSize, &blockSize, (void *)square, dynamicSMemUsage, arrayCount));

        std::cout << "Suggested block size: " << blockSize << std::endl
                  << "Minimum grid size for maximum occupancy: " << minGridSize << std::endl;
    }
    else {
        // This block size is too small. Given limited number of
        // active blocks per multiprocessor, the number of active
        // threads will be limited, and thus unable to achieve maximum
        // occupancy.
        //
        blockSize = manualBlockSize;
    }

    // Round up
    //
    gridSize = (arrayCount + blockSize - 1) / blockSize;

    // Launch and profile
    //
    checkCudaErrors(cudaEventRecord(start));
    square<<<gridSize, blockSize, dynamicSMemUsage>>>(array, arrayCount);
    checkCudaErrors(cudaEventRecord(end));

    checkCudaErrors(cudaDeviceSynchronize());

    // Calculate occupancy
    //
    potentialOccupancy = reportPotentialOccupancy((void *)square, blockSize, dynamicSMemUsage);

    std::cout << "Potential occupancy: " << potentialOccupancy * 100 << "%" << std::endl;

    // Report elapsed time
    //
    checkCudaErrors(cudaEventElapsedTime(&elapsedTime, start, end));
    std::cout << "Elapsed time: " << elapsedTime << "ms" << std::endl;

    return 0;
}

////////////////////////////////////////////////////////////////////////////////
// The test
//
// The test generates an array and squares it with a CUDA kernel, then
// verifies the result.
////////////////////////////////////////////////////////////////////////////////
static int test(bool automaticLaunchConfig, const int count = 1000000)
{
    int *array;
    int *dArray;
    int  size = count * sizeof(int);

    array = new int[count];

    for (int i = 0; i < count; i += 1) {
        array[i] = i;
    }

    checkCudaErrors(cudaMalloc(&dArray, size));
    checkCudaErrors(cudaMemcpy(dArray, array, size, cudaMemcpyHostToDevice));

    for (int i = 0; i < count; i += 1) {
        array[i] = 0;
    }

    launchConfig(dArray, count, automaticLaunchConfig);

    checkCudaErrors(cudaMemcpy(array, dArray, size, cudaMemcpyDeviceToHost));
    checkCudaErrors(cudaFree(dArray));

    // Verify the return data
    //
    for (int i = 0; i < count; i += 1) {
        if (array[i] != i * i) {
            std::cout << "element " << i << " expected " << i * i << " actual " << array[i] << std::endl;
            return 1;
        }
    }
    delete[] array;

    return 0;
}

////////////////////////////////////////////////////////////////////////////////
// Sample Main
//
// The sample runs the test with manually configured launch and
// automatically configured launch, and reports the occupancy and
// performance.
////////////////////////////////////////////////////////////////////////////////
int main()
{
    int status;

    std::cout << "starting Simple Occupancy" << std::endl << std::endl;

    std::cout << "[ Manual configuration with " << manualBlockSize << " threads per block ]" << std::endl;

    status = test(false);
    if (status) {
        std::cerr << "Test failed\n" << std::endl;
        return -1;
    }

    std::cout << std::endl;

    std::cout << "[ Automatic, occupancy-based configuration ]" << std::endl;
    status = test(true);
    if (status) {
        std::cerr << "Test failed\n" << std::endl;
        return -1;
    }

    std::cout << std::endl;
    std::cout << "Test PASSED\n" << std::endl;

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/simpleOccupancy/simpleOccupancy.cu`.

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

- **Total Lines**: 239
- **Approximate Size**: 8246 bytes

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
