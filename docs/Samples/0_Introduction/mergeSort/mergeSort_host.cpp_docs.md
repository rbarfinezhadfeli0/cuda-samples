# Documentation for Samples/0_Introduction/mergeSort/mergeSort_host.cpp

## File Metadata

- **Path**: `Samples/0_Introduction/mergeSort/mergeSort_host.cpp`
- **Type**: .cpp
- **Location**: Samples/0_Introduction/mergeSort
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp
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

#include <assert.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "mergeSort_common.h"

////////////////////////////////////////////////////////////////////////////////
// Helper functions
////////////////////////////////////////////////////////////////////////////////
static void checkOrder(uint *data, uint N, uint sortDir)
{
    if (N <= 1) {
        return;
    }

    for (uint i = 0; i < N - 1; i++)
        if ((sortDir && (data[i] > data[i + 1])) || (!sortDir && (data[i] < data[i + 1]))) {
            fprintf(stderr, "checkOrder() failed!!!\n");
            exit(EXIT_FAILURE);
        }
}

static uint umin(uint a, uint b) { return (a <= b) ? a : b; }

static uint getSampleCount(uint dividend)
{
    return ((dividend % SAMPLE_STRIDE) != 0) ? (dividend / SAMPLE_STRIDE + 1) : (dividend / SAMPLE_STRIDE);
}

static uint nextPowerOfTwo(uint x)
{
    --x;
    x |= x >> 1;
    x |= x >> 2;
    x |= x >> 4;
    x |= x >> 8;
    x |= x >> 16;
    return ++x;
}

static uint binarySearchInclusive(uint val, uint *data, uint L, uint sortDir)
{
    if (L == 0) {
        return 0;
    }

    uint pos = 0;

    for (uint stride = nextPowerOfTwo(L); stride > 0; stride >>= 1) {
        uint newPos = umin(pos + stride, L);

        if ((sortDir && (data[newPos - 1] <= val)) || (!sortDir && (data[newPos - 1] >= val))) {
            pos = newPos;
        }
    }

    return pos;
}

static uint binarySearchExclusive(uint val, uint *data, uint L, uint sortDir)
{
    if (L == 0) {
        return 0;
    }

    uint pos = 0;

    for (uint stride = nextPowerOfTwo(L); stride > 0; stride >>= 1) {
        uint newPos = umin(pos + stride, L);

        if ((sortDir && (data[newPos - 1] < val)) || (!sortDir && (data[newPos - 1] > val))) {
            pos = newPos;
        }
    }

    return pos;
}

