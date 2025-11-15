# Documentation for Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/jacobiCudaGraphs
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
#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <vector>

#include "jacobi.h"

namespace cg = cooperative_groups;

// 8 Rows of square-matrix A processed by each CTA.
// This can be max 32 and only power of 2 (i.e., 2/4/8/16/32).
#define ROWS_PER_CTA 8

#if !defined(__CUDA_ARCH__) || __CUDA_ARCH__ >= 600
#else
__device__ double atomicAdd(double *address, double val)
{
    unsigned long long int *address_as_ull = (unsigned long long int *)address;
    unsigned long long int  old            = *address_as_ull, assumed;

    do {
        assumed = old;
        old     = atomicCAS(address_as_ull, assumed, __double_as_longlong(val + __longlong_as_double(assumed)));

        // Note: uses integer comparison to avoid hang in case of NaN (since NaN !=
        // NaN)
    } while (assumed != old);

    return __longlong_as_double(old);
}
#endif

static __global__ void
JacobiMethod(const float *A, const double *b, const float conv_threshold, double *x, double *x_new, double *sum)
{
    // Handle to thread block group
    cg::thread_block  cta = cg::this_thread_block();
    __shared__ double x_shared[N_ROWS]; // N_ROWS == n
    __shared__ double b_shared[ROWS_PER_CTA + 1];

    for (int i = threadIdx.x; i < N_ROWS; i += blockDim.x) {
        x_shared[i] = x[i];
    }

    if (threadIdx.x < ROWS_PER_CTA) {
        int k = threadIdx.x;
        for (int i = k + (blockIdx.x * ROWS_PER_CTA); (k < ROWS_PER_CTA) && (i < N_ROWS);
             k += ROWS_PER_CTA, i += ROWS_PER_CTA) {
            b_shared[i % (ROWS_PER_CTA + 1)] = b[i];
        }
    }

    cg::sync(cta);

    cg::thread_block_tile<32> tile32 = cg::tiled_partition<32>(cta);

    for (int k = 0, i = blockIdx.x * ROWS_PER_CTA; (k < ROWS_PER_CTA) && (i < N_ROWS); k++, i++) {
        double rowThreadSum = 0.0;
        for (int j = threadIdx.x; j < N_ROWS; j += blockDim.x) {
            rowThreadSum += (A[i * N_ROWS + j] * x_shared[j]);
        }

        for (int offset = tile32.size() / 2; offset > 0; offset /= 2) {
            rowThreadSum += tile32.shfl_down(rowThreadSum, offset);
        }

        if (tile32.thread_rank() == 0) {
            atomicAdd(&b_shared[i % (ROWS_PER_CTA + 1)], -rowThreadSum);
        }
    }

    cg::sync(cta);

    if (threadIdx.x < ROWS_PER_CTA) {
        cg::thread_block_tile<ROWS_PER_CTA> tile8    = cg::tiled_partition<ROWS_PER_CTA>(cta);
        double                              temp_sum = 0.0;

        int k = threadIdx.x;

        for (int i = k + (blockIdx.x * ROWS_PER_CTA); (k < ROWS_PER_CTA) && (i < N_ROWS);
             k += ROWS_PER_CTA, i += ROWS_PER_CTA) {
            double dx = b_shared[i % (ROWS_PER_CTA + 1)];
            dx /= A[i * N_ROWS + i];

            x_new[i] = (x_shared[i] + dx);
            temp_sum += fabs(dx);
        }

        for (int offset = tile8.size() / 2; offset > 0; offset /= 2) {
            temp_sum += tile8.shfl_down(temp_sum, offset);
        }

        if (tile8.thread_rank() == 0) {
            atomicAdd(sum, temp_sum);
        }
    }
}

