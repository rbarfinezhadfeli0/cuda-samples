# Documentation for Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu

## File Metadata

- **Path**: `Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu`
- **Type**: .cu
- **Location**: Samples/3_CUDA_Features/graphMemoryNodes
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

// System includes
#include <assert.h>
#include <climits>
#include <stdio.h>
#include <vector>

// CUDA runtime
#include <cuda_runtime.h>

// helper functions and utilities to work with CUDA
#include <helper_cuda.h>
#include <helper_functions.h>

#define THREADS_PER_BLOCK  512
#define ALLOWABLE_VARIANCE 1.e-6f
#define NUM_ELEMENTS       8000000

// Stores the square of each input element in output array
__global__ void squareArray(const float *input, float *output, int numElements)
{
    int idx = blockIdx.x * blockDim.x + threadIdx.x;

    if (idx < numElements) {
        output[idx] = input[idx] * input[idx];
    }
}

// Stores the negative of each input element in output array
__global__ void negateArray(const float *input, float *output, int numElements)
{
    int idx = blockIdx.x * blockDim.x + threadIdx.x;

    if (idx < numElements) {
        output[idx] = input[idx] * -1;
    }
}

struct negSquareArrays
{
    float *input;
    float *square;
    float *negSquare;
    int    numElements;
    size_t bytes;
    size_t numBlocks;
};

void fillRandomly(float *array, int numElements)
{
    for (int n = 0; n < numElements; n++) {
        array[n] = rand() / (float)RAND_MAX;
    }
}

void resetOutputArrays(negSquareArrays *hostArrays)
{
    fillRandomly(hostArrays->square, hostArrays->numElements);
    fillRandomly(hostArrays->negSquare, hostArrays->numElements);
}

void prepareHostArrays(negSquareArrays *hostArrays)
{
    hostArrays->numElements = NUM_ELEMENTS;
    size_t bytes            = hostArrays->numElements * sizeof(float);

    size_t numBlocks = hostArrays->numElements / (size_t)THREADS_PER_BLOCK;
    if ((numBlocks % (size_t)THREADS_PER_BLOCK) != 0) {
        numBlocks++;
    }

    hostArrays->input     = (float *)malloc(bytes);
    hostArrays->square    = (float *)malloc(bytes);
    hostArrays->negSquare = (float *)malloc(bytes);
    hostArrays->bytes     = bytes;
    hostArrays->numBlocks = numBlocks;

    fillRandomly(hostArrays->input, hostArrays->numElements);
    fillRandomly(hostArrays->square, hostArrays->numElements);
    fillRandomly(hostArrays->negSquare, hostArrays->numElements);
}

void createFreeGraph(cudaGraphExec_t *graphExec, float *dPtr)
{
    cudaGraph_t     graph;
    cudaGraphNode_t freeNode;

    checkCudaErrors(cudaGraphCreate(&graph, 0));

    checkCudaErrors(cudaGraphAddMemFreeNode(&freeNode, graph, NULL, 0, (void *)dPtr));

    checkCudaErrors(cudaGraphInstantiate(graphExec, graph, NULL, NULL, 0));
    checkCudaErrors(cudaGraphDestroy(graph));
}

/**
 * Demonstrates explicitly creating a CUDA graph including memory nodes.
 * createNegateSquaresGraphWithStreamCapture constructs an equivalent graph
 * using stream capture.
 *
 * If d_negSquare_out is non null, then:
 * 1) d_negSquare will not be freed;
 * 2) the value of d_negSquare_out will be set to d_negSquare.
 *
 * Diagram of the graph constructed by createNegateSquaresGraphExplicitly:
 *
 * alloc d_input
 *       |
 * alloc d_square
 *       |
 * Memcpy a to device
 *       |
 * launch kernel squareArray ------->---- Memcpy d_square to host
 *       |                                      |
 * free d_input                                 |
 *       |                                      |
 * allocate d_negSquare                         |
 *       |                                      |
 * launch kernel negateArray -------->--- free d_square
 *       |
 * Memcpy d_negSquare to host
 *       |
 * free d_negSquare
 */
