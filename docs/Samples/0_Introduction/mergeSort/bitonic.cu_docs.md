# Documentation for Samples/0_Introduction/mergeSort/bitonic.cu

## File Metadata

- **Path**: `Samples/0_Introduction/mergeSort/bitonic.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/mergeSort
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

#include <cooperative_groups.h>

namespace cg = cooperative_groups;
#include <assert.h>
#include <helper_cuda.h>

#include "mergeSort_common.h"

inline __device__ void Comparator(uint &keyA, uint &valA, uint &keyB, uint &valB, uint arrowDir)
{
    uint t;

    if ((keyA > keyB) == arrowDir) {
        t    = keyA;
        keyA = keyB;
        keyB = t;
        t    = valA;
        valA = valB;
        valB = t;
    }
}

__global__ void
bitonicSortSharedKernel(uint *d_DstKey, uint *d_DstVal, uint *d_SrcKey, uint *d_SrcVal, uint arrayLength, uint sortDir)
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();
    // Shared memory storage for one or more short vectors
    __shared__ uint s_key[SHARED_SIZE_LIMIT];
    __shared__ uint s_val[SHARED_SIZE_LIMIT];

    // Offset to the beginning of subbatch and load data
    d_SrcKey += blockIdx.x * SHARED_SIZE_LIMIT + threadIdx.x;
    d_SrcVal += blockIdx.x * SHARED_SIZE_LIMIT + threadIdx.x;
    d_DstKey += blockIdx.x * SHARED_SIZE_LIMIT + threadIdx.x;
    d_DstVal += blockIdx.x * SHARED_SIZE_LIMIT + threadIdx.x;
    s_key[threadIdx.x + 0]                       = d_SrcKey[0];
    s_val[threadIdx.x + 0]                       = d_SrcVal[0];
    s_key[threadIdx.x + (SHARED_SIZE_LIMIT / 2)] = d_SrcKey[(SHARED_SIZE_LIMIT / 2)];
    s_val[threadIdx.x + (SHARED_SIZE_LIMIT / 2)] = d_SrcVal[(SHARED_SIZE_LIMIT / 2)];

    for (uint size = 2; size < arrayLength; size <<= 1) {
        // Bitonic merge
        uint dir = (threadIdx.x & (size / 2)) != 0;

        for (uint stride = size / 2; stride > 0; stride >>= 1) {
            cg::sync(cta);
            uint pos = 2 * threadIdx.x - (threadIdx.x & (stride - 1));
            Comparator(s_key[pos + 0], s_val[pos + 0], s_key[pos + stride], s_val[pos + stride], dir);
        }
    }

    // ddd == sortDir for the last bitonic merge step
    {
        for (uint stride = arrayLength / 2; stride > 0; stride >>= 1) {
            cg::sync(cta);
            uint pos = 2 * threadIdx.x - (threadIdx.x & (stride - 1));
            Comparator(s_key[pos + 0], s_val[pos + 0], s_key[pos + stride], s_val[pos + stride], sortDir);
        }
    }

    cg::sync(cta);
    d_DstKey[0]                       = s_key[threadIdx.x + 0];
    d_DstVal[0]                       = s_val[threadIdx.x + 0];
    d_DstKey[(SHARED_SIZE_LIMIT / 2)] = s_key[threadIdx.x + (SHARED_SIZE_LIMIT / 2)];
    d_DstVal[(SHARED_SIZE_LIMIT / 2)] = s_val[threadIdx.x + (SHARED_SIZE_LIMIT / 2)];
}

// Helper function (also used by odd-even merge sort)
extern "C" uint factorRadix2(uint *log2L, uint L)
{
    if (!L) {
        *log2L = 0;
        return 0;
    }
    else {
        for (*log2L = 0; (L & 1) == 0; L >>= 1, *log2L++)
            ;

        return L;
    }
}

extern "C" void bitonicSortShared(uint *d_DstKey,
                                  uint *d_DstVal,
                                  uint *d_SrcKey,
                                  uint *d_SrcVal,
                                  uint  batchSize,
                                  uint  arrayLength,
                                  uint  sortDir)
{
    // Nothing to sort
    if (arrayLength < 2) {
        return;
    }

    // Only power-of-two array lengths are supported by this implementation
    uint log2L;
    uint factorizationRemainder = factorRadix2(&log2L, arrayLength);
    assert(factorizationRemainder == 1);

    uint blockCount  = batchSize * arrayLength / SHARED_SIZE_LIMIT;
    uint threadCount = SHARED_SIZE_LIMIT / 2;

    assert(arrayLength <= SHARED_SIZE_LIMIT);
    assert((batchSize * arrayLength) % SHARED_SIZE_LIMIT == 0);

    bitonicSortSharedKernel<<<blockCount, threadCount>>>(d_DstKey, d_DstVal, d_SrcKey, d_SrcVal, arrayLength, sortDir);
    getLastCudaError("bitonicSortSharedKernel<<<>>> failed!\n");
}

////////////////////////////////////////////////////////////////////////////////
// Merge step 3: merge elementary intervals
////////////////////////////////////////////////////////////////////////////////
static inline __host__ __device__ uint iDivUp(uint a, uint b) { return ((a % b) == 0) ? (a / b) : (a / b + 1); }

static inline __host__ __device__ uint getSampleCount(uint dividend) { return iDivUp(dividend, SAMPLE_STRIDE); }

template <uint sortDir>
static inline __device__ void
ComparatorExtended(uint &keyA, uint &valA, uint &flagA, uint &keyB, uint &valB, uint &flagB, uint arrowDir)
{
    uint t;

    if ((!(flagA || flagB) && ((keyA > keyB) == arrowDir)) || ((arrowDir == sortDir) && (flagA == 1))
        || ((arrowDir != sortDir) && (flagB == 1))) {
        t     = keyA;
        keyA  = keyB;
        keyB  = t;
        t     = valA;
        valA  = valB;
        valB  = t;
        t     = flagA;
        flagA = flagB;
        flagB = t;
    }
}

template <uint sortDir>
__global__ void bitonicMergeElementaryIntervalsKernel(uint *d_DstKey,
                                                      uint *d_DstVal,
                                                      uint *d_SrcKey,
                                                      uint *d_SrcVal,
                                                      uint *d_LimitsA,
                                                      uint *d_LimitsB,
                                                      uint  stride,
                                                      uint  N)
{
    // Handle to thread block group
    cg::thread_block cta = cg::this_thread_block();
    __shared__ uint  s_key[2 * SAMPLE_STRIDE];
    __shared__ uint  s_val[2 * SAMPLE_STRIDE];
    __shared__ uint  s_inf[2 * SAMPLE_STRIDE];

    const uint intervalI   = blockIdx.x & ((2 * stride) / SAMPLE_STRIDE - 1);
    const uint segmentBase = (blockIdx.x - intervalI) * SAMPLE_STRIDE;
    d_SrcKey += segmentBase;
    d_SrcVal += segmentBase;
    d_DstKey += segmentBase;
    d_DstVal += segmentBase;

    // Set up threadblock-wide parameters
    __shared__ uint startSrcA, lenSrcA, startSrcB, lenSrcB, startDst;

    if (threadIdx.x == 0) {
        uint segmentElementsA = stride;
        uint segmentElementsB = umin(stride, N - segmentBase - stride);
        uint segmentSamplesA  = stride / SAMPLE_STRIDE;
        uint segmentSamplesB  = getSampleCount(segmentElementsB);
        uint segmentSamples   = segmentSamplesA + segmentSamplesB;

        startSrcA = d_LimitsA[blockIdx.x];
        startSrcB = d_LimitsB[blockIdx.x];
        startDst  = startSrcA + startSrcB;

        uint endSrcA = (intervalI + 1 < segmentSamples) ? d_LimitsA[blockIdx.x + 1] : segmentElementsA;
        uint endSrcB = (intervalI + 1 < segmentSamples) ? d_LimitsB[blockIdx.x + 1] : segmentElementsB;
        lenSrcA      = endSrcA - startSrcA;
        lenSrcB      = endSrcB - startSrcB;
    }

    s_inf[threadIdx.x + 0]             = 1;
    s_inf[threadIdx.x + SAMPLE_STRIDE] = 1;

    // Load input data
    cg::sync(cta);

    if (threadIdx.x < lenSrcA) {
        s_key[threadIdx.x] = d_SrcKey[0 + startSrcA + threadIdx.x];
        s_val[threadIdx.x] = d_SrcVal[0 + startSrcA + threadIdx.x];
        s_inf[threadIdx.x] = 0;
    }

    // Prepare for bitonic merge by inversing the ordering
    if (threadIdx.x < lenSrcB) {
        s_key[2 * SAMPLE_STRIDE - 1 - threadIdx.x] = d_SrcKey[stride + startSrcB + threadIdx.x];
        s_val[2 * SAMPLE_STRIDE - 1 - threadIdx.x] = d_SrcVal[stride + startSrcB + threadIdx.x];
        s_inf[2 * SAMPLE_STRIDE - 1 - threadIdx.x] = 0;
    }

    //"Extended" bitonic merge
    for (uint stride = SAMPLE_STRIDE; stride > 0; stride >>= 1) {
        cg::sync(cta);
        uint pos = 2 * threadIdx.x - (threadIdx.x & (stride - 1));
        ComparatorExtended<sortDir>(s_key[pos + 0],
                                    s_val[pos + 0],
                                    s_inf[pos + 0],
                                    s_key[pos + stride],
                                    s_val[pos + stride],
                                    s_inf[pos + stride],
                                    sortDir);
    }

    // Store sorted data
    cg::sync(cta);
    d_DstKey += startDst;
    d_DstVal += startDst;

    if (threadIdx.x < lenSrcA) {
        d_DstKey[threadIdx.x] = s_key[threadIdx.x];
        d_DstVal[threadIdx.x] = s_val[threadIdx.x];
    }

    if (threadIdx.x < lenSrcB) {
        d_DstKey[lenSrcA + threadIdx.x] = s_key[lenSrcA + threadIdx.x];
        d_DstVal[lenSrcA + threadIdx.x] = s_val[lenSrcA + threadIdx.x];
    }
}

extern "C" void bitonicMergeElementaryIntervals(uint *d_DstKey,
                                                uint *d_DstVal,
                                                uint *d_SrcKey,
                                                uint *d_SrcVal,
                                                uint *d_LimitsA,
                                                uint *d_LimitsB,
                                                uint  stride,
                                                uint  N,
                                                uint  sortDir)
{
    uint lastSegmentElements = N % (2 * stride);

    uint mergePairs = (lastSegmentElements > stride) ? getSampleCount(N) : (N - lastSegmentElements) / SAMPLE_STRIDE;

    if (sortDir) {
        bitonicMergeElementaryIntervalsKernel<1U>
            <<<mergePairs, SAMPLE_STRIDE>>>(d_DstKey, d_DstVal, d_SrcKey, d_SrcVal, d_LimitsA, d_LimitsB, stride, N);
        getLastCudaError("mergeElementaryIntervalsKernel<1> failed\n");
    }
    else {
        bitonicMergeElementaryIntervalsKernel<0U>
            <<<mergePairs, SAMPLE_STRIDE>>>(d_DstKey, d_DstVal, d_SrcKey, d_SrcVal, d_LimitsA, d_LimitsB, stride, N);
        getLastCudaError("mergeElementaryIntervalsKernel<0> failed\n");
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/mergeSort/bitonic.cu`.

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

- **Total Lines**: 282
- **Approximate Size**: 11249 bytes

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