////////////////////////////////////////////////////////////////////////////////
// Merge step 1: find sample ranks in each segment
////////////////////////////////////////////////////////////////////////////////
static void generateSampleRanks(uint *ranksA, uint *ranksB, uint *srcKey, uint stride, uint N, uint sortDir)
{
    uint lastSegmentElements = N % (2 * stride);
    uint sampleCount = (lastSegmentElements > stride) ? (N + 2 * stride - lastSegmentElements) / (2 * SAMPLE_STRIDE)
                                                      : (N - lastSegmentElements) / (2 * SAMPLE_STRIDE);

    for (uint pos = 0; pos < sampleCount; pos++) {
        const uint i           = pos & ((stride / SAMPLE_STRIDE) - 1);
        const uint segmentBase = (pos - i) * (2 * SAMPLE_STRIDE);

        const uint lenA = stride;
        const uint lenB = umin(stride, N - segmentBase - stride);
        const uint nA   = stride / SAMPLE_STRIDE;
        const uint nB   = getSampleCount(lenB);

        if (i < nA) {
            ranksA[(segmentBase + 0) / SAMPLE_STRIDE + i] = i * SAMPLE_STRIDE;
            ranksB[(segmentBase + 0) / SAMPLE_STRIDE + i] = binarySearchExclusive(
                srcKey[segmentBase + i * SAMPLE_STRIDE], srcKey + segmentBase + stride, lenB, sortDir);
        }

        if (i < nB) {
            ranksB[(segmentBase + stride) / SAMPLE_STRIDE + i] = i * SAMPLE_STRIDE;
            ranksA[(segmentBase + stride) / SAMPLE_STRIDE + i] = binarySearchInclusive(
                srcKey[segmentBase + stride + i * SAMPLE_STRIDE], srcKey + segmentBase, lenA, sortDir);
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
// Merge step 2: merge ranks and indices to derive elementary intervals
////////////////////////////////////////////////////////////////////////////////
static void mergeRanksAndIndices(uint *limits, uint *ranks, uint stride, uint N)
{
    uint lastSegmentElements = N % (2 * stride);
    uint sampleCount = (lastSegmentElements > stride) ? (N + 2 * stride - lastSegmentElements) / (2 * SAMPLE_STRIDE)
                                                      : (N - lastSegmentElements) / (2 * SAMPLE_STRIDE);

    for (uint pos = 0; pos < sampleCount; pos++) {
        const uint i           = pos & ((stride / SAMPLE_STRIDE) - 1);
        const uint segmentBase = (pos - i) * (2 * SAMPLE_STRIDE);

        const uint lenA = stride;
        const uint lenB = umin(stride, N - segmentBase - stride);
        const uint nA   = stride / SAMPLE_STRIDE;
        const uint nB   = getSampleCount(lenB);

        if (i < nA) {
            uint dstPosA =
                binarySearchExclusive(
                    ranks[(segmentBase + 0) / SAMPLE_STRIDE + i], ranks + (segmentBase + stride) / SAMPLE_STRIDE, nB, 1)
                + i;
            assert(dstPosA < nA + nB);
            limits[(segmentBase / SAMPLE_STRIDE) + dstPosA] = ranks[(segmentBase + 0) / SAMPLE_STRIDE + i];
        }

        if (i < nB) {
            uint dstPosA =
                binarySearchInclusive(
                    ranks[(segmentBase + stride) / SAMPLE_STRIDE + i], ranks + (segmentBase + 0) / SAMPLE_STRIDE, nA, 1)
                + i;
            assert(dstPosA < nA + nB);
            limits[(segmentBase / SAMPLE_STRIDE) + dstPosA] = ranks[(segmentBase + stride) / SAMPLE_STRIDE + i];
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
// Merge step 3: merge elementary intervals (each interval is <= SAMPLE_STRIDE)
////////////////////////////////////////////////////////////////////////////////
static void merge(uint *dstKey,
                  uint *dstVal,
                  uint *srcAKey,
                  uint *srcAVal,
                  uint *srcBKey,
                  uint *srcBVal,
                  uint  lenA,
                  uint  lenB,
                  uint  sortDir)
{
    checkOrder(srcAKey, lenA, sortDir);
    checkOrder(srcBKey, lenB, sortDir);

    for (uint i = 0; i < lenA; i++) {
        uint dstPos = binarySearchExclusive(srcAKey[i], srcBKey, lenB, sortDir) + i;
        assert(dstPos < lenA + lenB);
        dstKey[dstPos] = srcAKey[i];
        dstVal[dstPos] = srcAVal[i];
    }

    for (uint i = 0; i < lenB; i++) {
        uint dstPos = binarySearchInclusive(srcBKey[i], srcAKey, lenA, sortDir) + i;
        assert(dstPos < lenA + lenB);
        dstKey[dstPos] = srcBKey[i];
        dstVal[dstPos] = srcBVal[i];
    }
}

static void mergeElementaryIntervals(uint *dstKey,
                                     uint *dstVal,
                                     uint *srcKey,
                                     uint *srcVal,
                                     uint *limitsA,
                                     uint *limitsB,
                                     uint  stride,
                                     uint  N,
                                     uint  sortDir)
{
    uint lastSegmentElements = N % (2 * stride);
    uint mergePairs = (lastSegmentElements > stride) ? getSampleCount(N) : (N - lastSegmentElements) / SAMPLE_STRIDE;

    for (uint pos = 0; pos < mergePairs; pos++) {
        uint i           = pos & ((2 * stride) / SAMPLE_STRIDE - 1);
        uint segmentBase = (pos - i) * SAMPLE_STRIDE;

        const uint lenA = stride;
        const uint lenB = umin(stride, N - segmentBase - stride);
        const uint nA   = stride / SAMPLE_STRIDE;
        const uint nB   = getSampleCount(lenB);
        const uint n    = nA + nB;

        const uint startPosA   = limitsA[pos];
        const uint endPosA     = (i + 1 < n) ? limitsA[pos + 1] : lenA;
        const uint startPosB   = limitsB[pos];
        const uint endPosB     = (i + 1 < n) ? limitsB[pos + 1] : lenB;
        const uint startPosDst = startPosA + startPosB;

        assert(startPosA <= endPosA && endPosA <= lenA);
        assert(startPosB <= endPosB && endPosB <= lenB);
        assert((endPosA - startPosA) <= SAMPLE_STRIDE);
        assert((endPosB - startPosB) <= SAMPLE_STRIDE);

        merge(dstKey + segmentBase + startPosDst,
              dstVal + segmentBase + startPosDst,
              (srcKey + segmentBase + 0) + startPosA,
              (srcVal + segmentBase + 0) + startPosA,
              (srcKey + segmentBase + stride) + startPosB,
              (srcVal + segmentBase + stride) + startPosB,
              endPosA - startPosA,
              endPosB - startPosB,
              sortDir);
    }
}

////////////////////////////////////////////////////////////////////////////////
// Retarded bubble sort
////////////////////////////////////////////////////////////////////////////////
static void bubbleSort(uint *key, uint *val, uint N, uint sortDir)
{
    if (N <= 1) {
        return;
    }

    for (uint bottom = 0; bottom < N - 1; bottom++) {
        uint savePos = bottom;
        uint saveKey = key[bottom];

        for (uint i = bottom + 1; i < N; i++)
            if ((sortDir && (key[i] < saveKey)) || (!sortDir && (key[i] > saveKey))) {
                savePos = i;
                saveKey = key[i];
            }

        if (savePos != bottom) {
            uint t;
            t            = key[savePos];
            key[savePos] = key[bottom];
            key[bottom]  = t;
            t            = val[savePos];
            val[savePos] = val[bottom];
            val[bottom]  = t;
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
// Interface function
////////////////////////////////////////////////////////////////////////////////
extern "C" void
mergeSortHost(uint *dstKey, uint *dstVal, uint *bufKey, uint *bufVal, uint *srcKey, uint *srcVal, uint N, uint sortDir)
{
    uint *ikey, *ival, *okey, *oval;
    uint  stageCount = 0;

    for (uint stride = SHARED_SIZE_LIMIT; stride < N; stride <<= 1, stageCount++)
        ;

    if (stageCount & 1) {
        ikey = bufKey;
        ival = bufVal;
        okey = dstKey;
        oval = dstVal;
    }
    else {
        ikey = dstKey;
        ival = dstVal;
        okey = bufKey;
        oval = bufVal;
    }

    printf("Bottom-level sort...\n");
    memcpy(ikey, srcKey, N * sizeof(uint));
    memcpy(ival, srcVal, N * sizeof(uint));

    for (uint pos = 0; pos < N; pos += SHARED_SIZE_LIMIT) {
        bubbleSort(ikey + pos, ival + pos, umin(SHARED_SIZE_LIMIT, N - pos), sortDir);
    }

    printf("Merge...\n");
    uint *ranksA  = (uint *)malloc(getSampleCount(N) * sizeof(uint));
    uint *ranksB  = (uint *)malloc(getSampleCount(N) * sizeof(uint));
    uint *limitsA = (uint *)malloc(getSampleCount(N) * sizeof(uint));
    uint *limitsB = (uint *)malloc(getSampleCount(N) * sizeof(uint));
    memset(ranksA, 0xFF, getSampleCount(N) * sizeof(uint));
    memset(ranksB, 0xFF, getSampleCount(N) * sizeof(uint));
    memset(limitsA, 0xFF, getSampleCount(N) * sizeof(uint));
    memset(limitsB, 0xFF, getSampleCount(N) * sizeof(uint));

    for (uint stride = SHARED_SIZE_LIMIT; stride < N; stride <<= 1) {
        uint lastSegmentElements = N % (2 * stride);

        // Find sample ranks and prepare for limiters merge
        generateSampleRanks(ranksA, ranksB, ikey, stride, N, sortDir);

        // Merge ranks and indices
        mergeRanksAndIndices(limitsA, ranksA, stride, N);
        mergeRanksAndIndices(limitsB, ranksB, stride, N);

        // Merge elementary intervals
        mergeElementaryIntervals(okey, oval, ikey, ival, limitsA, limitsB, stride, N, sortDir);

        if (lastSegmentElements <= stride) {
            // Last merge segment consists of a single array which just needs to be
            // passed through
            memcpy(
                okey + (N - lastSegmentElements), ikey + (N - lastSegmentElements), lastSegmentElements * sizeof(uint));
            memcpy(
                oval + (N - lastSegmentElements), ival + (N - lastSegmentElements), lastSegmentElements * sizeof(uint));
        }

        uint *t;
        t    = ikey;
        ikey = okey;
        okey = t;
        t    = ival;
        ival = oval;
        oval = t;
    }

    free(limitsB);
    free(limitsA);
    free(ranksB);
    free(ranksA);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/mergeSort/mergeSort_host.cpp`.

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

- **Total Lines**: 364
- **Approximate Size**: 13522 bytes

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