// Thread block size for finalError kernel should be multiple of 32
static __global__ void finalError(double *x, double *g_sum)
{
    // Handle to thread block group
    cg::thread_block         cta = cg::this_thread_block();
    extern __shared__ double warpSum[];
    double                   sum = 0.0;

    int globalThreadId = blockIdx.x * blockDim.x + threadIdx.x;

    for (int i = globalThreadId; i < N_ROWS; i += blockDim.x * gridDim.x) {
        double d = x[i] - 1.0;
        sum += fabs(d);
    }

    cg::thread_block_tile<32> tile32 = cg::tiled_partition<32>(cta);

    for (int offset = tile32.size() / 2; offset > 0; offset /= 2) {
        sum += tile32.shfl_down(sum, offset);
    }

    if (tile32.thread_rank() == 0) {
        warpSum[threadIdx.x / warpSize] = sum;
    }

    cg::sync(cta);

    double blockSum = 0.0;
    if (threadIdx.x < (blockDim.x / warpSize)) {
        blockSum = warpSum[threadIdx.x];
    }

    if (threadIdx.x < 32) {
        for (int offset = tile32.size() / 2; offset > 0; offset /= 2) {
            blockSum += tile32.shfl_down(blockSum, offset);
        }
        if (tile32.thread_rank() == 0) {
            atomicAdd(g_sum, blockSum);
        }
    }
}

double JacobiMethodGpuCudaGraphExecKernelSetParams(const float  *A,
                                                   const double *b,
                                                   const float   conv_threshold,
                                                   const int     max_iter,
                                                   double       *x,
                                                   double       *x_new,
                                                   cudaStream_t  stream)
{
    // CTA size
    dim3 nthreads(256, 1, 1);
    // grid size
    dim3            nblocks((N_ROWS / ROWS_PER_CTA) + 2, 1, 1);
    cudaGraph_t     graph;
    cudaGraphExec_t graphExec = NULL;

    double  sum   = 0.0;
    double *d_sum = NULL;
    checkCudaErrors(cudaMalloc(&d_sum, sizeof(double)));

    std::vector<cudaGraphNode_t> nodeDependencies;
    cudaGraphNode_t              memcpyNode, jacobiKernelNode, memsetNode;
    cudaMemcpy3DParms            memcpyParams = {0};
    cudaMemsetParams             memsetParams = {0};

    memsetParams.dst   = (void *)d_sum;
    memsetParams.value = 0;
    memsetParams.pitch = 0;
    // elementSize can be max 4 bytes, so we take sizeof(float) and width=2
    memsetParams.elementSize = sizeof(float);
    memsetParams.width       = 2;
    memsetParams.height      = 1;

    checkCudaErrors(cudaGraphCreate(&graph, 0));
    checkCudaErrors(cudaGraphAddMemsetNode(&memsetNode, graph, NULL, 0, &memsetParams));
    nodeDependencies.push_back(memsetNode);

    cudaKernelNodeParams NodeParams0, NodeParams1;
    NodeParams0.func           = (void *)JacobiMethod;
    NodeParams0.gridDim        = nblocks;
    NodeParams0.blockDim       = nthreads;
    NodeParams0.sharedMemBytes = 0;
    void *kernelArgs0[6]       = {
        (void *)&A, (void *)&b, (void *)&conv_threshold, (void *)&x, (void *)&x_new, (void *)&d_sum};
    NodeParams0.kernelParams = kernelArgs0;
    NodeParams0.extra        = NULL;

    checkCudaErrors(cudaGraphAddKernelNode(
        &jacobiKernelNode, graph, nodeDependencies.data(), nodeDependencies.size(), &NodeParams0));

    nodeDependencies.clear();
    nodeDependencies.push_back(jacobiKernelNode);

    memcpyParams.srcArray = NULL;
    memcpyParams.srcPos   = make_cudaPos(0, 0, 0);
    memcpyParams.srcPtr   = make_cudaPitchedPtr(d_sum, sizeof(double), 1, 1);
    memcpyParams.dstArray = NULL;
    memcpyParams.dstPos   = make_cudaPos(0, 0, 0);
    memcpyParams.dstPtr   = make_cudaPitchedPtr(&sum, sizeof(double), 1, 1);
    memcpyParams.extent   = make_cudaExtent(sizeof(double), 1, 1);
    memcpyParams.kind     = cudaMemcpyDeviceToHost;

    checkCudaErrors(
        cudaGraphAddMemcpyNode(&memcpyNode, graph, nodeDependencies.data(), nodeDependencies.size(), &memcpyParams));

    checkCudaErrors(cudaGraphInstantiate(&graphExec, graph, NULL, NULL, 0));

    NodeParams1.func           = (void *)JacobiMethod;
    NodeParams1.gridDim        = nblocks;
    NodeParams1.blockDim       = nthreads;
    NodeParams1.sharedMemBytes = 0;
    void *kernelArgs1[6]       = {
        (void *)&A, (void *)&b, (void *)&conv_threshold, (void *)&x_new, (void *)&x, (void *)&d_sum};
    NodeParams1.kernelParams = kernelArgs1;
    NodeParams1.extra        = NULL;

    int k = 0;
    for (k = 0; k < max_iter; k++) {
        checkCudaErrors(cudaGraphExecKernelNodeSetParams(
            graphExec, jacobiKernelNode, ((k & 1) == 0) ? &NodeParams0 : &NodeParams1));
        checkCudaErrors(cudaGraphLaunch(graphExec, stream));
        checkCudaErrors(cudaStreamSynchronize(stream));

        if (sum <= conv_threshold) {
            checkCudaErrors(cudaMemsetAsync(d_sum, 0, sizeof(double), stream));
            nblocks.x            = (N_ROWS / nthreads.x) + 1;
            size_t sharedMemSize = ((nthreads.x / 32) + 1) * sizeof(double);
            if ((k & 1) == 0) {
                finalError<<<nblocks, nthreads, sharedMemSize, stream>>>(x_new, d_sum);
            }
            else {
                finalError<<<nblocks, nthreads, sharedMemSize, stream>>>(x, d_sum);
            }

            checkCudaErrors(cudaMemcpyAsync(&sum, d_sum, sizeof(double), cudaMemcpyDeviceToHost, stream));
            checkCudaErrors(cudaStreamSynchronize(stream));
            printf("GPU iterations : %d\n", k + 1);
            printf("GPU error : %.3e\n", sum);
            break;
        }
    }

    checkCudaErrors(cudaFree(d_sum));
    return sum;
}

