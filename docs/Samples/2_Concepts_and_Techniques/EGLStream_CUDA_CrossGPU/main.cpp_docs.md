# Documentation for Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/main.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/main.cpp`
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

#include "cudaEGL.h"
#include "cuda_consumer.h"
#include "cuda_producer.h"
#include "eglstrm_common.h"
#include "helper.h"
#if defined(EXTENSION_LIST)
EXTENSION_LIST(EXTLST_EXTERN)
#endif

bool        signal_stop = 0;
extern bool verbose;

static void sig_handler(int sig)
{
    signal_stop = 1;
    printf("Signal: %d\n", sig);
}

void DoneCons(int consumerStatus, int send_fd)
{
    EGLStreamFini();
    // get the final status from producer, combine and print
    int producerStatus = -1;
    if (-1 == recv(send_fd, (void *)&producerStatus, sizeof(int), 0)) {
        printf("%s: Cuda Consumer could not receive status from producer.\n", __func__);
    }
    close(send_fd);

    if (producerStatus == 0 && consumerStatus == 0) {
        printf("&&&& EGLStream_CUDA_CrossGPU PASSED\n");
        exit(EXIT_SUCCESS);
    }
    else {
        printf("&&&& EGLStream_CUDA_CrossGPU FAILED\n");
        exit(EXIT_FAILURE);
    }
}

void DoneProd(int producerStatus, int connect_fd)
{
    EGLStreamFini();
    if (-1 == send(connect_fd, (void *)&producerStatus, sizeof(int), 0)) {
        printf("%s: Cuda Producer could not send status to consumer.\n", __func__);
    }
    close(connect_fd);
    if (producerStatus == 0) {
        exit(EXIT_SUCCESS);
    }
    else {
        exit(EXIT_FAILURE);
    }
}

