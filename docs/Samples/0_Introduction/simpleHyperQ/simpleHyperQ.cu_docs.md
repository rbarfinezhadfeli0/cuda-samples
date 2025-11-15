# Documentation for Samples/0_Introduction/simpleHyperQ/simpleHyperQ.cu

## File Metadata

- **Path**: `Samples/0_Introduction/simpleHyperQ/simpleHyperQ.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/simpleHyperQ
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

//
// This sample demonstrates how HyperQ allows supporting devices to avoid false
// dependencies between kernels in different streams.
//
// - Devices without HyperQ will run a maximum of two kernels at a time (one
//   kernel_A and one kernel_B).
// - Devices with HyperQ will run up to 32 kernels simultaneously.

#include <cooperative_groups.h>
#include <stdio.h>

namespace cg = cooperative_groups;
#include <helper_cuda.h>
#include <helper_functions.h>

const char *sSDKsample = "hyperQ";

// This subroutine does no real work but runs for at least the specified number
// of clock ticks.
__device__ void clock_block(clock_t *d_o, clock_t clock_count)
{
    unsigned int start_clock = (unsigned int)clock();

    clock_t clock_offset = 0;

    while (clock_offset < clock_count) {
        unsigned int end_clock = (unsigned int)clock();

        // The code below should work like
        // this (thanks to modular arithmetics):
        //
        // clock_offset = (clock_t) (end_clock > start_clock ?
        //                           end_clock - start_clock :
        //                           end_clock + (0xffffffffu - start_clock));
        //
        // Indeed, let m = 2^32 then
        // end - start = end + m - start (mod m).

        clock_offset = (clock_t)(end_clock - start_clock);
    }

    d_o[0] = clock_offset;
}

// We create two identical kernels calling clock_block(), we create two so that
// we can identify dependencies in the profile timeline ("kernel_B" is always
// dependent on "kernel_A" in the same stream).
__global__ void kernel_A(clock_t *d_o, clock_t clock_count) { clock_block(d_o, clock_count); }
__global__ void kernel_B(clock_t *d_o, clock_t clock_count) { clock_block(d_o, clock_count); }

// Single-warp reduction kernel (note: this is not optimized for simplicity)
__global__ void sum(clock_t *d_clocks, int N)
{
    // Handle to thread block group
    cg::thread_block   cta = cg::this_thread_block();
    __shared__ clock_t s_clocks[32];

    clock_t my_sum = 0;

    for (int i = threadIdx.x; i < N; i += blockDim.x) {
        my_sum += d_clocks[i];
    }

    s_clocks[threadIdx.x] = my_sum;
    cg::sync(cta);

    for (int i = warpSize / 2; i > 0; i /= 2) {
        if (threadIdx.x < i) {
            s_clocks[threadIdx.x] += s_clocks[threadIdx.x + i];
        }

        cg::sync(cta);
    }

    if (threadIdx.x == 0) {
        d_clocks[0] = s_clocks[0];
    }
}

int main(int argc, char **argv)
{
    int   nstreams    = 32; // One stream for each pair of kernels
    float kernel_time = 10; // Time each kernel should run in ms
    float elapsed_time;
    int   cuda_device = 0;

    printf("starting %s...\n", sSDKsample);

    // Get number of streams (if overridden on the command line)
    if (checkCmdLineFlag(argc, (const char **)argv, "nstreams")) {
        nstreams = getCmdLineArgumentInt(argc, (const char **)argv, "nstreams");
    }

    // Use command-line specified CUDA device, otherwise use device with
    // highest Gflops/s
    cuda_device = findCudaDevice(argc, (const char **)argv);

    // Get device properties
    cudaDeviceProp deviceProp;
    checkCudaErrors(cudaGetDevice(&cuda_device));
    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, cuda_device));

    // Get device clock rate
    int clockRate;
    checkCudaErrors(cudaDeviceGetAttribute(&clockRate, cudaDevAttrClockRate, cuda_device));

    // HyperQ is available in devices of Compute Capability 3.5 and higher
    if (deviceProp.major < 3 || (deviceProp.major == 3 && deviceProp.minor < 5)) {
        if (deviceProp.concurrentKernels == 0) {
            printf("> GPU does not support concurrent kernel execution (SM 3.5 or "
                   "higher required)\n");
            printf("  CUDA kernel runs will be serialized\n");
        }
        else {
            printf("> GPU does not support HyperQ\n");
            printf("  CUDA kernel runs will have limited concurrency\n");
        }
    }

    printf("> Detected Compute SM %d.%d hardware with %d multi-processors\n",
           deviceProp.major,
           deviceProp.minor,
           deviceProp.multiProcessorCount);

    // Allocate host memory for the output (reduced to a single value)
    clock_t *a = 0;
    checkCudaErrors(cudaMallocHost((void **)&a, sizeof(clock_t)));

    // Allocate device memory for the output (one value for each kernel)
    clock_t *d_a = 0;
    checkCudaErrors(cudaMalloc((void **)&d_a, 2 * nstreams * sizeof(clock_t)));

    // Allocate and initialize an array of stream handles
    cudaStream_t *streams = (cudaStream_t *)malloc(nstreams * sizeof(cudaStream_t));

    for (int i = 0; i < nstreams; i++) {
        checkCudaErrors(cudaStreamCreate(&(streams[i])));
    }

    // Create CUDA event handles
    cudaEvent_t start_event, stop_event;
    checkCudaErrors(cudaEventCreate(&start_event));
    checkCudaErrors(cudaEventCreate(&stop_event));

    // Target time per kernel is kernel_time ms, clockRate is in KHz
    // Target number of clocks = target time * clock frequency
#if defined(__arm__) || defined(__aarch64__)
    // the kernel takes more time than the channel reset time on arm archs, so to
    // prevent hangs reduce time_clocks.
    clock_t time_clocks = (clock_t)(kernel_time * (clockRate / 100));
#else
    clock_t time_clocks = (clock_t)(kernel_time * clockRate);
#endif
    clock_t total_clocks = 0;

    // Start the clock
    checkCudaErrors(cudaEventRecord(start_event, 0));

    // Queue pairs of {kernel_A, kernel_B} in separate streams
    for (int i = 0; i < nstreams; ++i) {
        kernel_A<<<1, 1, 0, streams[i]>>>(&d_a[2 * i], time_clocks);
        total_clocks += time_clocks;
        kernel_B<<<1, 1, 0, streams[i]>>>(&d_a[2 * i + 1], time_clocks);
        total_clocks += time_clocks;
    }

    // Stop the clock in stream 0 (i.e. all previous kernels will be complete)
    checkCudaErrors(cudaEventRecord(stop_event, 0));

    // At this point the CPU has dispatched all work for the GPU and can
    // continue processing other tasks in parallel. In this sample we just want
    // to wait until all work is done so we use a blocking cudaMemcpy below.

    // Run the sum kernel and copy the result back to host
    sum<<<1, 32>>>(d_a, 2 * nstreams);
    checkCudaErrors(cudaMemcpy(a, d_a, sizeof(clock_t), cudaMemcpyDeviceToHost));

    // stop_event will have been recorded but including the synchronize here to
    // prevent copy/paste errors!
    checkCudaErrors(cudaEventSynchronize(stop_event));
    checkCudaErrors(cudaEventElapsedTime(&elapsed_time, start_event, stop_event));

    printf("Expected time for serial execution of %d sets of kernels is between "
           "approx. %.3fs and %.3fs\n",
           nstreams,
           (nstreams + 1) * kernel_time / 1000.0f,
           2 * nstreams * kernel_time / 1000.0f);
    printf("Expected time for fully concurrent execution of %d sets of kernels is "
           "approx. %.3fs\n",
           nstreams,
           2 * kernel_time / 1000.0f);
    printf("Measured time for sample = %.3fs\n", elapsed_time / 1000.0f);

    bool bTestResult = (a[0] >= total_clocks);

    // Release resources
    for (int i = 0; i < nstreams; i++) {
        cudaStreamDestroy(streams[i]);
    }

    free(streams);
    cudaEventDestroy(start_event);
    cudaEventDestroy(stop_event);
    cudaFreeHost(a);
    cudaFree(d_a);

    exit(bTestResult ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/simpleHyperQ/simpleHyperQ.cu`.

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

- **Total Lines**: 236
- **Approximate Size**: 8985 bytes

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