void createNegateSquaresGraphExplicitly(cudaGraphExec_t *graphExec,
                                        int              device,
                                        negSquareArrays *hostArrays,
                                        float          **d_negSquare_out = NULL)
{
    // Array buffers on device
    float *d_input, *d_square, *d_negSquare;

    // Memory allocation parameters
    cudaMemAllocNodeParams allocParams;
    memset(&allocParams, 0, sizeof(allocParams));
    allocParams.bytesize                = hostArrays->bytes;
    allocParams.poolProps.allocType     = cudaMemAllocationTypePinned;
    allocParams.poolProps.location.id   = device;
    allocParams.poolProps.location.type = cudaMemLocationTypeDevice;

    // Kernel launch parameters
    cudaKernelNodeParams kernelNodeParams = {0};
    kernelNodeParams.gridDim              = dim3(hostArrays->numBlocks, 1, 1);
    kernelNodeParams.blockDim             = dim3(THREADS_PER_BLOCK, 1, 1);
    kernelNodeParams.sharedMemBytes       = 0;
    kernelNodeParams.extra                = NULL;

    cudaGraph_t     graph;
    cudaGraphNode_t allocNodeInput, allocNodeSquare, allocNodeNegSquare;
    cudaGraphNode_t copyNodeInput, copyNodeSquare, copyNodeNegSquare;
    cudaGraphNode_t squareKernelNode, negateKernelNode;
    cudaGraphNode_t freeNodeInput, freeNodeSquare;

    // Buffer for storing graph node dependencies
    std::vector<cudaGraphNode_t> nodeDependencies;

    checkCudaErrors(cudaGraphCreate(&graph, 0));

    checkCudaErrors(cudaGraphAddMemAllocNode(&allocNodeInput, graph, NULL, 0, &allocParams));
    d_input = (float *)allocParams.dptr;

    // To keep the graph structure simple (fewer branching dependencies),
    // allocNodeSquare should depend on allocNodeInput
    checkCudaErrors(cudaGraphAddMemAllocNode(&allocNodeSquare, graph, &allocNodeInput, 1, &allocParams));
    d_square = (float *)allocParams.dptr;

    // copyNodeInput needs to depend on allocNodeInput because copyNodeInput
    // writes to d_input. It does so here indirectly through allocNodeSquare.
    checkCudaErrors(cudaGraphAddMemcpyNode1D(&copyNodeInput,
                                             graph,
                                             &allocNodeSquare,
                                             1,
                                             d_input,
                                             hostArrays->input,
                                             hostArrays->bytes,
                                             cudaMemcpyHostToDevice));

    void *squareKernelArgs[3]     = {(void *)&d_input, (void *)&d_square, (void *)&(hostArrays->numElements)};
    kernelNodeParams.func         = (void *)squareArray;
    kernelNodeParams.kernelParams = (void **)squareKernelArgs;

    // Square kernel depends on copyNodeInput to ensure all data is on the device
    // before kernel launch.
    checkCudaErrors(cudaGraphAddKernelNode(&squareKernelNode, graph, &copyNodeInput, 1, &kernelNodeParams));

    checkCudaErrors(cudaGraphAddMemcpyNode1D(&copyNodeSquare,
                                             graph,
                                             &squareKernelNode,
                                             1,
                                             hostArrays->square,
                                             d_square,
                                             hostArrays->bytes,
                                             cudaMemcpyDeviceToHost));

    // Free of d_input depends on the square kernel to ensure that d_input is not
    // freed while being read by the kernel. It also depends on the alloc of
    // d_input via squareKernelNode > copyNodeInput > allocNodeSquare >
    // allocNodeInput.
    checkCudaErrors(cudaGraphAddMemFreeNode(&freeNodeInput, graph, &squareKernelNode, 1, d_input));

    // Allocation of C depends on free of A so CUDA can reuse the virtual address.
    checkCudaErrors(cudaGraphAddMemAllocNode(&allocNodeNegSquare, graph, &freeNodeInput, 1, &allocParams));
    d_negSquare = (float *)allocParams.dptr;

    if (d_negSquare == d_input) {
        printf("Check verified that d_negSquare and d_input share a virtual "
               "address.\n");
    }

    void *negateKernelArgs[3]     = {(void *)&d_square, (void *)&d_negSquare, (void *)&(hostArrays->numElements)};
    kernelNodeParams.func         = (void *)negateArray;
    kernelNodeParams.kernelParams = (void **)negateKernelArgs;

    checkCudaErrors(cudaGraphAddKernelNode(&negateKernelNode, graph, &allocNodeNegSquare, 1, &kernelNodeParams));

    nodeDependencies.push_back(copyNodeSquare);
    nodeDependencies.push_back(negateKernelNode);
    checkCudaErrors(
        cudaGraphAddMemFreeNode(&freeNodeSquare, graph, nodeDependencies.data(), nodeDependencies.size(), d_square));
    nodeDependencies.clear();

    checkCudaErrors(cudaGraphAddMemcpyNode1D(&copyNodeNegSquare,
                                             graph,
                                             &negateKernelNode,
                                             1,
                                             hostArrays->negSquare,
                                             d_negSquare,
                                             hostArrays->bytes,
                                             cudaMemcpyDeviceToHost));

    if (d_negSquare_out == NULL) {
        cudaGraphNode_t freeNodeNegSquare;
        checkCudaErrors(cudaGraphAddMemFreeNode(&freeNodeNegSquare, graph, &copyNodeNegSquare, 1, d_negSquare));
    }
    else {
        *d_negSquare_out = d_negSquare;
    }

    checkCudaErrors(cudaGraphInstantiate(graphExec, graph, NULL, NULL, 0));
    checkCudaErrors(cudaGraphDestroy(graph));
}

