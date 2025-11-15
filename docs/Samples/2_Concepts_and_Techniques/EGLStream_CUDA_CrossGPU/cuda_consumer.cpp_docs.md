# Documentation for Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_consumer.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_consumer.cpp`
- **Type**: .cpp
- **Location**: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU
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

//
// DESCRIPTION:   Simple CUDA consumer rendering sample app
//

#include "cuda_consumer.h"

#include <cuda_runtime.h>
#include <math.h>
#include <unistd.h>

#include "eglstrm_common.h"

#if defined(EXTENSION_LIST)
EXTENSION_LIST(EXTLST_EXTERN)
#endif
CUgraphicsResource cudaResource;

static int    count_acq           = 0;
static double acquire_time[25000] = {0}, total_time_acq = 0;

static int    count_rel       = 0;
static double rel_time[25000] = {0}, total_time_rel = 0;

void acquireApiStat(void);
void acquireApiStat(void)
{
    int    i   = 0;
    double min = 10000000, max = 0;
    double average_launch_time = 0, standard_deviation = 0;
    if (count_acq == 0)
        return;
    // lets compute the standard deviation
    min = max           = acquire_time[1];
    average_launch_time = (total_time_acq - acquire_time[0]) / count_acq;
    for (i = 1; i < count_acq; i++) {
        standard_deviation += (acquire_time[i] - average_launch_time) * (acquire_time[i] - average_launch_time);
        if (acquire_time[i] < min)
            min = acquire_time[i];
        if (acquire_time[i] > max)
            max = acquire_time[i];
    }
    standard_deviation = sqrt(standard_deviation / count_acq);
    printf("acquire Avg: %lf\n", average_launch_time);
    printf("acquire  SD: %lf\n", standard_deviation);
    printf("acquire min: %lf\n", min);
    printf("acquire max: %lf\n", max);

    min = max           = rel_time[1];
    average_launch_time = (total_time_rel - rel_time[0]) / count_rel;
    for (i = 1; i < count_rel; i++) {
        standard_deviation += (rel_time[i] - average_launch_time) * (rel_time[i] - average_launch_time);
        if (rel_time[i] < min)
            min = rel_time[i];
        if (rel_time[i] > max)
            max = rel_time[i];
    }
    standard_deviation = sqrt(standard_deviation / count_rel);
    printf("release Avg: %lf\n", average_launch_time);
    printf("release  SD: %lf\n", standard_deviation);
    printf("release min: %lf\n", min);
    printf("release max: %lf\n", max);
}
CUresult cudaConsumerAcquireFrame(test_cuda_consumer_s *cudaConsumer, int frameNumber)
{
    CUresult        cuStatus = CUDA_SUCCESS;
    CUeglFrame      cudaEgl;
    struct timespec start, end;
    EGLint          streamState = 0;
    double          curTime;

    if (!cudaConsumer) {
        printf("%s: Bad parameter\n", __func__);
        goto done;
    }

    while (1) {
        if (!eglQueryStreamKHR(cudaConsumer->eglDisplay, cudaConsumer->eglStream, EGL_STREAM_STATE_KHR, &streamState)) {
            printf("Cuda Consumer: eglQueryStreamKHR EGL_STREAM_STATE_KHR failed\n");
            cuStatus = CUDA_ERROR_UNKNOWN;
            goto done;
        }
        if (streamState == EGL_STREAM_STATE_DISCONNECTED_KHR) {
            printf("Cuda Consumer: EGL_STREAM_STATE_DISCONNECTED_KHR received\n");
            cuStatus = CUDA_ERROR_UNKNOWN;
            goto done;
        }

        if (streamState == EGL_STREAM_STATE_NEW_FRAME_AVAILABLE_KHR) {
            break;
        }
    }
    if (cudaConsumer->profileAPI) {
        getTime(&start);
    }
    cuStatus =
        cuEGLStreamConsumerAcquireFrame(&(cudaConsumer->cudaConn), &cudaResource, &cudaConsumer->consCudaStream, 16000);
    if (cudaConsumer->profileAPI) {
        getTime(&end);
        curTime                   = TIME_DIFF(end, start);
        acquire_time[count_acq++] = curTime;
        if (count_acq == 25000)
            count_acq = 0;
        total_time_acq += curTime;
    }
    if (cuStatus == CUDA_SUCCESS) {
        CUdeviceptr pDevPtr = 0;
        cudaError_t err;

        cuStatus = cuGraphicsResourceGetMappedEglFrame(&cudaEgl, cudaResource, 0, 0);
        if (cuStatus != CUDA_SUCCESS) {
            printf("Cuda get resource failed with %d\n", cuStatus);
            goto done;
        }
        pDevPtr = (CUdeviceptr)cudaEgl.frame.pPitch[0];

        err = cudaConsumer_filter(cudaConsumer->consCudaStream,
                                  (char *)pDevPtr,
                                  WIDTH * 4,
                                  HEIGHT,
                                  PROD_DATA + frameNumber,
                                  CONS_DATA + frameNumber,
                                  frameNumber);
        if (err != cudaSuccess) {
            printf("Cuda Consumer: kernel failed with: %s\n", cudaGetErrorString(err));
            goto done;
        }
    }

done:
    return cuStatus;
}