double JacobiMethodGpuCudaGraphExecUpdate(const float  *A,
                                          const double *b,
                                          const float   conv_threshold,
                                          const int     max_iter,
                                          double       *x,
                                          double       *x_new,
                                          cudaStream_t  stream)
{
    // CTA size
    dim3 nthreads(256, 1, 1);
    // grid size
    dim3            nblocks((N_ROWS / ROWS_PER_CTA) + 2, 1, 1);
    cudaGraph_t     graph;
    cudaGraphExec_t graphExec = NULL;

    double  sum = 0.0;
    double *d_sum;
    checkCudaErrors(cudaMalloc(&d_sum, sizeof(double)));

    int k = 0;
    for (k = 0; k < max_iter; k++) {
        checkCudaErrors(cudaStreamBeginCapture(stream, cudaStreamCaptureModeGlobal));
        checkCudaErrors(cudaMemsetAsync(d_sum, 0, sizeof(double), stream));
        if ((k & 1) == 0) {
            JacobiMethod<<<nblocks, nthreads, 0, stream>>>(A, b, conv_threshold, x, x_new, d_sum);
        }
        else {
            JacobiMethod<<<nblocks, nthreads, 0, stream>>>(A, b, conv_threshold, x_new, x, d_sum);
        }
        checkCudaErrors(cudaMemcpyAsync(&sum, d_sum, sizeof(double), cudaMemcpyDeviceToHost, stream));
        checkCudaErrors(cudaStreamEndCapture(stream, &graph));

        if (graphExec == NULL) {
            checkCudaErrors(cudaGraphInstantiate(&graphExec, graph, NULL, NULL, 0));
        }
        else {
            cudaGraphExecUpdateResult updateResult_out;
            checkCudaErrors(cudaGraphExecUpdate(graphExec, graph, NULL, &updateResult_out));
            if (updateResult_out != cudaGraphExecUpdateSuccess) {
                if (graphExec != NULL) {
                    checkCudaErrors(cudaGraphExecDestroy(graphExec));
                }
                printf("k = %d graph update failed with error - %d\n", k, updateResult_out);
                checkCudaErrors(cudaGraphInstantiate(&graphExec, graph, NULL, NULL, 0));
            }
        }
        checkCudaErrors(cudaGraphLaunch(graphExec, stream));
        checkCudaErrors(cudaStreamSynchronize(stream));

        if (sum <= conv_threshold) {
            checkCudaErrors(cudaMemsetAsync(d_sum, 0, sizeof(double), stream));
            nblocks.x            = (N_ROWS / nthreads.x) + 1;
            size_t sharedMemSize = ((nthreads.x / 32) + 1) * sizeof(double);
            if ((k & 1) == 0) {
                finalError<<<nblocks, nthreads, sharedMemSize, stream>>>(x_new, d_sum);
            }
            else {
                finalError<<<nblocks, nthreads, sharedMemSize, stream>>>(x, d_sum);
            }

            checkCudaErrors(cudaMemcpyAsync(&sum, d_sum, sizeof(double), cudaMemcpyDeviceToHost, stream));
            checkCudaErrors(cudaStreamSynchronize(stream));
            printf("GPU iterations : %d\n", k + 1);
            printf("GPU error : %.3e\n", sum);
            break;
        }
    }

    checkCudaErrors(cudaFree(d_sum));
    return sum;
}