/**
 * Adds work to a CUDA stream which negates the square of values in the input
 * array.
 *
 * If d_negSquare_out is non null, then:
 * 1) d_negSquare will not be freed;
 * 2) the value of d_negSquare_out will be set to d_negSquare.
 *
 * Diagram of the stream operations in doNegateSquaresInStream
 * ---------------------------------------------------------------------
 * | STREAM                             | STREAM2                      |
 * ---------------------------------------------------------------------
 *
 * alloc d_input
 *       |
 * alloc d_square
 *       |
 * Memcpy a to device
 *       |
 * launch kernel squareArray
 *       |
 * record squareKernelCompleteEvent -->-- wait squareKernelCompleteEvent
 *       |                                      |
 * free d_input                                 |
 *       |                                      |
 * allocate d_negSquare                   Memcpy d_square to host
 *       |                                      |
 * launch kernel negateArray                    |
 *       |                                      |
 * record negateKernelCompleteEvent -->-- wait negateKernelCompleteEvent
 *       |                                      |
 * Memcpy d_negSquare to host                   |
 *       |                                free d_square
 * free d_negSquare                             |
 *       |                                      |
 * wait squareFreeEvent --------------<---- record squareFreeEvent
 */
void doNegateSquaresInStream(cudaStream_t stream1, negSquareArrays *hostArrays, float **d_negSquare_out = NULL)
{
    float       *d_input, *d_square, *d_negSquare;
    cudaStream_t stream2;
    cudaEvent_t  squareKernelCompleteEvent, negateKernelCompleteEvent, squareFreeEvent;

    checkCudaErrors(cudaStreamCreateWithFlags(&stream2, cudaStreamNonBlocking));

    checkCudaErrors(cudaEventCreate(&squareKernelCompleteEvent));
    checkCudaErrors(cudaEventCreate(&negateKernelCompleteEvent));
    checkCudaErrors(cudaEventCreate(&squareFreeEvent));

    // Virtual addresses are assigned synchronously when cudaMallocAsync is
    // called, thus there is no performace benefit gained by separating the
    // allocations into two streams.
    checkCudaErrors(cudaMallocAsync(&d_input, hostArrays->bytes, stream1));
    checkCudaErrors(cudaMallocAsync(&d_square, hostArrays->bytes, stream1));

    checkCudaErrors(cudaMemcpyAsync(d_input, hostArrays->input, hostArrays->bytes, cudaMemcpyHostToDevice, stream1));
    squareArray<<<hostArrays->numBlocks, THREADS_PER_BLOCK, 0, stream1>>>(d_input, d_square, hostArrays->numElements);
    checkCudaErrors(cudaEventRecord(squareKernelCompleteEvent, stream1));

    checkCudaErrors(cudaStreamWaitEvent(stream2, squareKernelCompleteEvent, 0));
    checkCudaErrors(cudaMemcpyAsync(hostArrays->square, d_square, hostArrays->bytes, cudaMemcpyDeviceToHost, stream2));

    checkCudaErrors(cudaFreeAsync(d_input, stream1));
    checkCudaErrors(cudaMallocAsync(&d_negSquare, hostArrays->bytes, stream1));
    negateArray<<<hostArrays->numBlocks, THREADS_PER_BLOCK, 0, stream1>>>(
        d_square, d_negSquare, hostArrays->numElements);
    checkCudaErrors(cudaEventRecord(negateKernelCompleteEvent, stream1));
    checkCudaErrors(
        cudaMemcpyAsync(hostArrays->negSquare, d_negSquare, hostArrays->bytes, cudaMemcpyDeviceToHost, stream1));
    if (d_negSquare_out == NULL) {
        checkCudaErrors(cudaFreeAsync(d_negSquare, stream1));
    }
    else {
        *d_negSquare_out = d_negSquare;
    }

    checkCudaErrors(cudaStreamWaitEvent(stream2, negateKernelCompleteEvent, 0));
    checkCudaErrors(cudaFreeAsync(d_square, stream2));
    checkCudaErrors(cudaEventRecord(squareFreeEvent, stream2));

    checkCudaErrors(cudaStreamWaitEvent(stream1, squareFreeEvent, 0));

    checkCudaErrors(cudaStreamDestroy(stream2));
    checkCudaErrors(cudaEventDestroy(squareKernelCompleteEvent));
    checkCudaErrors(cudaEventDestroy(negateKernelCompleteEvent));
    checkCudaErrors(cudaEventDestroy(squareFreeEvent));
}

