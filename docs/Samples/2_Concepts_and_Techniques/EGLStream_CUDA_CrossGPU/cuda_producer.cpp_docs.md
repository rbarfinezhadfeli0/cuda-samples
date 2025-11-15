# Documentation for Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp`
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
// DESCRIPTION:   Simple cuda EGL stream producer app
//

#include "cuda_producer.h"

#include <cuda_runtime.h>

#include "cudaEGL.h"
#include "eglstrm_common.h"
#if defined(EXTENSION_LIST)
EXTENSION_LIST(EXTLST_EXTERN)
#endif
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "cuda_runtime.h"
#include "math.h"

int         cudaPresentReturnData = INIT_DATA;
int         fakePresent           = 0;
CUeglFrame  fakeFrame;
CUdeviceptr cudaPtrFake;
extern bool isCrossDevice;

void cudaProducerPrepareFrame(CUeglFrame *cudaEgl, CUdeviceptr cudaPtr, int bufferSize)
{
    cudaEgl->frame.pPitch[0] = (void *)cudaPtr;
    cudaEgl->width           = WIDTH;
    cudaEgl->depth           = 0;
    cudaEgl->height          = HEIGHT;
    cudaEgl->pitch           = WIDTH * 4;
    cudaEgl->frameType       = CU_EGL_FRAME_TYPE_PITCH;
    cudaEgl->planeCount      = 1;
    cudaEgl->numChannels     = 4;
    cudaEgl->eglColorFormat  = CU_EGL_COLOR_FORMAT_ARGB;
    cudaEgl->cuFormat        = CU_AD_FORMAT_UNSIGNED_INT8;
}

static int    count_present = 0, count_return = 0;
static double present_time[25000] = {0}, total_time_present = 0;
static double return_time[25000] = {0}, total_time_return = 0;

void presentApiStat(void);
void presentApiStat(void)
{
    int    i   = 0;
    double min = 10000000, max = 0;
    double average_launch_time = 0, standard_deviation = 0;
    if (count_present == 0)
        return;
    // lets compute the standard deviation
    min = max           = present_time[1];
    average_launch_time = (total_time_present) / count_present;
    for (i = 1; i < count_present; i++) {
        standard_deviation += (present_time[i] - average_launch_time) * (present_time[i] - average_launch_time);
        if (present_time[i] < min)
            min = present_time[i];
        if (present_time[i] > max)
            max = present_time[i];
    }
    standard_deviation = sqrt(standard_deviation / count_present);
    printf("present Avg: %lf\n", average_launch_time);
    printf("present  SD: %lf\n", standard_deviation);
    printf("present min: %lf\n", min);
    printf("present max: %lf\n", max);

    min = max           = return_time[1];
    average_launch_time = (total_time_return - return_time[0]) / count_return;
    for (i = 1; i < count_return; i++) {
        standard_deviation += (return_time[i] - average_launch_time) * (return_time[i] - average_launch_time);
        if (return_time[i] < min)
            min = return_time[i];
        if (return_time[i] > max)
            max = return_time[i];
    }
    standard_deviation = sqrt(standard_deviation / count_return);
    printf("return  Avg: %lf\n", average_launch_time);
    printf("return   SD: %lf\n", standard_deviation);
    printf("return  min: %lf\n", min);
    printf("return  max: %lf\n", max);
}
CUresult cudaProducerPresentFrame(test_cuda_producer_s *cudaProducer, CUeglFrame cudaEgl, int t)
{
    static int      flag   = 0;
    CUresult        status = CUDA_SUCCESS;
    struct timespec start, end;
    double          curTime;
    CUdeviceptr     pDevPtr = (CUdeviceptr)cudaEgl.frame.pPitch[0];
    cudaProducer_filter(
        cudaProducer->prodCudaStream, (char *)pDevPtr, WIDTH * 4, HEIGHT, cudaPresentReturnData, PROD_DATA + t, t);
    if (cudaProducer->profileAPI) {
        getTime(&start);
    }
    status = cuEGLStreamProducerPresentFrame(&cudaProducer->cudaConn, cudaEgl, &cudaProducer->prodCudaStream);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: Present frame failed, status:%d\n", status);
        goto done;
    }
    flag++;
    if (cudaProducer->profileAPI && flag > 10) {
        getTime(&end);
        curTime                       = TIME_DIFF(end, start);
        present_time[count_present++] = curTime;
        if (count_present == 25000)
            count_present = 0;
        total_time_present += curTime;
    }
done:
    return status;
}