double JacobiMethodGpu(const float  *A,
                       const double *b,
                       const float   conv_threshold,
                       const int     max_iter,
                       double       *x,
                       double       *x_new,
                       cudaStream_t  stream)
{
    // CTA size
    dim3 nthreads(256, 1, 1);
    // grid size
    dim3 nblocks((N_ROWS / ROWS_PER_CTA) + 2, 1, 1);

    double  sum = 0.0;
    double *d_sum;
    checkCudaErrors(cudaMalloc(&d_sum, sizeof(double)));
    int k = 0;

    for (k = 0; k < max_iter; k++) {
        checkCudaErrors(cudaMemsetAsync(d_sum, 0, sizeof(double), stream));
        if ((k & 1) == 0) {
            JacobiMethod<<<nblocks, nthreads, 0, stream>>>(A, b, conv_threshold, x, x_new, d_sum);
        }
        else {
            JacobiMethod<<<nblocks, nthreads, 0, stream>>>(A, b, conv_threshold, x_new, x, d_sum);
        }
        checkCudaErrors(cudaMemcpyAsync(&sum, d_sum, sizeof(double), cudaMemcpyDeviceToHost, stream));
        checkCudaErrors(cudaStreamSynchronize(stream));

        if (sum <= conv_threshold) {
            checkCudaErrors(cudaMemsetAsync(d_sum, 0, sizeof(double), stream));
            nblocks.x            = (N_ROWS / nthreads.x) + 1;
            size_t sharedMemSize = ((nthreads.x / 32) + 1) * sizeof(double);
            if ((k & 1) == 0) {
                finalError<<<nblocks, nthreads, sharedMemSize, stream>>>(x_new, d_sum);
            }
            else {
                finalError<<<nblocks, nthreads, sharedMemSize, stream>>>(x, d_sum);
            }

            checkCudaErrors(cudaMemcpyAsync(&sum, d_sum, sizeof(double), cudaMemcpyDeviceToHost, stream));
            checkCudaErrors(cudaStreamSynchronize(stream));
            printf("GPU iterations : %d\n", k + 1);
            printf("GPU error : %.3e\n", sum);
            break;
        }
    }

    checkCudaErrors(cudaFree(d_sum));
    return sum;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/jacobiCudaGraphs/jacobi.cu`.

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

- **Total Lines**: 396
- **Approximate Size**: 15250 bytes

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