/**
 * Demonstrates creating a CUDA graph including memory nodes using stream
 * capture. createNegateSquaresGraphExplicitly constructs an equivalent graph
 * without stream capture.
 */
void createNegateSquaresGraphWithStreamCapture(cudaGraphExec_t *graphExec,
                                               negSquareArrays *hostArrays,
                                               float          **d_negSquare_out = NULL)
{
    cudaGraph_t  graph;
    cudaStream_t stream;

    checkCudaErrors(cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking));

    checkCudaErrors(cudaStreamBeginCapture(stream, cudaStreamCaptureModeGlobal));
    doNegateSquaresInStream(stream, hostArrays, d_negSquare_out);
    checkCudaErrors(cudaStreamEndCapture(stream, &graph));

    checkCudaErrors(cudaGraphInstantiate(graphExec, graph, NULL, NULL, 0));
    checkCudaErrors(cudaStreamDestroy(stream));
    checkCudaErrors(cudaGraphDestroy(graph));
}

void prepareRefArrays(negSquareArrays *hostArrays, negSquareArrays *deviceRefArrays, bool **foundValidationFailure)
{
    deviceRefArrays->bytes       = hostArrays->bytes;
    deviceRefArrays->numElements = hostArrays->numElements;

    for (int i = 0; i < hostArrays->numElements; i++) {
        hostArrays->square[i]    = hostArrays->input[i] * hostArrays->input[i];
        hostArrays->negSquare[i] = hostArrays->square[i] * -1;
    }

    checkCudaErrors(cudaMalloc((void **)&deviceRefArrays->negSquare, deviceRefArrays->bytes));
    checkCudaErrors(
        cudaMemcpy(deviceRefArrays->negSquare, hostArrays->negSquare, hostArrays->bytes, cudaMemcpyHostToDevice));

    checkCudaErrors(cudaMallocManaged((void **)foundValidationFailure, sizeof(bool)));
}

