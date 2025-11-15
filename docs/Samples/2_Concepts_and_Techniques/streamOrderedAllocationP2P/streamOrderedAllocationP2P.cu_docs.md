# Documentation for Samples/2_Concepts_and_Techniques/streamOrderedAllocationP2P/streamOrderedAllocationP2P.cu

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/streamOrderedAllocationP2P/streamOrderedAllocationP2P.cu`
- **Type**: .cu
- **Location**: Samples/2_Concepts_and_Techniques/streamOrderedAllocationP2P
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
 * This sample demonstrates peer-to-peer access of stream ordered memory
 * allocated with cudaMallocAsync and cudaMemPool family of APIs through simple
 * kernel which does peer-to-peer to access & scales vector elements.
 */

// System includes
#include <assert.h>
#include <iostream>
#include <map>
#include <set>
#include <stdio.h>
#include <utility>

// CUDA runtime
#include <cuda_runtime.h>

// helper functions and utilities to work with CUDA
#include <helper_cuda.h>
#include <helper_functions.h>

// Simple kernel to demonstrate copying cudaMallocAsync memory via P2P to peer
// device
__global__ void copyP2PAndScale(const int *src, int *dst, int N)
{
    int idx = blockIdx.x * blockDim.x + threadIdx.x;

    if (idx < N) {
        // scale & store src vector.
        dst[idx] = 2 * src[idx];
    }
}

// Map of device version to device number
std::multimap<std::pair<int, int>, int> getIdenticalGPUs()
{
    int numGpus = 0;
    checkCudaErrors(cudaGetDeviceCount(&numGpus));

    std::multimap<std::pair<int, int>, int> identicalGpus;

    for (int i = 0; i < numGpus; i++) {
        int isMemPoolSupported = 0;
        checkCudaErrors(cudaDeviceGetAttribute(&isMemPoolSupported, cudaDevAttrMemoryPoolsSupported, i));

        // Filter unsupported devices
        if (isMemPoolSupported) {
            int major = 0, minor = 0;
            checkCudaErrors(cudaDeviceGetAttribute(&major, cudaDevAttrComputeCapabilityMajor, i));
            checkCudaErrors(cudaDeviceGetAttribute(&minor, cudaDevAttrComputeCapabilityMinor, i));
            identicalGpus.emplace(std::make_pair(major, minor), i);
        }
    }

    return identicalGpus;
}

std::pair<int, int> getP2PCapableGpuPair()
{
    constexpr size_t kNumGpusRequired = 2;

    auto gpusByArch = getIdenticalGPUs();

    auto it  = gpusByArch.begin();
    auto end = gpusByArch.end();

    auto bestFit = std::make_pair(it, it);
    // use std::distance to find the largest number of GPUs amongst architectures
    auto distance = [](decltype(bestFit) p) { return std::distance(p.first, p.second); };

    // Read each unique key/pair element in order
    for (; it != end; it = gpusByArch.upper_bound(it->first)) {
        // first and second are iterators bounded within the architecture group
        auto testFit = gpusByArch.equal_range(it->first);
        // Always use devices with highest architecture version or whichever has the
        // most devices available
        if (distance(bestFit) <= distance(testFit))
            bestFit = testFit;
    }

    if (distance(bestFit) < kNumGpusRequired) {
        printf("No Two or more GPUs with same architecture capable of cuda Memory "
               "Pools found."
               "\nWaiving the sample\n");
        exit(EXIT_WAIVED);
    }

    std::set<int> bestFitDeviceIds;

    // check & select peer-to-peer access capable GPU devices.
    int devIds[2];
    for (auto itr = bestFit.first; itr != bestFit.second; itr++) {
        int deviceId = itr->second;
        checkCudaErrors(cudaSetDevice(deviceId));

        std::for_each(itr, bestFit.second, [&deviceId, &bestFitDeviceIds, &kNumGpusRequired](decltype(*itr) mapPair) {
            if (deviceId != mapPair.second) {
                int access = 0;
                checkCudaErrors(cudaDeviceCanAccessPeer(&access, deviceId, mapPair.second));
                printf("Device=%d %s Access Peer Device=%d\n", deviceId, access ? "CAN" : "CANNOT", mapPair.second);
                if (access && bestFitDeviceIds.size() < kNumGpusRequired) {
                    bestFitDeviceIds.emplace(deviceId);
                    bestFitDeviceIds.emplace(mapPair.second);
                }
                else {
                    printf("Ignoring device %i (max devices exceeded)\n", mapPair.second);
                }
            }
        });

        if (bestFitDeviceIds.size() >= kNumGpusRequired) {
            printf("Selected p2p capable devices - ");
            int i = 0;
            for (auto devicesItr = bestFitDeviceIds.begin(); devicesItr != bestFitDeviceIds.end(); devicesItr++) {
                devIds[i++] = *devicesItr;
                printf("deviceId = %d  ", *devicesItr);
            }
            printf("\n");
            break;
        }
    }

    // if bestFitDeviceIds.size() == 0 it means the GPUs in system are not p2p
    // capable, hence we add it without p2p capability check.
    if (!bestFitDeviceIds.size()) {
        printf("No Two or more Devices p2p capable found.. exiting..\n");
        exit(EXIT_WAIVED);
    }

    auto p2pGpuPair = std::make_pair(devIds[0], devIds[1]);

    return p2pGpuPair;
}

int memPoolP2PCopy()
{
    int          *dev0_srcVec, *dev1_dstVec; // Device buffers
    cudaStream_t  stream1, stream2;
    cudaMemPool_t memPool;
    cudaEvent_t   waitOnStream1;

    // Allocate CPU memory.
    size_t nelem = 1048576;
    size_t bytes = nelem * sizeof(int);

    int *a      = (int *)malloc(bytes);
    int *output = (int *)malloc(bytes);

    /* Initialize the vectors. */
    for (int n = 0; n < nelem; n++) {
        a[n] = rand() / (int)RAND_MAX;
    }

    auto p2pDevices = getP2PCapableGpuPair();
    printf("selected devices = %d & %d\n", p2pDevices.first, p2pDevices.second);
    checkCudaErrors(cudaSetDevice(p2pDevices.first));
    checkCudaErrors(cudaEventCreate(&waitOnStream1));

    checkCudaErrors(cudaStreamCreateWithFlags(&stream1, cudaStreamNonBlocking));

    // Get the default mempool for device p2pDevices.first from the pair
    checkCudaErrors(cudaDeviceGetDefaultMemPool(&memPool, p2pDevices.first));

    // Allocate memory in a stream from the pool set above.
    checkCudaErrors(cudaMallocAsync(&dev0_srcVec, bytes, stream1));

    checkCudaErrors(cudaMemcpyAsync(dev0_srcVec, a, bytes, cudaMemcpyHostToDevice, stream1));
    checkCudaErrors(cudaEventRecord(waitOnStream1, stream1));

    checkCudaErrors(cudaSetDevice(p2pDevices.second));
    checkCudaErrors(cudaStreamCreateWithFlags(&stream2, cudaStreamNonBlocking));

    // Allocate memory in p2pDevices.second device
    checkCudaErrors(cudaMallocAsync(&dev1_dstVec, bytes, stream2));

    // Setup peer mappings for p2pDevices.second device
    cudaMemAccessDesc desc;
    memset(&desc, 0, sizeof(cudaMemAccessDesc));
    desc.location.type = cudaMemLocationTypeDevice;
    desc.location.id   = p2pDevices.second;
    desc.flags         = cudaMemAccessFlagsProtReadWrite;
    checkCudaErrors(cudaMemPoolSetAccess(memPool, &desc, 1));

    printf("> copyP2PAndScale kernel running ...\n");
    dim3 block(256);
    dim3 grid((unsigned int)ceil(nelem / (int)block.x));
    checkCudaErrors(cudaStreamWaitEvent(stream2, waitOnStream1));
    copyP2PAndScale<<<grid, block, 0, stream2>>>(dev0_srcVec, dev1_dstVec, nelem);

    checkCudaErrors(cudaMemcpyAsync(output, dev1_dstVec, bytes, cudaMemcpyDeviceToHost, stream2));
    checkCudaErrors(cudaFreeAsync(dev0_srcVec, stream2));
    checkCudaErrors(cudaFreeAsync(dev1_dstVec, stream2));
    checkCudaErrors(cudaStreamSynchronize(stream2));

    /* Compare the results */
    printf("> Checking the results from copyP2PAndScale() ...\n");

    for (int n = 0; n < nelem; n++) {
        if ((2 * a[n]) != output[n]) {
            printf("mismatch i = %d expected = %d val = %d\n", n, 2 * a[n], output[n]);
            return EXIT_FAILURE;
        }
    }

    free(a);
    free(output);
    checkCudaErrors(cudaStreamDestroy(stream1));
    checkCudaErrors(cudaStreamDestroy(stream2));
    printf("PASSED\n");

    return EXIT_SUCCESS;
}

int main(int argc, char **argv)
{
    int ret = memPoolP2PCopy();
    return ret;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/streamOrderedAllocationP2P/streamOrderedAllocationP2P.cu`.

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

- **Total Lines**: 246
- **Approximate Size**: 9160 bytes

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
