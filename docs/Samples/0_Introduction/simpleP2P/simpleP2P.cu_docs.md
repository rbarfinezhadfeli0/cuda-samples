# Documentation for Samples/0_Introduction/simpleP2P/simpleP2P.cu

## File Metadata

- **Path**: `Samples/0_Introduction/simpleP2P/simpleP2P.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/simpleP2P
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
 * This sample demonstrates a combination of Peer-to-Peer (P2P) and
 * Unified Virtual Address Space (UVA) features new to SDK 4.0
 */

// includes, system
#include <stdio.h>
#include <stdlib.h>

// CUDA includes
#include <cuda_runtime.h>

// includes, project
#include <helper_cuda.h>
#include <helper_functions.h> // helper for shared that are common to CUDA Samples

__global__ void SimpleKernel(float *src, float *dst)
{
    // Just a dummy kernel, doing enough for us to verify that everything
    // worked
    const int idx = blockIdx.x * blockDim.x + threadIdx.x;
    dst[idx]      = src[idx] * 2.0f;
}

inline bool IsAppBuiltAs64() { return sizeof(void *) == 8; }

int main(int argc, char **argv)
{
    printf("[%s] - Starting...\n", argv[0]);

    if (!IsAppBuiltAs64()) {
        printf("%s is only supported with on 64-bit OSs and the application must be "
               "built as a 64-bit target.  Test is being waived.\n",
               argv[0]);
        exit(EXIT_WAIVED);
    }

    // Number of GPUs
    printf("Checking for multiple GPUs...\n");
    int gpu_n;
    checkCudaErrors(cudaGetDeviceCount(&gpu_n));
    printf("CUDA-capable device count: %i\n", gpu_n);

    if (gpu_n < 2) {
        printf("Two or more GPUs with Peer-to-Peer access capability are required for "
               "%s.\n",
               argv[0]);
        printf("Waiving test.\n");
        exit(EXIT_WAIVED);
    }

    // Query device properties
    cudaDeviceProp prop[64];
    int            gpuid[2]; // we want to find the first two GPU's that can support P2P

    for (int i = 0; i < gpu_n; i++) {
        checkCudaErrors(cudaGetDeviceProperties(&prop[i], i));
    }
    // Check possibility for peer access
    printf("\nChecking GPU(s) for support of peer to peer memory access...\n");

    int can_access_peer;
    int p2pCapableGPUs[2]; // We take only 1 pair of P2P capable GPUs
    p2pCapableGPUs[0] = p2pCapableGPUs[1] = -1;

    // Show all the combinations of supported P2P GPUs
    for (int i = 0; i < gpu_n; i++) {
        for (int j = 0; j < gpu_n; j++) {
            if (i == j) {
                continue;
            }
            checkCudaErrors(cudaDeviceCanAccessPeer(&can_access_peer, i, j));
            printf("> Peer access from %s (GPU%d) -> %s (GPU%d) : %s\n",
                   prop[i].name,
                   i,
                   prop[j].name,
                   j,
                   can_access_peer ? "Yes" : "No");
            if (can_access_peer && p2pCapableGPUs[0] == -1) {
                p2pCapableGPUs[0] = i;
                p2pCapableGPUs[1] = j;
            }
        }
    }

    if (p2pCapableGPUs[0] == -1 || p2pCapableGPUs[1] == -1) {
        printf("Two or more GPUs with Peer-to-Peer access capability are required for "
               "%s.\n",
               argv[0]);
        printf("Peer to Peer access is not available amongst GPUs in the system, "
               "waiving test.\n");

        exit(EXIT_WAIVED);
    }

    // Use first pair of p2p capable GPUs detected.
    gpuid[0] = p2pCapableGPUs[0];
    gpuid[1] = p2pCapableGPUs[1];

    // Enable peer access
    printf("Enabling peer access between GPU%d and GPU%d...\n", gpuid[0], gpuid[1]);
    checkCudaErrors(cudaSetDevice(gpuid[0]));
    checkCudaErrors(cudaDeviceEnablePeerAccess(gpuid[1], 0));
    checkCudaErrors(cudaSetDevice(gpuid[1]));
    checkCudaErrors(cudaDeviceEnablePeerAccess(gpuid[0], 0));

    // Allocate buffers
    const size_t buf_size = 1024 * 1024 * 16 * sizeof(float);
    printf(
        "Allocating buffers (%iMB on GPU%d, GPU%d and CPU Host)...\n", int(buf_size / 1024 / 1024), gpuid[0], gpuid[1]);
    checkCudaErrors(cudaSetDevice(gpuid[0]));
    float *g0;
    checkCudaErrors(cudaMalloc(&g0, buf_size));
    checkCudaErrors(cudaSetDevice(gpuid[1]));
    float *g1;
    checkCudaErrors(cudaMalloc(&g1, buf_size));
    float *h0;
    checkCudaErrors(cudaMallocHost(&h0, buf_size)); // Automatically portable with UVA

    // Create CUDA event handles
    printf("Creating event handles...\n");
    cudaEvent_t start_event, stop_event;
    float       time_memcpy;
    int         eventflags = cudaEventBlockingSync;
    checkCudaErrors(cudaEventCreateWithFlags(&start_event, eventflags));
    checkCudaErrors(cudaEventCreateWithFlags(&stop_event, eventflags));

    // P2P memcopy() benchmark
    checkCudaErrors(cudaEventRecord(start_event, 0));

    for (int i = 0; i < 100; i++) {
        // With UVA we don't need to specify source and target devices, the
        // runtime figures this out by itself from the pointers
        // Ping-pong copy between GPUs
        if (i % 2 == 0) {
            checkCudaErrors(cudaMemcpy(g1, g0, buf_size, cudaMemcpyDefault));
        }
        else {
            checkCudaErrors(cudaMemcpy(g0, g1, buf_size, cudaMemcpyDefault));
        }
    }

    checkCudaErrors(cudaEventRecord(stop_event, 0));
    checkCudaErrors(cudaEventSynchronize(stop_event));
    checkCudaErrors(cudaEventElapsedTime(&time_memcpy, start_event, stop_event));
    printf("cudaMemcpyPeer / cudaMemcpy between GPU%d and GPU%d: %.2fGB/s\n",
           gpuid[0],
           gpuid[1],
           (1.0f / (time_memcpy / 1000.0f)) * ((100.0f * buf_size)) / 1024.0f / 1024.0f / 1024.0f);

    // Prepare host buffer and copy to GPU 0
    printf("Preparing host buffer and memcpy to GPU%d...\n", gpuid[0]);

    for (int i = 0; i < buf_size / sizeof(float); i++) {
        h0[i] = float(i % 4096);
    }

    checkCudaErrors(cudaSetDevice(gpuid[0]));
    checkCudaErrors(cudaMemcpy(g0, h0, buf_size, cudaMemcpyDefault));

    // Kernel launch configuration
    const dim3 threads(512, 1);
    const dim3 blocks((buf_size / sizeof(float)) / threads.x, 1);

    // Run kernel on GPU 1, reading input from the GPU 0 buffer, writing
    // output to the GPU 1 buffer
    printf("Run kernel on GPU%d, taking source data from GPU%d and writing to "
           "GPU%d...\n",
           gpuid[1],
           gpuid[0],
           gpuid[1]);
    checkCudaErrors(cudaSetDevice(gpuid[1]));
    SimpleKernel<<<blocks, threads>>>(g0, g1);

    checkCudaErrors(cudaDeviceSynchronize());

    // Run kernel on GPU 0, reading input from the GPU 1 buffer, writing
    // output to the GPU 0 buffer
    printf("Run kernel on GPU%d, taking source data from GPU%d and writing to "
           "GPU%d...\n",
           gpuid[0],
           gpuid[1],
           gpuid[0]);
    checkCudaErrors(cudaSetDevice(gpuid[0]));
    SimpleKernel<<<blocks, threads>>>(g1, g0);

    checkCudaErrors(cudaDeviceSynchronize());

    // Copy data back to host and verify
    printf("Copy data back to host from GPU%d and verify results...\n", gpuid[0]);
    checkCudaErrors(cudaMemcpy(h0, g0, buf_size, cudaMemcpyDefault));

    int error_count = 0;

    for (int i = 0; i < buf_size / sizeof(float); i++) {
        // Re-generate input data and apply 2x '* 2.0f' computation of both
        // kernel runs
        if (h0[i] != float(i % 4096) * 2.0f * 2.0f) {
            printf("Verification error @ element %i: val = %f, ref = %f\n", i, h0[i], (float(i % 4096) * 2.0f * 2.0f));

            if (error_count++ > 10) {
                break;
            }
        }
    }

    // Disable peer access (also unregisters memory for non-UVA cases)
    printf("Disabling peer access...\n");
    checkCudaErrors(cudaSetDevice(gpuid[0]));
    checkCudaErrors(cudaDeviceDisablePeerAccess(gpuid[1]));
    checkCudaErrors(cudaSetDevice(gpuid[1]));
    checkCudaErrors(cudaDeviceDisablePeerAccess(gpuid[0]));

    // Cleanup and shutdown
    printf("Shutting down...\n");
    checkCudaErrors(cudaEventDestroy(start_event));
    checkCudaErrors(cudaEventDestroy(stop_event));
    checkCudaErrors(cudaSetDevice(gpuid[0]));
    checkCudaErrors(cudaFree(g0));
    checkCudaErrors(cudaSetDevice(gpuid[1]));
    checkCudaErrors(cudaFree(g1));
    checkCudaErrors(cudaFreeHost(h0));

    for (int i = 0; i < gpu_n; i++) {
        checkCudaErrors(cudaSetDevice(i));
    }

    if (error_count != 0) {
        printf("Test failed!\n");
        exit(EXIT_FAILURE);
    }
    else {
        printf("Test passed\n");
        exit(EXIT_SUCCESS);
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/simpleP2P/simpleP2P.cu`.

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

- **Total Lines**: 264
- **Approximate Size**: 9708 bytes

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