int checkValidationFailure(bool *foundValidationFailure)
{
    if (*foundValidationFailure) {
        printf("Validation FAILURE!\n\n");
        *foundValidationFailure = false;
        return EXIT_FAILURE;
    }
    else {
        printf("Validation PASSED!\n\n");
        return EXIT_SUCCESS;
    }
}

__global__ void validateGPU(float *d_negSquare, negSquareArrays devRefArrays, bool *foundValidationFailure)
{
    int   idx = blockIdx.x * blockDim.x + threadIdx.x;
    float ref, diff;

    if (idx < devRefArrays.numElements) {
        ref  = devRefArrays.negSquare[idx];
        diff = d_negSquare[idx] - ref;
        diff *= diff;
        ref *= ref;
        if (diff / ref > ALLOWABLE_VARIANCE) {
            *foundValidationFailure = true;
        }
    }
}

void validateHost(negSquareArrays *hostArrays, bool *foundValidationFailure)
{
    float ref, diff;

    for (int i = 0; i < hostArrays->numElements; i++) {
        ref  = hostArrays->input[i] * hostArrays->input[i] * -1;
        diff = hostArrays->negSquare[i] - ref;
        diff *= diff;
        ref *= ref;
        if (diff / ref > ALLOWABLE_VARIANCE) {
            *foundValidationFailure = true;
        }
    }
}

