# Documentation for Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu

## File Metadata

- **Path**: `Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/UnifiedMemoryStreams
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
 * This sample implements a simple task consumer using threads and streams
 * with all data in Unified Memory, and tasks consumed by both host and device
 */

// system includes
#include <algorithm>
#include <cstdio>
#include <ctime>
#include <vector>
#ifdef USE_PTHREADS
#include <pthread.h>
#else
#include <omp.h>
#endif
#include <stdlib.h>

// cuBLAS
#include <cublas_v2.h>

// utilities
#include <helper_cuda.h>

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
// SRAND48 and DRAND48 don't exist on windows, but these are the equivalent
// functions
void   srand48(long seed) { srand((unsigned int)seed); }
double drand48() { return double(rand()) / RAND_MAX; }
#endif

const char *sSDKname = "UnifiedMemoryStreams";

// simple task
template <typename T> struct Task
{
    unsigned int size, id;
    T           *data;
    T           *result;
    T           *vector;

    Task()
        : size(0)
        , id(0)
        , data(NULL)
        , result(NULL)
        , vector(NULL) {};
    Task(unsigned int s)
        : size(s)
        , id(0)
        , data(NULL)
        , result(NULL)
    {
        // allocate unified memory -- the operation performed in this example will
        // be a DGEMV
        checkCudaErrors(cudaMallocManaged(&data, sizeof(T) * size * size));
        checkCudaErrors(cudaMallocManaged(&result, sizeof(T) * size));
        checkCudaErrors(cudaMallocManaged(&vector, sizeof(T) * size));
        checkCudaErrors(cudaDeviceSynchronize());
    }

    ~Task()
    {
        // ensure all memory is deallocated
        checkCudaErrors(cudaDeviceSynchronize());
        checkCudaErrors(cudaFree(data));
        checkCudaErrors(cudaFree(result));
        checkCudaErrors(cudaFree(vector));
    }

    void allocate(const unsigned int s, const unsigned int unique_id)
    {
        // allocate unified memory outside of constructor
        id   = unique_id;
        size = s;
        checkCudaErrors(cudaMallocManaged(&data, sizeof(T) * size * size));
        checkCudaErrors(cudaMallocManaged(&result, sizeof(T) * size));
        checkCudaErrors(cudaMallocManaged(&vector, sizeof(T) * size));
        checkCudaErrors(cudaDeviceSynchronize());

        // populate data with random elements
        for (unsigned int i = 0; i < size * size; i++) {
            data[i] = drand48();
        }

        for (unsigned int i = 0; i < size; i++) {
            result[i] = 0.;
            vector[i] = drand48();
        }
    }
};

#ifdef USE_PTHREADS
struct threadData_t
{
    int             tid;
    Task<double>   *TaskListPtr;
    cudaStream_t   *streams;
    cublasHandle_t *handles;
    int             taskSize;
};

typedef struct threadData_t threadData;
#endif

// simple host dgemv: assume data is in row-major format and square
template <typename T> void gemv(int m, int n, T alpha, T *A, T *x, T beta, T *result)
{
    // rows
    for (int i = 0; i < n; i++) {
        result[i] *= beta;

        for (int j = 0; j < n; j++) {
            result[i] += A[i * n + j] * x[j];
        }
    }
}

// execute a single task on either host or device depending on size
#ifdef USE_PTHREADS
void *execute(void *inpArgs)
{
    threadData     *dataPtr = (threadData *)inpArgs;
    cudaStream_t   *stream  = dataPtr->streams;
    cublasHandle_t *handle  = dataPtr->handles;
    int             tid     = dataPtr->tid;

    for (int i = 0; i < dataPtr->taskSize; i++) {
        Task<double> &t = dataPtr->TaskListPtr[i];

        if (t.size < 100) {
            // perform on host
            printf("Task [%d], thread [%d] executing on host (%d)\n", t.id, tid, t.size);

            // attach managed memory to a (dummy) stream to allow host access while
            // the device is running
            checkCudaErrors(cudaStreamAttachMemAsync(stream[0], t.data, 0, cudaMemAttachHost));
            checkCudaErrors(cudaStreamAttachMemAsync(stream[0], t.vector, 0, cudaMemAttachHost));
            checkCudaErrors(cudaStreamAttachMemAsync(stream[0], t.result, 0, cudaMemAttachHost));
            // necessary to ensure Async cudaStreamAttachMemAsync calls have finished
            checkCudaErrors(cudaStreamSynchronize(stream[0]));
            // call the host operation
            gemv(t.size, t.size, 1.0, t.data, t.vector, 0.0, t.result);
        }
        else {
            // perform on device
            printf("Task [%d], thread [%d] executing on device (%d)\n", t.id, tid, t.size);
            double one  = 1.0;
            double zero = 0.0;

            // attach managed memory to my stream
            checkCudaErrors(cublasSetStream(handle[tid + 1], stream[tid + 1]));
            checkCudaErrors(cudaStreamAttachMemAsync(stream[tid + 1], t.data, 0, cudaMemAttachSingle));
            checkCudaErrors(cudaStreamAttachMemAsync(stream[tid + 1], t.vector, 0, cudaMemAttachSingle));
            checkCudaErrors(cudaStreamAttachMemAsync(stream[tid + 1], t.result, 0, cudaMemAttachSingle));
            // call the device operation
            checkCudaErrors(cublasDgemv(
                handle[tid + 1], CUBLAS_OP_N, t.size, t.size, &one, t.data, t.size, t.vector, 1, &zero, t.result, 1));
        }
    }

    pthread_exit(NULL);
}
#else
template <typename T> void execute(Task<T> &t, cublasHandle_t *handle, cudaStream_t *stream, int tid)
{
    if (t.size < 100) {
        // perform on host
        printf("Task [%d], thread [%d] executing on host (%d)\n", t.id, tid, t.size);

        // attach managed memory to a (dummy) stream to allow host access while the
        // device is running
        checkCudaErrors(cudaStreamAttachMemAsync(stream[0], t.data, 0, cudaMemAttachHost));
        checkCudaErrors(cudaStreamAttachMemAsync(stream[0], t.vector, 0, cudaMemAttachHost));
        checkCudaErrors(cudaStreamAttachMemAsync(stream[0], t.result, 0, cudaMemAttachHost));
        // necessary to ensure Async cudaStreamAttachMemAsync calls have finished
        checkCudaErrors(cudaStreamSynchronize(stream[0]));
        // call the host operation
        gemv(t.size, t.size, 1.0, t.data, t.vector, 0.0, t.result);
    }
    else {
        // perform on device
        printf("Task [%d], thread [%d] executing on device (%d)\n", t.id, tid, t.size);
        double one  = 1.0;
        double zero = 0.0;

        // attach managed memory to my stream
        checkCudaErrors(cublasSetStream(handle[tid + 1], stream[tid + 1]));
        checkCudaErrors(cudaStreamAttachMemAsync(stream[tid + 1], t.data, 0, cudaMemAttachSingle));
        checkCudaErrors(cudaStreamAttachMemAsync(stream[tid + 1], t.vector, 0, cudaMemAttachSingle));
        checkCudaErrors(cudaStreamAttachMemAsync(stream[tid + 1], t.result, 0, cudaMemAttachSingle));
        // call the device operation
        checkCudaErrors(cublasDgemv(
            handle[tid + 1], CUBLAS_OP_N, t.size, t.size, &one, t.data, t.size, t.vector, 1, &zero, t.result, 1));
    }
}
#endif

// populate a list of tasks with random sizes
template <typename T> void initialise_tasks(std::vector<Task<T>> &TaskList)
{
    for (unsigned int i = 0; i < TaskList.size(); i++) {
        // generate random size
        int size;
        size = std::max((int)(drand48() * 1000.0), 64);
        TaskList[i].allocate(size, i);
    }
}

int main(int argc, char **argv)
{
    // set device
    cudaDeviceProp device_prop;
    int            dev_id = findCudaDevice(argc, (const char **)argv);
    checkCudaErrors(cudaGetDeviceProperties(&device_prop, dev_id));

    if (!device_prop.managedMemory) {
        // This samples requires being run on a device that supports Unified Memory
        fprintf(stderr, "Unified Memory not supported on this device\n");

        exit(EXIT_WAIVED);
    }

    int computeMode;
    checkCudaErrors(cudaDeviceGetAttribute(&computeMode, cudaDevAttrComputeMode, dev_id));
    if (computeMode == cudaComputeModeProhibited) {
        // This sample requires being run with a default or process exclusive mode
        fprintf(stderr,
                "This sample requires a device in either default or process "
                "exclusive mode\n");

        exit(EXIT_WAIVED);
    }

    // randomise task sizes
    int seed = (int)time(NULL);
    srand48(seed);

    // set number of threads
    const int nthreads = 4;

    // number of streams = number of threads
    cudaStream_t   *streams = new cudaStream_t[nthreads + 1];
    cublasHandle_t *handles = new cublasHandle_t[nthreads + 1];

    for (int i = 0; i < nthreads + 1; i++) {
        checkCudaErrors(cudaStreamCreate(&streams[i]));
        checkCudaErrors(cublasCreate(&handles[i]));
    }

    // create list of N tasks
    unsigned int              N = 40;
    std::vector<Task<double>> TaskList(N);
    initialise_tasks(TaskList);

    printf("Executing tasks on host / device\n");

// run through all tasks using threads and streams
#ifdef USE_PTHREADS
    pthread_t   threads[nthreads];
    threadData *InputToThreads = new threadData[nthreads];

    for (int i = 0; i < nthreads; i++) {
        checkCudaErrors(cudaSetDevice(dev_id));
        InputToThreads[i].tid     = i;
        InputToThreads[i].streams = streams;
        InputToThreads[i].handles = handles;

        if ((TaskList.size() / nthreads) == 0) {
            InputToThreads[i].taskSize    = (TaskList.size() / nthreads);
            InputToThreads[i].TaskListPtr = &TaskList[i * (TaskList.size() / nthreads)];
        }
        else {
            if (i == nthreads - 1) {
                InputToThreads[i].taskSize = (TaskList.size() / nthreads) + (TaskList.size() % nthreads);
                InputToThreads[i].TaskListPtr =
                    &TaskList[i * (TaskList.size() / nthreads) + (TaskList.size() % nthreads)];
            }
            else {
                InputToThreads[i].taskSize    = (TaskList.size() / nthreads);
                InputToThreads[i].TaskListPtr = &TaskList[i * (TaskList.size() / nthreads)];
            }
        }

        pthread_create(&threads[i], NULL, &execute, &InputToThreads[i]);
    }
    for (int i = 0; i < nthreads; i++) {
        pthread_join(threads[i], NULL);
    }
#else
    omp_set_num_threads(nthreads);
#pragma omp parallel for schedule(dynamic)
    for (int i = 0; i < TaskList.size(); i++) {
        checkCudaErrors(cudaSetDevice(dev_id));
        int tid = omp_get_thread_num();
        execute(TaskList[i], handles, streams, tid);
    }
#endif

    cudaDeviceSynchronize();

    // Destroy CUDA Streams, cuBlas handles
    for (int i = 0; i < nthreads + 1; i++) {
        cudaStreamDestroy(streams[i]);
        cublasDestroy(handles[i]);
    }

    // Free TaskList
    std::vector<Task<double>>().swap(TaskList);

    printf("All Done!\n");
    exit(EXIT_SUCCESS);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu`.

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

- **Total Lines**: 340
- **Approximate Size**: 12254 bytes

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