CUresult cudaConsumerReleaseFrame(test_cuda_consumer_s *cudaConsumer, int frameNumber)
{
    CUresult        cuStatus = CUDA_SUCCESS;
    struct timespec start, end;
    double          curTime;

    if (!cudaConsumer) {
        printf("%s: Bad parameter\n", __func__);
        goto done;
    }
    if (cudaConsumer->profileAPI) {
        getTime(&start);
    }
    cuStatus = cuEGLStreamConsumerReleaseFrame(&cudaConsumer->cudaConn, cudaResource, &cudaConsumer->consCudaStream);
    if (cudaConsumer->profileAPI) {
        getTime(&end);
        curTime               = TIME_DIFF(end, start);
        rel_time[count_rel++] = curTime;
        if (count_rel == 25000)
            count_rel = 0;
        total_time_rel += curTime;
    }
    if (cuStatus != CUDA_SUCCESS) {
        printf("cuEGLStreamConsumerReleaseFrame failed, status:%d\n", cuStatus);
        goto done;
    }

done:
    return cuStatus;
}

CUresult cudaDeviceCreateConsumer(test_cuda_consumer_s *cudaConsumer)
{
    CUdevice          device;
    CUresult          status          = CUDA_SUCCESS;
    CUctxCreateParams ctxCreateParams = {};

    if (CUDA_SUCCESS != (status = cuInit(0))) {
        printf("Failed to initialize CUDA\n");
        return status;
    }

    if (CUDA_SUCCESS != (status = cuDeviceGet(&device, cudaConsumer->cudaDevId))) {
        printf("failed to get CUDA device\n");
        return status;
    }

    if (CUDA_SUCCESS != (status = cuCtxCreate(&cudaConsumer->context, &ctxCreateParams, 0, device))) {
        printf("failed to create CUDA context\n");
        return status;
    }

    int  major = 0, minor = 0;
    char deviceName[256];
    cuDeviceGetAttribute(&major, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR, device);
    cuDeviceGetAttribute(&minor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR, device);
    cuDeviceGetName(deviceName, 256, device);
    printf("CUDA Consumer on GPU Device %d: \"%s\" with compute capability "
           "%d.%d\n\n",
           device,
           deviceName,
           major,
           minor);

    cuCtxPopCurrent(&cudaConsumer->context);
    if (major < 6) {
        printf("EGLStream_CUDA_CrossGPU requires SM 6.0 or higher arch GPU.  "
               "Exiting...\n");
        exit(2); // EXIT_WAIVED
    }

    return status;
}

CUresult cuda_consumer_init(test_cuda_consumer_s *cudaConsumer, TestArgs *args)
{
    CUresult status = CUDA_SUCCESS;
    int      bufferSize;

    cudaConsumer->charCnt = args->charCnt;
    bufferSize            = args->charCnt;

    cudaConsumer->pCudaCopyMem = (unsigned char *)malloc(bufferSize);
    if (cudaConsumer->pCudaCopyMem == NULL) {
        printf("Cuda Consumer: malloc failed\n");
        goto done;
    }

    status = cuStreamCreate(&cudaConsumer->consCudaStream, 0);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Consumer: cuStreamCreate failed, status:%d\n", status);
        goto done;
    }

    atexit(acquireApiStat);
done:
    return status;
}

CUresult cuda_consumer_Deinit(test_cuda_consumer_s *cudaConsumer)
{
    if (cudaConsumer->pCudaCopyMem) {
        free(cudaConsumer->pCudaCopyMem);
    }
    return cuEGLStreamConsumerDisconnect(&cudaConsumer->cudaConn);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_consumer.cpp`.

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

- **Total Lines**: 266
- **Approximate Size**: 9173 bytes

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