int main(int argc, char **argv)
{
    negSquareArrays hostArrays, deviceRefArrays;
    cudaStream_t    stream;
    cudaGraphExec_t graphExec, graphExecFreeC;

    // Declare pointers for GPU buffers
    float *d_negSquare            = NULL;
    bool  *foundValidationFailure = NULL;

    srand(time(0));
    int device = findCudaDevice(argc, (const char **)argv);

    int driverVersion             = 0;
    int deviceSupportsMemoryPools = 0;

    cudaDriverGetVersion(&driverVersion);
    printf("Driver version is: %d.%d\n", driverVersion / 1000, (driverVersion % 100) / 10);

    if (driverVersion < 11040) {
        printf("Waiving execution as driver does not support Graph Memory Nodes\n");
        exit(EXIT_WAIVED);
    }

    cudaDeviceGetAttribute(&deviceSupportsMemoryPools, cudaDevAttrMemoryPoolsSupported, device);
    if (!deviceSupportsMemoryPools) {
        printf("Waiving execution as device does not support Memory Pools\n");
        exit(EXIT_WAIVED);
    }
    else {
        printf("Setting up sample.\n");
    }

    prepareHostArrays(&hostArrays);
    prepareRefArrays(&hostArrays, &deviceRefArrays, &foundValidationFailure);
    checkCudaErrors(cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking));
    printf("Setup complete.\n\n");

    printf("Running negateSquares in a stream.\n");
    doNegateSquaresInStream(stream, &hostArrays);
    checkCudaErrors(cudaStreamSynchronize(stream));
    printf("Validating negateSquares in a stream...\n");
    validateHost(&hostArrays, foundValidationFailure);
    checkValidationFailure(foundValidationFailure);
    resetOutputArrays(&hostArrays);

    printf("Running negateSquares in a stream-captured graph.\n");
    createNegateSquaresGraphWithStreamCapture(&graphExec, &hostArrays);
    checkCudaErrors(cudaGraphLaunch(graphExec, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));
    printf("Validating negateSquares in a stream-captured graph...\n");
    validateHost(&hostArrays, foundValidationFailure);
    checkValidationFailure(foundValidationFailure);
    resetOutputArrays(&hostArrays);

    printf("Running negateSquares in an explicitly constructed graph.\n");
    createNegateSquaresGraphExplicitly(&graphExec, device, &hostArrays);
    checkCudaErrors(cudaGraphLaunch(graphExec, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));
    printf("Validating negateSquares in an explicitly constructed graph...\n");
    validateHost(&hostArrays, foundValidationFailure);
    checkValidationFailure(foundValidationFailure);
    resetOutputArrays(&hostArrays);

    // Each of the three examples below free d_negSquare outside the graph. As
    // demonstrated by validateGPU, d_negSquare can be accessed by outside the
    // graph before d_negSquare is freed.

    printf("Running negateSquares with d_negSquare freed outside the stream.\n");
    createNegateSquaresGraphExplicitly(&graphExec, device, &hostArrays, &d_negSquare);
    checkCudaErrors(cudaGraphLaunch(graphExec, stream));
    validateGPU<<<hostArrays.numBlocks, THREADS_PER_BLOCK, 0, stream>>>(
        d_negSquare, deviceRefArrays, foundValidationFailure);
    // Since cudaFree is synchronous, the stream must synchronize before freeing
    // d_negSquare to ensure d_negSquare no longer being accessed.
    checkCudaErrors(cudaStreamSynchronize(stream));
    checkCudaErrors(cudaFree(d_negSquare));
    printf("Validating negateSquares with d_negSquare freed outside the "
           "stream...\n");
    validateHost(&hostArrays, foundValidationFailure);
    checkValidationFailure(foundValidationFailure);
    resetOutputArrays(&hostArrays);

    printf("Running negateSquares with d_negSquare freed outside the graph.\n");
    checkCudaErrors(cudaGraphLaunch(graphExec, stream));
    validateGPU<<<hostArrays.numBlocks, THREADS_PER_BLOCK, 0, stream>>>(
        d_negSquare, deviceRefArrays, foundValidationFailure);
    checkCudaErrors(cudaFreeAsync(d_negSquare, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));
    printf("Validating negateSquares with d_negSquare freed outside the graph...\n");
    checkValidationFailure(foundValidationFailure);
    resetOutputArrays(&hostArrays);

    printf("Running negateSquares with d_negSquare freed in a different graph.\n");
    createFreeGraph(&graphExecFreeC, d_negSquare);
    checkCudaErrors(cudaGraphLaunch(graphExec, stream));
    validateGPU<<<hostArrays.numBlocks, THREADS_PER_BLOCK, 0, stream>>>(
        d_negSquare, deviceRefArrays, foundValidationFailure);
    checkCudaErrors(cudaGraphLaunch(graphExecFreeC, stream));
    checkCudaErrors(cudaStreamSynchronize(stream));
    printf("Validating negateSquares with d_negSquare freed in a different "
           "graph...\n");
    checkValidationFailure(foundValidationFailure);

    printf("Cleaning up sample.\n");
    checkCudaErrors(cudaGraphExecDestroy(graphExec));
    checkCudaErrors(cudaGraphExecDestroy(graphExecFreeC));
    checkCudaErrors(cudaStreamDestroy(stream));
    checkCudaErrors(cudaFree(foundValidationFailure));
    checkCudaErrors(cudaFree(deviceRefArrays.negSquare));
    free(hostArrays.input);
    free(hostArrays.square);
    free(hostArrays.negSquare);
    printf("Cleanup complete. Exiting sample.\n");
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu`.

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

- **Total Lines**: 556
- **Approximate Size**: 22916 bytes

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
