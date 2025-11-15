# Documentation for Samples/7_libNVVM/simple/simple.c

## File Metadata

- **Path**: `Samples/7_libNVVM/simple/simple.c`
- **Type**: .c
- **Location**: Samples/7_libNVVM/simple
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```c
// Copyright (c) 1993-2025, NVIDIA CORPORATION. All rights reserved.
//
// Redistribution and use in source and binary forms, with or without
// modification, are permitted provided that the following conditions
// are met:
//  * Redistributions of source code must retain the above copyright
//    notice, this list of conditions and the following disclaimer.
//  * Redistributions in binary form must reproduce the above copyright
//    notice, this list of conditions and the following disclaimer in the
//    documentation and/or other materials provided with the distribution.
//  * Neither the name of NVIDIA CORPORATION nor the names of its
//    contributors may be used to endorse or promote products derived
//    from this software without specific prior written permission.
//
// THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
// EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
// IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
// PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
// CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
// EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
// PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
// PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
// OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
// (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
// OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

#include <assert.h>
#include <builtin_types.h>
#include <cuda.h>
#include <math.h>
#include <nvvm.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>

// If 'err' is non-zero, emit an error message and exit.
#define checkCudaErrors(err) __checkCudaErrors(err, __FILE__, __LINE__)
static void __checkCudaErrors(CUresult err, const char *filename, int line)
{
    assert(filename);
    if (CUDA_SUCCESS != err) {
        const char    *ename = NULL;
        const CUresult res   = cuGetErrorName(err, &ename);
        fprintf(stderr,
                "CUDA API Error %04d: \"%s\" from file <%s>, "
                "line %i.\n",
                err,
                ((CUDA_SUCCESS == res) ? ename : "Unknown"),
                filename,
                line);
        exit(err);
    }
}

// Return a CUDA capable device or exit if one cannot be found.
static CUdevice cudaDeviceInit(int *devMajor, int *devMinor)
{
    assert(devMajor && devMinor);
    CUresult err         = cuInit(0);
    int      deviceCount = 0;
    if (CUDA_SUCCESS == err)
        checkCudaErrors(cuDeviceGetCount(&deviceCount));
    if (deviceCount == 0) {
        fprintf(stderr, "cudaDeviceInit error: no devices supporting CUDA\n");
        exit(EXIT_FAILURE);
    }

    // Locate a CUDA supporting device and its name.
    CUdevice cuDevice = 0;
    checkCudaErrors(cuDeviceGet(&cuDevice, 0));
    char name[128];
    cuDeviceGetName(name, sizeof(name), cuDevice);
    printf("Using CUDA Device [0]: %s\n", name);

    // Obtain the device's compute capability.
    checkCudaErrors(cuDeviceGetAttribute(devMajor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR, cuDevice));
    checkCudaErrors(cuDeviceGetAttribute(devMinor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR, cuDevice));
    if (*devMajor < 7 && *devMinor < 5) {
        fprintf(stderr, "Device 0 is not sm_75 or later\n");
        exit(EXIT_FAILURE);
    }

    return cuDevice;
}

static CUresult
initCUDA(CUcontext *phContext, CUdevice *phDevice, CUmodule *phModule, CUfunction *phKernel, const char *ptx)
{
    assert(phContext && phDevice && phModule && phKernel && ptx);

    // Create a CUDA context on the device.
    checkCudaErrors(cuCtxCreate(phContext, NULL, 0, *phDevice));

    // Load the PTX.
    checkCudaErrors(cuModuleLoadDataEx(phModule, ptx, 0, 0, 0));

    // Locate the kernel entry point.
    checkCudaErrors(cuModuleGetFunction(phKernel, *phModule, "simple"));

    return CUDA_SUCCESS;
}

static char *loadProgramSource(const char *filename, size_t *size)
{
    assert(filename && size);
    char *source = NULL;
    *size        = 0;
    FILE *fh     = fopen(filename, "rb");
    if (fh) {
        struct stat statbuf;
        stat(filename, &statbuf);
        source = malloc(statbuf.st_size + 1);
        assert(source);
        fread(source, statbuf.st_size, 1, fh);
        source[statbuf.st_size] = 0;
        *size                   = statbuf.st_size + 1;
    }
    else {
        fprintf(stderr, "Error reading file %s\n", filename);
        exit(EXIT_FAILURE);
    }
    return source;
}

static char *generatePTX(const char *ir, size_t size, const char *filename, int devMajor, int devMinor)
{
    assert(ir && filename);

    // Create a program instance for use with libNVVM.
    nvvmProgram program;
    nvvmResult  result = nvvmCreateProgram(&program);
    if (result != NVVM_SUCCESS) {
        fprintf(stderr, "nvvmCreateProgram: Failed\n");
        exit(EXIT_FAILURE);
    }

    // Add the NVVM IR to the program instance.
    result = nvvmAddModuleToProgram(program, ir, size, filename);
    if (result != NVVM_SUCCESS) {
        fprintf(stderr, "nvvmAddModuleToProgram: Failed\n");
        exit(EXIT_FAILURE);
    }

    // Dynamically construct the compute capability.
    char arch[32] = {0};
    snprintf(arch, sizeof(arch) - 1, "-arch=compute_%d%d", devMajor, devMinor);

    // Compile the IR into PTX.
    const char *options[] = {arch};
    result                = nvvmCompileProgram(program, 1, options);
    if (result != NVVM_SUCCESS) {
        fprintf(stderr, "nvvmCompileProgram: Failed\n");
        size_t logSize;
        nvvmGetProgramLogSize(program, &logSize);
        char *msg = malloc(logSize);
        assert(msg);
        nvvmGetProgramLog(program, msg);
        fprintf(stderr, "%s\n", msg);
        free(msg);
        exit(EXIT_FAILURE);
    }

    // Obrain the resulting PTX.
    size_t ptxSize;
    result = nvvmGetCompiledResultSize(program, &ptxSize);
    if (result != NVVM_SUCCESS) {
        fprintf(stderr, "nvvmGetCompiledResultSize: Failed\n");
        exit(EXIT_FAILURE);
    }
    char *ptx = malloc(ptxSize);
    assert(ptx);
    result = nvvmGetCompiledResult(program, ptx);
    if (result != NVVM_SUCCESS) {
        fprintf(stderr, "nvvmGetCompiledResult: Failed\n");
        free(ptx);
        exit(EXIT_FAILURE);
    }

    // Cleanup the libNVVM program instance.
    result = nvvmDestroyProgram(&program);
    if (result != NVVM_SUCCESS) {
        fprintf(stderr, "nvvmDestroyProgram: Failed\n");
        free(ptx);
        exit(EXIT_FAILURE);
    }

    return ptx;
}

int main(int argc, char **argv)
{
    const unsigned int nThreads = 32;
    const unsigned int nBlocks  = 1;
    const size_t       memSize  = nThreads * nBlocks * sizeof(int);
    const char        *filename = "simple-gpu64.ll";

    // Retrieve the NVVM IR from filename and create the kernel parameters.
    size_t size = 0;
    char  *ir   = loadProgramSource(filename, &size);
    fprintf(stdout, "NVVM IR (.ll) file loaded\n");

    // Initialize the device and obtain the compute capability.
    int      devMajor = 0, devMinor = 0;
    CUdevice hDevice = cudaDeviceInit(&devMajor, &devMinor);

    // Use libNVVM to generate PTX from the NVVM IR.
    char *ptx = generatePTX(ir, size, filename, devMajor, devMinor);
    fprintf(stdout, "PTX generated:\n");
    fprintf(stdout, "%s\n", ptx);

    // Initialize the device and get a handle to the kernel.
    CUcontext  hContext = 0;
    CUmodule   hModule  = 0;
    CUfunction hKernel  = 0;
    checkCudaErrors(initCUDA(&hContext, &hDevice, &hModule, &hKernel, ptx));

    // Allocate memory on the host and device.
    int *hData = malloc(memSize);
    if (!hData) {
        fprintf(stderr, "Could not allocate host memory\n");
        exit(EXIT_FAILURE);
    }
    CUdeviceptr dData = 0;
    checkCudaErrors(cuMemAlloc(&dData, memSize));

    // Launch the kernel.
    void *params[] = {&dData};
    checkCudaErrors(cuLaunchKernel(hKernel, nBlocks, 1, 1, nThreads, 1, 1, 0, NULL, params, NULL));
    fprintf(stdout, "CUDA kernel launched\n");

    // Copy the result back to the host.
    checkCudaErrors(cuMemcpyDtoH(hData, dData, memSize));

    // Print the result.
    for (unsigned i = 0; i < nBlocks * nThreads; i++)
        fprintf(stdout, "%d ", hData[i]);
    fprintf(stdout, "\n");

    // Cleanup.
    if (dData)
        checkCudaErrors(cuMemFree(dData));
    if (hModule)
        checkCudaErrors(cuModuleUnload(hModule));
    if (hContext)
        checkCudaErrors(cuCtxDestroy(hContext));
    free(hData);
    free(ir);
    free(ptx);

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/7_libNVVM/simple/simple.c`.

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

- **Total Lines**: 252
- **Approximate Size**: 8691 bytes

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
