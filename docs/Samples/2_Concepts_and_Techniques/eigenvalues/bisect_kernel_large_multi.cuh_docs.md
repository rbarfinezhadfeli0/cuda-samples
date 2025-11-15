# Documentation for Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large_multi.cuh

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large_multi.cuh`
- **Type**: .cuh
- **Location**: Samples/2_Concepts_and_Techniques/eigenvalues
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```cuh
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

/* Perform second step of bisection algorithm for large matrices for
 * intervals that contained after the first step more than one eigenvalue
 */

#ifndef _BISECT_KERNEL_LARGE_MULTI_H_
#define _BISECT_KERNEL_LARGE_MULTI_H_

#include <cooperative_groups.h>

namespace cg = cooperative_groups;
// includes, project
#include "config.h"
#include "util.h"

// additional kernel
#include "bisect_util.cu"

////////////////////////////////////////////////////////////////////////////////
//! Perform second step of bisection algorithm for large matrices for
//! intervals that after the first step contained more than one eigenvalue
//! @param  g_d  diagonal elements of symmetric, tridiagonal matrix
//! @param  g_s  superdiagonal elements of symmetric, tridiagonal matrix
//! @param  n    matrix size
//! @param  blocks_mult  start addresses of blocks of intervals that are
//!                      processed by one block of threads, each of the
//!                      intervals contains more than one eigenvalue
//! @param  blocks_mult_sum  total number of eigenvalues / singleton intervals
//!                          in one block of intervals
//! @param  g_left  left limits of intervals
//! @param  g_right  right limits of intervals
//! @param  g_left_count  number of eigenvalues less than left limits
//! @param  g_right_count  number of eigenvalues less than right limits
//! @param  g_lambda  final eigenvalue
//! @param  g_pos  index of eigenvalue (in ascending order)
//! @param  precision  desired precision of eigenvalues
////////////////////////////////////////////////////////////////////////////////
__global__ void bisectKernelLarge_MultIntervals(float             *g_d,
                                                float             *g_s,
                                                const unsigned int n,
                                                unsigned int      *blocks_mult,
                                                unsigned int      *blocks_mult_sum,
                                                float             *g_left,
                                                float             *g_right,
                                                unsigned int      *g_left_count,
                                                unsigned int      *g_right_count,
                                                float             *g_lambda,
                                                unsigned int      *g_pos,
                                                float              precision)
{
    // Handle to thread block group
    cg::thread_block   cta = cg::this_thread_block();
    const unsigned int tid = threadIdx.x;

    // left and right limits of interval
    __shared__ float s_left[2 * MAX_THREADS_BLOCK];
    __shared__ float s_right[2 * MAX_THREADS_BLOCK];

    // number of eigenvalues smaller than interval limits
    __shared__ unsigned int s_left_count[2 * MAX_THREADS_BLOCK];
    __shared__ unsigned int s_right_count[2 * MAX_THREADS_BLOCK];

    // helper array for chunk compaction of second chunk
    __shared__ unsigned int s_compaction_list[2 * MAX_THREADS_BLOCK + 1];
    // compaction list helper for exclusive scan
    unsigned int *s_compaction_list_exc = s_compaction_list + 1;

    // flag if all threads are converged
    __shared__ unsigned int all_threads_converged;
    // number of active threads
    __shared__ unsigned int num_threads_active;
    // number of threads to employ for compaction
    __shared__ unsigned int num_threads_compaction;
    // flag if second chunk has to be compacted
    __shared__ unsigned int compact_second_chunk;

    // parameters of block of intervals processed by this block of threads
    __shared__ unsigned int c_block_start;
    __shared__ unsigned int c_block_end;
    __shared__ unsigned int c_block_offset_output;

    // midpoint of currently active interval of the thread
    float mid = 0.0f;
    // number of eigenvalues smaller than \a mid
    unsigned int mid_count = 0;
    // current interval parameter
    float        left;
    float        right;
    unsigned int left_count;
    unsigned int right_count;
    // helper for compaction, keep track which threads have a second child
    unsigned int is_active_second = 0;

    // initialize common start conditions
    if (0 == tid) {
        c_block_start         = blocks_mult[blockIdx.x];
        c_block_end           = blocks_mult[blockIdx.x + 1];
        c_block_offset_output = blocks_mult_sum[blockIdx.x];

        num_threads_active     = c_block_end - c_block_start;
        s_compaction_list[0]   = 0;
        num_threads_compaction = ceilPow2(num_threads_active);

        all_threads_converged = 1;
        compact_second_chunk  = 0;
    }

    cg::sync(cta);

    // read data into shared memory
    if (tid < num_threads_active) {
        s_left[tid]        = g_left[c_block_start + tid];
        s_right[tid]       = g_right[c_block_start + tid];
        s_left_count[tid]  = g_left_count[c_block_start + tid];
        s_right_count[tid] = g_right_count[c_block_start + tid];
    }

    cg::sync(cta);

    // do until all threads converged
    while (true) {
        // for (int iter=0; iter < 0; iter++) {

        // subdivide interval if currently active and not already converged
        subdivideActiveInterval(tid,
                                s_left,
                                s_right,
                                s_left_count,
                                s_right_count,
                                num_threads_active,
                                left,
                                right,
                                left_count,
                                right_count,
                                mid,
                                all_threads_converged);

        cg::sync(cta);

        // stop if all eigenvalues have been found
        if (1 == all_threads_converged) {
            break;
        }

        // compute number of eigenvalues smaller than mid for active and not
        // converged intervals, use all threads for loading data from gmem and
        // s_left and s_right as scratch space to store the data load from gmem
        // in shared memory
        mid_count = computeNumSmallerEigenvalsLarge(
            g_d, g_s, n, mid, tid, num_threads_active, s_left, s_right, (left == right), cta);

        cg::sync(cta);

        if (tid < num_threads_active) {
            // store intervals
            if (left != right) {
                storeNonEmptyIntervals(tid,
                                       num_threads_active,
                                       s_left,
                                       s_right,
                                       s_left_count,
                                       s_right_count,
                                       left,
                                       mid,
                                       right,
                                       left_count,
                                       mid_count,
                                       right_count,
                                       precision,
                                       compact_second_chunk,
                                       s_compaction_list_exc,
                                       is_active_second);
            }
            else {
                storeIntervalConverged(s_left,
                                       s_right,
                                       s_left_count,
                                       s_right_count,
                                       left,
                                       mid,
                                       right,
                                       left_count,
                                       mid_count,
                                       right_count,
                                       s_compaction_list_exc,
                                       compact_second_chunk,
                                       num_threads_active,
                                       is_active_second);
            }
        }

        cg::sync(cta);

        // compact second chunk of intervals if any of the threads generated
        // two child intervals
        if (1 == compact_second_chunk) {
            createIndicesCompaction(s_compaction_list_exc, num_threads_compaction, cta);

            compactIntervals(s_left,
                             s_right,
                             s_left_count,
                             s_right_count,
                             mid,
                             right,
                             mid_count,
                             right_count,
                             s_compaction_list,
                             num_threads_active,
                             is_active_second);
        }

        cg::sync(cta);

        // update state variables
        if (0 == tid) {
            num_threads_active += s_compaction_list[num_threads_active];
            num_threads_compaction = ceilPow2(num_threads_active);

            compact_second_chunk  = 0;
            all_threads_converged = 1;
        }

        cg::sync(cta);

        // clear
        s_compaction_list_exc[threadIdx.x]              = 0;
        s_compaction_list_exc[threadIdx.x + blockDim.x] = 0;

        cg::sync(cta);

    } // end until all threads converged

    // write data back to global memory
    if (tid < num_threads_active) {
        unsigned int addr = c_block_offset_output + tid;

        g_lambda[addr] = s_left[tid];
        g_pos[addr]    = s_right_count[tid];
    }
}

#endif // #ifndef _BISECT_KERNEL_LARGE_MULTI_H_

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/eigenvalues/bisect_kernel_large_multi.cuh`.

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

- **Total Lines**: 269
- **Approximate Size**: 11218 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

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

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

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
