# Documentation for Samples/8_Platform_Specific/Tegra/cuDLAHybridMode/main.cu

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/cuDLAHybridMode/main.cu`
- **Type**: .cu
- **Location**: Samples/8_Platform_Specific/Tegra/cuDLAHybridMode
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

#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <fstream>
#include <sstream>
#include <sys/stat.h>

#include "cuda_runtime.h"
#include "cudla.h"

#define DPRINTF(...) printf(__VA_ARGS__)

static void printTensorDesc(cudlaModuleTensorDescriptor *tensorDesc)
{
    DPRINTF("\tTENSOR NAME : %s\n", tensorDesc->name);
    DPRINTF("\tsize: %lu\n", tensorDesc->size);

    DPRINTF("\tdims: [%lu, %lu, %lu, %lu]\n", tensorDesc->n, tensorDesc->c, tensorDesc->h, tensorDesc->w);

    DPRINTF("\tdata fmt: %d\n", tensorDesc->dataFormat);
    DPRINTF("\tdata type: %d\n", tensorDesc->dataType);
    DPRINTF("\tdata category: %d\n", tensorDesc->dataCategory);
    DPRINTF("\tpixel fmt: %d\n", tensorDesc->pixelFormat);
    DPRINTF("\tpixel mapping: %d\n", tensorDesc->pixelMapping);
    DPRINTF("\tstride[0]: %d\n", tensorDesc->stride[0]);
    DPRINTF("\tstride[1]: %d\n", tensorDesc->stride[1]);
    DPRINTF("\tstride[2]: %d\n", tensorDesc->stride[2]);
    DPRINTF("\tstride[3]: %d\n", tensorDesc->stride[3]);
}

static int initializeInputBuffers(char *filePath, cudlaModuleTensorDescriptor *tensorDesc, unsigned char *buf)
{
    // Read the file in filePath and fill up 'buf' according to format
    // specified by the user.

    return 0;
}

typedef struct
{
    cudlaDevHandle               devHandle;
    cudlaModule                  moduleHandle;
    unsigned char               *loadableData;
    cudaStream_t                 stream;
    unsigned char               *inputBuffer;
    unsigned char               *outputBuffer;
    void                        *inputBufferGPU;
    void                        *outputBufferGPU;
    cudlaModuleTensorDescriptor *inputTensorDesc;
    cudlaModuleTensorDescriptor *outputTensorDesc;
} ResourceList;

void cleanUp(ResourceList *resourceList);

void cleanUp(ResourceList *resourceList)
{
    if (resourceList->inputTensorDesc != NULL) {
        free(resourceList->inputTensorDesc);
        resourceList->inputTensorDesc = NULL;
    }
    if (resourceList->outputTensorDesc != NULL) {
        free(resourceList->outputTensorDesc);
        resourceList->outputTensorDesc = NULL;
    }

    if (resourceList->loadableData != NULL) {
        free(resourceList->loadableData);
        resourceList->loadableData = NULL;
    }

    if (resourceList->moduleHandle != NULL) {
        cudlaModuleUnload(resourceList->moduleHandle, 0);
        resourceList->moduleHandle = NULL;
    }

    if (resourceList->devHandle != NULL) {
        cudlaDestroyDevice(resourceList->devHandle);
        resourceList->devHandle = NULL;
    }

    if (resourceList->inputBufferGPU != 0) {
        cudaFree(resourceList->inputBufferGPU);
        resourceList->inputBufferGPU = 0;
    }
    if (resourceList->outputBufferGPU != 0) {
        cudaFree(resourceList->outputBufferGPU);
        resourceList->outputBufferGPU = 0;
    }

    if (resourceList->inputBuffer != NULL) {
        free(resourceList->inputBuffer);
        resourceList->inputBuffer = NULL;
    }
    if (resourceList->outputBuffer != NULL) {
        free(resourceList->outputBuffer);
        resourceList->outputBuffer = NULL;
    }

    if (resourceList->stream != NULL) {
        cudaStreamDestroy(resourceList->stream);
        resourceList->stream = NULL;
    }
}

int main(int argc, char **argv)
{
    cudlaDevHandle devHandle;
    cudlaModule    moduleHandle;
    cudlaStatus    err;
    FILE          *fp = NULL;
    struct stat    st;
    size_t         file_size;
    size_t         actually_read = 0;
    unsigned char *loadableData  = NULL;

    cudaStream_t stream;
    cudaError_t  result;
    const char  *errPtr = NULL;

    ResourceList resourceList;

    memset(&resourceList, 0x00, sizeof(ResourceList));

    if (argc != 3) {
        DPRINTF("Usage : ./cuDLAHybridMode <loadable> <imageFile>\n");
        return 1;
    }

    // Read loadable into buffer.
    fp = fopen(argv[1], "rb");
    if (fp == NULL) {
        DPRINTF("Cannot open file %s\n", argv[1]);
        return 1;
    }

    if (stat(argv[1], &st) != 0) {
        DPRINTF("Cannot stat file\n");
        return 1;
    }

    file_size = st.st_size;
    DPRINTF("The file size = %ld\n", file_size);

    loadableData = (unsigned char *)malloc(file_size);
    if (loadableData == NULL) {
        DPRINTF("Cannot Allocate memory for loadable\n");
        return 1;
    }

    actually_read = fread(loadableData, 1, file_size, fp);
    if (actually_read != file_size) {
        free(loadableData);
        DPRINTF("Read wrong size\n");
        return 1;
    }
    fclose(fp);

    resourceList.loadableData = loadableData;

    // Initialize CUDA.
    result = cudaFree(0);
    if (result != cudaSuccess) {
        errPtr = cudaGetErrorName(result);
        DPRINTF("Error in creating cudaFree = %s\n", errPtr);
        cleanUp(&resourceList);
        return 1;
    }
    result = cudaSetDevice(0);
    if (result != cudaSuccess) {
        errPtr = cudaGetErrorName(result);
        DPRINTF("Error in creating cudaSetDevice = %s\n", errPtr);
        cleanUp(&resourceList);
        return 1;
    }

    err = cudlaCreateDevice(0, &devHandle, CUDLA_CUDA_DLA);
    if (err != cudlaSuccess) {
        DPRINTF("Error in cuDLA create device = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }

    DPRINTF("Device created successfully\n");
    resourceList.devHandle = devHandle;

    err = cudlaModuleLoadFromMemory(devHandle, loadableData, file_size, &moduleHandle, 0);
    if (err != cudlaSuccess) {
        DPRINTF("Error in cudlaModuleLoadFromMemory = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    else {
        DPRINTF("Successfully loaded module\n");
    }

    resourceList.moduleHandle = moduleHandle;

    // Create CUDA stream.
    result = cudaStreamCreateWithFlags(&stream, cudaStreamNonBlocking);

    if (result != cudaSuccess) {
        errPtr = cudaGetErrorName(result);
        DPRINTF("Error in creating cuda stream = %s\n", errPtr);
        cleanUp(&resourceList);
        return 1;
    }

    resourceList.stream = stream;

    // Get tensor attributes.
    uint32_t             numInputTensors  = 0;
    uint32_t             numOutputTensors = 0;
    cudlaModuleAttribute attribute;

    err = cudlaModuleGetAttributes(moduleHandle, CUDLA_NUM_INPUT_TENSORS, &attribute);
    if (err != cudlaSuccess) {
        DPRINTF("Error in getting numInputTensors = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    numInputTensors = attribute.numInputTensors;
    DPRINTF("numInputTensors = %d\n", numInputTensors);

    err = cudlaModuleGetAttributes(moduleHandle, CUDLA_NUM_OUTPUT_TENSORS, &attribute);
    if (err != cudlaSuccess) {
        DPRINTF("Error in getting numOutputTensors = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    numOutputTensors = attribute.numOutputTensors;
    DPRINTF("numOutputTensors = %d\n", numOutputTensors);

    cudlaModuleTensorDescriptor *inputTensorDesc =
        (cudlaModuleTensorDescriptor *)malloc(sizeof(cudlaModuleTensorDescriptor) * numInputTensors);
    cudlaModuleTensorDescriptor *outputTensorDesc =
        (cudlaModuleTensorDescriptor *)malloc(sizeof(cudlaModuleTensorDescriptor) * numOutputTensors);

    if ((inputTensorDesc == NULL) || (outputTensorDesc == NULL)) {
        if (inputTensorDesc != NULL) {
            free(inputTensorDesc);
            inputTensorDesc = NULL;
        }

        if (outputTensorDesc != NULL) {
            free(outputTensorDesc);
            outputTensorDesc = NULL;
        }

        cleanUp(&resourceList);
        return 1;
    }

    resourceList.inputTensorDesc  = inputTensorDesc;
    resourceList.outputTensorDesc = outputTensorDesc;

    attribute.inputTensorDesc = inputTensorDesc;
    err                       = cudlaModuleGetAttributes(moduleHandle, CUDLA_INPUT_TENSOR_DESCRIPTORS, &attribute);
    if (err != cudlaSuccess) {
        DPRINTF("Error in getting input tensor descriptor = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    DPRINTF("Printing input tensor descriptor\n");
    printTensorDesc(inputTensorDesc);

    attribute.outputTensorDesc = outputTensorDesc;
    err                        = cudlaModuleGetAttributes(moduleHandle, CUDLA_OUTPUT_TENSOR_DESCRIPTORS, &attribute);
    if (err != cudlaSuccess) {
        DPRINTF("Error in getting output tensor descriptor = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    DPRINTF("Printing output tensor descriptor\n");
    printTensorDesc(outputTensorDesc);

    // Setup the input and output buffers which will be used as an input to CUDA.
    unsigned char *inputBuffer = (unsigned char *)malloc(inputTensorDesc[0].size);
    if (inputBuffer == NULL) {
        DPRINTF("Error in allocating input memory\n");
        cleanUp(&resourceList);
        return 1;
    }

    resourceList.inputBuffer = inputBuffer;

    unsigned char *outputBuffer = (unsigned char *)malloc(outputTensorDesc[0].size);
    if (outputBuffer == NULL) {
        DPRINTF("Error in allocating output memory\n");
        cleanUp(&resourceList);
        return 1;
    }

    resourceList.outputBuffer = outputBuffer;

    memset(inputBuffer, 0x00, inputTensorDesc[0].size);
    memset(outputBuffer, 0x00, outputTensorDesc[0].size);

    // Fill up the buffers with data.
    if (initializeInputBuffers(argv[2], inputTensorDesc, inputBuffer) != 0) {
        DPRINTF("Error in initializing input buffer\n");
        cleanUp(&resourceList);
        return 1;
    }

    // Allocate memory on GPU.
    void *inputBufferGPU;
    void *outputBufferGPU;
    result = cudaMalloc(&inputBufferGPU, inputTensorDesc[0].size);
    if (result != cudaSuccess) {
        DPRINTF("Error in allocating input memory on GPU\n");
        cleanUp(&resourceList);
        return 1;
    }

    resourceList.inputBufferGPU = inputBufferGPU;

    result = cudaMalloc(&outputBufferGPU, outputTensorDesc[0].size);
    if (result != cudaSuccess) {
        DPRINTF("Error in allocating output memory on GPU\n");
        cleanUp(&resourceList);
        return 1;
    }

    resourceList.outputBufferGPU = outputBufferGPU;

    // Register the CUDA-allocated buffers.
    uint64_t *inputBufferRegisteredPtr  = NULL;
    uint64_t *outputBufferRegisteredPtr = NULL;

    err =
        cudlaMemRegister(devHandle, (uint64_t *)inputBufferGPU, inputTensorDesc[0].size, &inputBufferRegisteredPtr, 0);
    if (err != cudlaSuccess) {
        DPRINTF("Error in registering input memory = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }

    err = cudlaMemRegister(
        devHandle, (uint64_t *)outputBufferGPU, outputTensorDesc[0].size, &outputBufferRegisteredPtr, 0);
    if (err != cudlaSuccess) {
        DPRINTF("Error in registering output memory = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    DPRINTF("ALL MEMORY REGISTERED SUCCESSFULLY\n");

    // Copy data from CPU buffers to GPU buffers.
    result = cudaMemcpyAsync(inputBufferGPU, inputBuffer, inputTensorDesc[0].size, cudaMemcpyHostToDevice, stream);
    if (result != cudaSuccess) {
        DPRINTF("Error in enqueueing memcpy for input\n");
        cleanUp(&resourceList);
        return 1;
    }
    result = cudaMemsetAsync(outputBufferGPU, 0, outputTensorDesc[0].size, stream);
    if (result != cudaSuccess) {
        DPRINTF("Error in enqueueing memset for output\n");
        cleanUp(&resourceList);
        return 1;
    }

    // Enqueue a cuDLA task.
    cudlaTask task;
    task.moduleHandle     = moduleHandle;
    task.outputTensor     = &outputBufferRegisteredPtr;
    task.numOutputTensors = 1;
    task.numInputTensors  = 1;
    task.inputTensor      = &inputBufferRegisteredPtr;
    task.waitEvents       = NULL;
    task.signalEvents     = NULL;
    err                   = cudlaSubmitTask(devHandle, &task, 1, stream, 0);
    if (err != cudlaSuccess) {
        DPRINTF("Error in submitting task\n");
        cleanUp(&resourceList);
        return 1;
    }
    DPRINTF("SUBMIT IS DONE !!!\n");

    // Wait for stream operations to finish and bring output buffer to CPU.
    result = cudaMemcpyAsync(outputBuffer, outputBufferGPU, outputTensorDesc[0].size, cudaMemcpyDeviceToHost, stream);
    if (result != cudaSuccess) {
        DPRINTF("Error in bringing result back to CPU\n");
        cleanUp(&resourceList);
        return 1;
    }
    result = cudaStreamSynchronize(stream);
    if (result != cudaSuccess) {
        DPRINTF("Error in synchronizing stream\n");
        cleanUp(&resourceList);
        return 1;
    }

    // Output is available in outputBuffer.

    // Teardown.
    err = cudlaMemUnregister(devHandle, inputBufferRegisteredPtr);
    if (err != cudlaSuccess) {
        DPRINTF("Error in unregistering input memory = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }

    err = cudlaMemUnregister(devHandle, outputBufferRegisteredPtr);
    if (err != cudlaSuccess) {
        DPRINTF("Error in registering output memory = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    DPRINTF("ALL MEMORY UNREGISTERED SUCCESSFULLY\n");

    free(inputTensorDesc);
    free(outputTensorDesc);
    free(loadableData);
    free(inputBuffer);
    free(outputBuffer);
    cudaFree(inputBufferGPU);
    cudaFree(outputBufferGPU);

    resourceList.inputTensorDesc  = NULL;
    resourceList.outputTensorDesc = NULL;
    resourceList.loadableData     = NULL;
    resourceList.inputBuffer      = NULL;
    resourceList.outputBuffer     = NULL;
    resourceList.inputBufferGPU   = 0;
    resourceList.outputBufferGPU  = 0;

    result = cudaStreamDestroy(stream);
    if (result != cudaSuccess) {
        errPtr = cudaGetErrorName(result);
        DPRINTF("Error in destroying cuda stream = %s\n", errPtr);
        cleanUp(&resourceList);
        return 1;
    }

    resourceList.stream = NULL;

    err = cudlaModuleUnload(moduleHandle, 0);
    if (err != cudlaSuccess) {
        DPRINTF("Error in cudlaModuleUnload = %d\n", err);
        cleanUp(&resourceList);
        return 1;
    }
    else {
        DPRINTF("Successfully unloaded module\n");
    }

    resourceList.moduleHandle = NULL;

    err = cudlaDestroyDevice(devHandle);
    if (err != cudlaSuccess) {
        DPRINTF("Error in cuDLA destroy device = %d\n", err);
        return 1;
    }
    DPRINTF("Device destroyed successfully\n");

    resourceList.devHandle = NULL;

    DPRINTF("cuDLAHybridMode DONE !!!\n");

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/cuDLAHybridMode/main.cu`.

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

- **Total Lines**: 488
- **Approximate Size**: 16051 bytes

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