int WIDTH = 8192, HEIGHT = 8192;
int main(int argc, char **argv)
{
    TestArgs                   args           = {0, false};
    CUresult                   curesult       = CUDA_SUCCESS;
    unsigned int               j              = 0;
    cudaError_t                err            = cudaSuccess;
    EGLNativeFileDescriptorKHR fileDescriptor = EGL_NO_FILE_DESCRIPTOR_KHR;
    struct timespec            start, end;
    CUeglFrame                 cudaEgl1, cudaEgl2;
    int                        consumerStatus = 0;
    int                        send_fd        = -1;

    if (parseCmdLine(argc, argv, &args) < 0) {
        printUsage();
        curesult = CUDA_ERROR_UNKNOWN;
        DoneCons(consumerStatus, send_fd);
    }

    printf("Width : %u, height: %u and iterations: %u\n", WIDTH, HEIGHT, NUMTRIALS);

    if (!args.isProducer) // Consumer code
    {
        test_cuda_consumer_s cudaConsumer;
        memset(&cudaConsumer, 0, sizeof(test_cuda_consumer_s));
        cudaConsumer.profileAPI = profileAPIs;

        // Hook up Ctrl-C handler
        signal(SIGINT, sig_handler);

        if (!EGLStreamInit(isCrossDevice, !args.isProducer, EGL_NO_FILE_DESCRIPTOR_KHR)) {
            printf("EGLStream Init failed.\n");
            curesult       = CUDA_ERROR_UNKNOWN;
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        cudaConsumer.cudaDevId = cudaDevIndexCons;
        curesult               = cudaDeviceCreateConsumer(&cudaConsumer);
        if (curesult != CUDA_SUCCESS) {
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        cuCtxPushCurrent(cudaConsumer.context);

        launchProducer(&args);

        args.charCnt = WIDTH * HEIGHT * 4;

        curesult = cuda_consumer_init(&cudaConsumer, &args);
        if (curesult != CUDA_SUCCESS) {
            printf("Cuda Consumer: Init failed, status: %d\n", curesult);
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        cuCtxPopCurrent(&cudaConsumer.context);

        send_fd = UnixSocketConnect(SOCK_PATH);
        if (-1 == send_fd) {
            printf("%s: Cuda Consumer cannot create socket %s\n", __func__, SOCK_PATH);
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        cuCtxPushCurrent(cudaConsumer.context);
        cudaConsumer.eglStream  = g_consumerEglStream;
        cudaConsumer.eglDisplay = g_consumerEglDisplay;

        // Send the EGL stream FD to producer
        fileDescriptor = eglGetStreamFileDescriptorKHR(cudaConsumer.eglDisplay, cudaConsumer.eglStream);
        if (EGL_NO_FILE_DESCRIPTOR_KHR == fileDescriptor) {
            printf("%s: Cuda Consumer could not get EGL file descriptor.\n", __func__);
            eglDestroyStreamKHR(cudaConsumer.eglDisplay, cudaConsumer.eglStream);
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        if (verbose)
            printf("%s: Cuda Consumer EGL stream FD obtained : %d.\n", __func__, fileDescriptor);

        int res = -1;
        res     = EGLStreamSendfd(send_fd, fileDescriptor);
        if (-1 == res) {
            printf("%s: Cuda Consumer could not send EGL file descriptor.\n", __func__);
            consumerStatus = -1;
            close(fileDescriptor);
        }

        if (CUDA_SUCCESS != (curesult = cuEGLStreamConsumerConnect(&(cudaConsumer.cudaConn), cudaConsumer.eglStream))) {
            printf("FAILED Connect CUDA consumer with error %d\n", curesult);
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        j = 0;
        for (j = 0; j < NUMTRIALS; j++) {
            curesult = cudaConsumerAcquireFrame(&cudaConsumer, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Consumer Test failed for frame = %d\n", j + 1);
                consumerStatus = -1;
                DoneCons(consumerStatus, send_fd);
            }
            curesult = cudaConsumerReleaseFrame(&cudaConsumer, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Consumer Test failed for frame = %d\n", j + 1);
                consumerStatus = -1;
                DoneCons(consumerStatus, send_fd);
            }

            curesult = cudaConsumerAcquireFrame(&cudaConsumer, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Consumer Test failed for frame = %d\n", j + 1);
                consumerStatus = -1;
                DoneCons(consumerStatus, send_fd);
            }
            curesult = cudaConsumerReleaseFrame(&cudaConsumer, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Consumer Test failed for frame = %d\n", j + 1);
                consumerStatus = -1;
                DoneCons(consumerStatus, send_fd);
            }
        }
        cuCtxSynchronize();
        close(fileDescriptor);
        err = cudaGetValueMismatch();
        if (err != cudaSuccess) {
            printf("Consumer: App failed with value mismatch\n");
            curesult       = CUDA_ERROR_UNKNOWN;
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        EGLint streamState = 0;
        if (!eglQueryStreamKHR(cudaConsumer.eglDisplay, cudaConsumer.eglStream, EGL_STREAM_STATE_KHR, &streamState)) {
            printf("Main, eglQueryStreamKHR EGL_STREAM_STATE_KHR failed\n");
            curesult       = CUDA_ERROR_UNKNOWN;
            consumerStatus = -1;
            DoneCons(consumerStatus, send_fd);
        }

        if (streamState != EGL_STREAM_STATE_DISCONNECTED_KHR) {
            if (CUDA_SUCCESS != (curesult = cuda_consumer_Deinit(&cudaConsumer))) {
                printf("Consumer Disconnect FAILED.\n");
                consumerStatus = -1;
                DoneCons(consumerStatus, send_fd);
            }
        }
    }
    else // Producer
    {
        test_cuda_producer_s cudaProducer;
        memset(&cudaProducer, 0, sizeof(test_cuda_producer_s));
        cudaProducer.profileAPI = profileAPIs;
        int producerStatus      = 0;

        setenv("CUDA_EGL_PRODUCER_RETURN_WAIT_TIMEOUT", "1600", 0);

        int connect_fd = -1;
        // Hook up Ctrl-C handler
        signal(SIGINT, sig_handler);

        // Create connection to Consumer
        connect_fd = UnixSocketCreate(SOCK_PATH);
        if (-1 == connect_fd) {
            printf("%s: Cuda Producer could not create socket: %s.\n", __func__, SOCK_PATH);
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        // Get the file descriptor of the stream from the consumer process
        // and re-create the EGL stream from it
        fileDescriptor = EGLStreamReceivefd(connect_fd);
        if (-1 == fileDescriptor) {
            printf("%s: Cuda Producer could not receive EGL file descriptor \n", __func__);
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        if (!EGLStreamInit(isCrossDevice, 0, fileDescriptor)) {
            printf("EGLStream Init failed.\n");
            producerStatus = -1;
            curesult       = CUDA_ERROR_UNKNOWN;
            DoneProd(producerStatus, connect_fd);
        }

        cudaProducer.eglDisplay = g_producerEglDisplay;
        cudaProducer.eglStream  = g_producerEglStream;
        cudaProducer.cudaDevId  = cudaDevIndexProd;

        curesult = cudaDeviceCreateProducer(&cudaProducer);
        if (curesult != CUDA_SUCCESS) {
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        args.charCnt = WIDTH * HEIGHT * 4;
        cuCtxPushCurrent(cudaProducer.context);
        curesult = cudaProducerInit(&cudaProducer, &args);
        if (curesult != CUDA_SUCCESS) {
            printf("Cuda Producer: Init failed, status: %d\n", curesult);
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        // wait for consumer to connect first
        int    err         = 0;
        int    wait_loop   = 0;
        EGLint streamState = 0;
        do {
            err =
                eglQueryStreamKHR(cudaProducer.eglDisplay, cudaProducer.eglStream, EGL_STREAM_STATE_KHR, &streamState);
            if ((0 != err) && (EGL_STREAM_STATE_CONNECTING_KHR != streamState)) {
                sleep(1);
                wait_loop++;
            }
        } while ((wait_loop < 10) && (0 != err) && (streamState != EGL_STREAM_STATE_CONNECTING_KHR));

        if ((0 == err) || (wait_loop >= 10)) {
            printf("%s: Cuda Producer eglQueryStreamKHR EGL_STREAM_STATE_KHR failed.\n", __func__);
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        if (CUDA_SUCCESS
            != (curesult =
                    cuEGLStreamProducerConnect(&(cudaProducer.cudaConn), cudaProducer.eglStream, WIDTH, HEIGHT))) {
            printf("Connect CUDA producer FAILED with error %d\n", curesult);
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        printf("main - Cuda Producer and Consumer Initialized.\n");

        cudaProducerPrepareFrame(&cudaEgl1, cudaProducer.cudaPtr, args.charCnt);
        cudaProducerPrepareFrame(&cudaEgl2, cudaProducer.cudaPtr1, args.charCnt);

        j = 0;
        for (j = 0; j < NUMTRIALS; j++) {
            curesult = cudaProducerPresentFrame(&cudaProducer, cudaEgl1, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Producer Test failed for frame = %d with cuda error:%d\n", j + 1, curesult);
                producerStatus = -1;
                DoneProd(producerStatus, connect_fd);
            }

            curesult = cudaProducerPresentFrame(&cudaProducer, cudaEgl2, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Producer Test failed for frame = %d with cuda error:%d\n", j + 1, curesult);
                producerStatus = -1;
                DoneProd(producerStatus, connect_fd);
            }

            curesult = cudaProducerReturnFrame(&cudaProducer, cudaEgl1, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Producer Test failed for frame = %d with cuda error:%d\n", j + 1, curesult);
                producerStatus = -1;
                DoneProd(producerStatus, connect_fd);
            }

            curesult = cudaProducerReturnFrame(&cudaProducer, cudaEgl2, j);
            if (curesult != CUDA_SUCCESS) {
                printf("Cuda Producer Test failed for frame = %d with cuda error:%d\n", j + 1, curesult);
                producerStatus = -1;
                DoneProd(producerStatus, connect_fd);
            }
        }

        cuCtxSynchronize();
        err = cudaGetValueMismatch();
        if (err != cudaSuccess) {
            printf("Prod: App failed with value mismatch\n");
            curesult       = CUDA_ERROR_UNKNOWN;
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        printf("Tear Down Start.....\n");
        if (!eglQueryStreamKHR(cudaProducer.eglDisplay, cudaProducer.eglStream, EGL_STREAM_STATE_KHR, &streamState)) {
            printf("Main, eglQueryStreamKHR EGL_STREAM_STATE_KHR failed\n");
            curesult       = CUDA_ERROR_UNKNOWN;
            producerStatus = -1;
            DoneProd(producerStatus, connect_fd);
        }

        if (streamState != EGL_STREAM_STATE_DISCONNECTED_KHR) {
            if (CUDA_SUCCESS != (curesult = cudaProducerDeinit(&cudaProducer))) {
                printf("Producer Disconnect FAILED with %d\n", curesult);
                producerStatus = -1;
                DoneProd(producerStatus, connect_fd);
            }
        }
        unsetenv("CUDA_EGL_PRODUCER_RETURN_WAIT_TIMEOUT");
    }

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/main.cpp`.

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

- **Total Lines**: 379
- **Approximate Size**: 14699 bytes

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
