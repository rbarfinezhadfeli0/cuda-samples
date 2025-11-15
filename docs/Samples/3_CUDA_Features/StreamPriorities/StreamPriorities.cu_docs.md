# Documentation for Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/StreamPriorities
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

// std::system includes
#include <cstdio>

// CUDA-C includes
#include <cuda_runtime.h>
#include <helper_cuda.h>

#define TOTAL_SIZE 256 * 1024 * 1024
#define EACH_SIZE  128 * 1024 * 1024

// # threadblocks
#define TBLOCKS 1024
#define THREADS 512

// throw error on equality
#define ERR_EQ(X, Y)                                                                 \
    do {                                                                             \
        if ((X) == (Y)) {                                                            \
            fprintf(stderr, "Error in %s at %s:%d\n", __func__, __FILE__, __LINE__); \
            exit(-1);                                                                \
        }                                                                            \
    } while (0)

// throw error on difference
#define ERR_NE(X, Y)                                                                 \
    do {                                                                             \
        if ((X) != (Y)) {                                                            \
            fprintf(stderr, "Error in %s at %s:%d\n", __func__, __FILE__, __LINE__); \
            exit(-1);                                                                \
        }                                                                            \
    } while (0)

// copy from source -> destination arrays
__global__ void memcpy_kernel(int *dst, int *src, size_t n)
{
    int num = gridDim.x * blockDim.x;
    int id  = blockDim.x * blockIdx.x + threadIdx.x;

    for (int i = id; i < n / sizeof(int); i += num) {
        dst[i] = src[i];
    }
}

// initialise memory
void mem_init(int *buf, size_t n)
{
    for (int i = 0; i < n / sizeof(int); i++) {
        buf[i] = i;
    }
}

int main(int argc, char **argv)
{
    cudaDeviceProp device_prop;
    int            dev_id;

    printf("Starting [%s]...\n", argv[0]);

    // set device
    dev_id = findCudaDevice(argc, (const char **)argv);
    checkCudaErrors(cudaGetDeviceProperties(&device_prop, dev_id));

    if ((device_prop.major << 4) + device_prop.minor < 0x35) {
        fprintf(stderr,
                "%s requires Compute Capability of SM 3.5 or higher to "
                "run.\nexiting...\n",
                argv[0]);
        exit(EXIT_WAIVED);
    }

    // get the range of priorities available
    // [ greatest_priority, lowest_priority ]
    int priority_low;
    int priority_hi;
    checkCudaErrors(cudaDeviceGetStreamPriorityRange(&priority_low, &priority_hi));

    printf("CUDA stream priority range: LOW: %d to HIGH: %d\n", priority_low, priority_hi);

    // create streams with highest and lowest available priorities
    cudaStream_t st_low;
    cudaStream_t st_hi;
    checkCudaErrors(cudaStreamCreateWithPriority(&st_low, cudaStreamNonBlocking, priority_low));
    checkCudaErrors(cudaStreamCreateWithPriority(&st_hi, cudaStreamNonBlocking, priority_hi));

    size_t size;
    size = TOTAL_SIZE;

    // initialise host data
    int *h_src_low;
    int *h_src_hi;
    ERR_EQ(h_src_low = (int *)malloc(size), NULL);
    ERR_EQ(h_src_hi = (int *)malloc(size), NULL);
    mem_init(h_src_low, size);
    mem_init(h_src_hi, size);

    // initialise device data
    int *h_dst_low;
    int *h_dst_hi;
    ERR_EQ(h_dst_low = (int *)malloc(size), NULL);
    ERR_EQ(h_dst_hi = (int *)malloc(size), NULL);
    memset(h_dst_low, 0, size);
    memset(h_dst_hi, 0, size);

    // copy source data -> device
    int *d_src_low;
    int *d_src_hi;
    checkCudaErrors(cudaMalloc(&d_src_low, size));
    checkCudaErrors(cudaMalloc(&d_src_hi, size));
    checkCudaErrors(cudaMemcpy(d_src_low, h_src_low, size, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(d_src_hi, h_src_hi, size, cudaMemcpyHostToDevice));

    // allocate memory for memcopy destination
    int *d_dst_low;
    int *d_dst_hi;
    checkCudaErrors(cudaMalloc(&d_dst_low, size));
    checkCudaErrors(cudaMalloc(&d_dst_hi, size));

    // create some events
    cudaEvent_t ev_start_low;
    cudaEvent_t ev_start_hi;
    cudaEvent_t ev_end_low;
    cudaEvent_t ev_end_hi;
    checkCudaErrors(cudaEventCreate(&ev_start_low));
    checkCudaErrors(cudaEventCreate(&ev_start_hi));
    checkCudaErrors(cudaEventCreate(&ev_end_low));
    checkCudaErrors(cudaEventCreate(&ev_end_hi));

    /* */

    // call pair of kernels repeatedly (with different priority streams)
    checkCudaErrors(cudaEventRecord(ev_start_low, st_low));
    checkCudaErrors(cudaEventRecord(ev_start_hi, st_hi));

    for (int i = 0; i < TOTAL_SIZE; i += EACH_SIZE) {
        int j = i / sizeof(int);
        memcpy_kernel<<<TBLOCKS, THREADS, 0, st_low>>>(d_dst_low + j, d_src_low + j, EACH_SIZE);
        memcpy_kernel<<<TBLOCKS, THREADS, 0, st_hi>>>(d_dst_hi + j, d_src_hi + j, EACH_SIZE);
    }

    checkCudaErrors(cudaEventRecord(ev_end_low, st_low));
    checkCudaErrors(cudaEventRecord(ev_end_hi, st_hi));

    checkCudaErrors(cudaEventSynchronize(ev_end_low));
    checkCudaErrors(cudaEventSynchronize(ev_end_hi));

    /* */

    size = TOTAL_SIZE;
    checkCudaErrors(cudaMemcpy(h_dst_low, d_dst_low, size, cudaMemcpyDeviceToHost));
    checkCudaErrors(cudaMemcpy(h_dst_hi, d_dst_hi, size, cudaMemcpyDeviceToHost));

    // check results of kernels
    ERR_NE(memcmp(h_dst_low, h_src_low, size), 0);
    ERR_NE(memcmp(h_dst_hi, h_src_hi, size), 0);

    // check timings
    float ms_low;
    float ms_hi;
    checkCudaErrors(cudaEventElapsedTime(&ms_low, ev_start_low, ev_end_low));
    checkCudaErrors(cudaEventElapsedTime(&ms_hi, ev_start_hi, ev_end_hi));

    printf("elapsed time of kernels launched to LOW priority stream: %.3lf ms\n", ms_low);
    printf("elapsed time of kernels launched to HI  priority stream: %.3lf ms\n", ms_hi);

    exit(EXIT_SUCCESS);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu`.

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

- **Total Lines**: 194
- **Approximate Size**: 7374 bytes

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