int      flag = 0;
CUresult cudaProducerReturnFrame(test_cuda_producer_s *cudaProducer, CUeglFrame cudaEgl, int t)
{
    CUresult        status = CUDA_SUCCESS;
    struct timespec start, end;
    double          curTime;
    CUdeviceptr     pDevPtr = 0;

    pDevPtr = (CUdeviceptr)cudaEgl.frame.pPitch[0];
    if (cudaProducer->profileAPI) {
        getTime(&start);
    }

    while (1) {
        status = cuEGLStreamProducerReturnFrame(&cudaProducer->cudaConn, &cudaEgl, &cudaProducer->prodCudaStream);
        if (status == CUDA_ERROR_LAUNCH_TIMEOUT) {
            continue;
        }
        else if (status != CUDA_SUCCESS) {
            printf("Cuda Producer: Return frame failed, status:%d\n", status);
            goto done;
        }
        break;
    }
    if (cudaProducer->profileAPI) {
        getTime(&end);
        curTime                     = TIME_DIFF(end, start);
        return_time[count_return++] = curTime;
        if (count_return == 25000)
            count_return = 0;
        total_time_return += curTime;
    }
    if (flag % 2 == 0) {
        cudaPresentReturnData++;
    }
    cudaProducer_filter(
        cudaProducer->prodCudaStream, (char *)pDevPtr, WIDTH * 4, HEIGHT, CONS_DATA + t, cudaPresentReturnData, t);
    flag++;
done:
    return status;
}

CUresult cudaDeviceCreateProducer(test_cuda_producer_s *cudaProducer)
{
    CUdevice          device;
    CUresult          status          = CUDA_SUCCESS;
    CUctxCreateParams ctxCreateParams = {};

    if (CUDA_SUCCESS != (status = cuInit(0))) {
        printf("Failed to initialize CUDA\n");
        return status;
    }

    if (CUDA_SUCCESS != (status = cuDeviceGet(&device, cudaProducer->cudaDevId))) {
        printf("failed to get CUDA device\n");
        return status;
    }

    if (CUDA_SUCCESS != (status = cuCtxCreate(&cudaProducer->context, &ctxCreateParams, 0, device))) {
        printf("failed to create CUDA context\n");
        return status;
    }

    int  major = 0, minor = 0;
    char deviceName[256];
    cuDeviceGetAttribute(&major, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR, device);
    cuDeviceGetAttribute(&minor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR, device);
    cuDeviceGetName(deviceName, 256, device);
    printf("CUDA Producer on GPU Device %d: \"%s\" with compute capability "
           "%d.%d\n\n",
           device,
           deviceName,
           major,
           minor);

    cuCtxPopCurrent(&cudaProducer->context);

    if (major < 6) {
        printf("EGLStream_CUDA_CrossGPU requires SM 6.0 or higher arch GPU.  "
               "Exiting...\n");
        exit(2); // EXIT_WAIVED
    }

    return status;
}

CUresult cudaProducerInit(test_cuda_producer_s *cudaProducer, TestArgs *args)
{
    CUresult status = CUDA_SUCCESS;
    int      bufferSize;

    cudaProducer->charCnt = args->charCnt;
    bufferSize            = cudaProducer->charCnt;

    cudaProducer->tempBuff = (char *)malloc(bufferSize);
    if (!cudaProducer->tempBuff) {
        printf("Cuda Producer: Failed to allocate image buffer\n");
        status = CUDA_ERROR_UNKNOWN;
        goto done;
    }
    memset((void *)cudaProducer->tempBuff, INIT_DATA, cudaProducer->charCnt);

    // Fill this init data
    status = cuMemAlloc(&cudaProducer->cudaPtr, bufferSize);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: cuda Malloc failed, status:%d\n", status);
        goto done;
    }
    status = cuMemcpyHtoD(cudaProducer->cudaPtr, (void *)(cudaProducer->tempBuff), bufferSize);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: cuMemCpy failed, status:%d\n", status);
        goto done;
    }

    // Fill this init data
    status = cuMemAlloc(&cudaProducer->cudaPtr1, bufferSize);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: cuda Malloc failed, status:%d\n", status);
        goto done;
    }
    status = cuMemcpyHtoD(cudaProducer->cudaPtr1, (void *)(cudaProducer->tempBuff), bufferSize);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: cuMemCpy failed, status:%d\n", status);
        goto done;
    }

    status = cuStreamCreate(&cudaProducer->prodCudaStream, 0);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: cuStreamCreate failed, status:%d\n", status);
        goto done;
    }

    // Fill this init data
    status = cuMemAlloc(&cudaPtrFake, 100);
    if (status != CUDA_SUCCESS) {
        printf("Cuda Producer: cuda Malloc failed, status:%d\n", status);
        goto done;
    }

    atexit(presentApiStat);
done:
    return status;
}

CUresult cudaProducerDeinit(test_cuda_producer_s *cudaProducer)
{
    if (cudaProducer->tempBuff) {
        free(cudaProducer->tempBuff);
    }
    if (cudaProducer->cudaPtr) {
        cuMemFree(cudaProducer->cudaPtr);
    }
    return cuEGLStreamProducerDisconnect(&cudaProducer->cudaConn);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp`.

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

- **Total Lines**: 295
- **Approximate Size**: 10305 bytes

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
