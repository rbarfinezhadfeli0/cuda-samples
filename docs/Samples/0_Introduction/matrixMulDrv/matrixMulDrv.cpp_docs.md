# Documentation for Samples/0_Introduction/matrixMulDrv/matrixMulDrv.cpp

## File Metadata

- **Path**: `Samples/0_Introduction/matrixMulDrv/matrixMulDrv.cpp`
- **Type**: .cpp
- **Location**: Samples/0_Introduction/matrixMulDrv
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

/* Matrix multiplication: C = A * B.
 * Host code.
 *
 * This sample implements matrix multiplication using the CUDA driver API.
 * It has been written for clarity of exposition to illustrate various CUDA
 * programming principles, not with the goal of providing the most
 * performant generic kernel for matrix multiplication.
 *
 * CUBLAS provides high-performance matrix multiplication.
 * See also:
 * V. Volkov and J. Demmel, "Benchmarking GPUs to tune dense linear algebra,"
 * in Proc. 2008 ACM/IEEE Conf. on Supercomputing (SC '08),
 * Piscataway, NJ: IEEE Press, 2008, pp. Art. 31:1-11.
 *
 * Volkov, V. 2010. Better performance at lower occupancy,
 * GPU Technology Conference 2~010 (GTC 2010).
 *
 */

// includes, system
#include <builtin_types.h>
#include <cstring>
#include <iostream>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project, CUDA
#include <cstring>
#include <cuda.h>
#include <helper_cuda_drvapi.h>
#include <helper_image.h>
#include <helper_string.h>
#include <helper_timer.h>
#include <iostream>
#include <string>

#include "matrixMul.h"


////////////////////////////////////////////////////////////////////////////////
// declaration, forward
void runTest(int argc, char **argv);
void randomInit(float *, int);

extern "C" void computeGold(float *, const float *, const float *, unsigned int, unsigned int, unsigned int);

static int initCUDA(int argc, char **argv, CUfunction *pMatrixMul, int *blk_size);

#ifndef FATBIN_FILE
#define FATBIN_FILE "matrixMul_kernel64.fatbin"
#endif

////////////////////////////////////////////////////////////////////////////////
// Globals
////////////////////////////////////////////////////////////////////////////////
CUdevice  cuDevice;
CUcontext cuContext;
CUmodule  cuModule;
size_t    totalGlobalMem;

const char *sSDKsample = "matrixMulDrv (Driver API)";

void constantInit(float *data, int size, float val)
{
    for (int i = 0; i < size; ++i) {
        data[i] = val;
    }
}

////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    printf("[ %s ]\n", sSDKsample);

    runTest(argc, argv);
}

////////////////////////////////////////////////////////////////////////////////
//! Run a simple test for CUDA
////////////////////////////////////////////////////////////////////////////////
void runTest(int argc, char **argv)
{
    // initialize CUDA
    CUfunction matrixMul  = NULL;
    int        block_size = 0;

    initCUDA(argc, argv, &matrixMul, &block_size);

    // set seed for rand()
    srand(2006);

    // allocate host memory for matrices A and B
    unsigned int size_A     = WA * HA;
    unsigned int mem_size_A = sizeof(float) * size_A;
    float       *h_A        = reinterpret_cast<float *>(malloc(mem_size_A));
    unsigned int size_B     = WB * HB;
    unsigned int mem_size_B = sizeof(float) * size_B;
    float       *h_B        = reinterpret_cast<float *>(malloc(mem_size_B));

    // initialize host memory
    const float valB = 0.01f;
    constantInit(h_A, size_A, 1.0f);
    constantInit(h_B, size_B, valB);

    // allocate device memory
    CUdeviceptr d_A;
    checkCudaErrors(cuMemAlloc(&d_A, mem_size_A));
    CUdeviceptr d_B;
    checkCudaErrors(cuMemAlloc(&d_B, mem_size_B));

    // copy host memory to device
    checkCudaErrors(cuMemcpyHtoD(d_A, h_A, mem_size_A));
    checkCudaErrors(cuMemcpyHtoD(d_B, h_B, mem_size_B));

    // allocate device memory for result
    size_t size_C     = WC * HC;
    size_t mem_size_C = sizeof(float) * size_C;

    CUdeviceptr d_C;
    checkCudaErrors(cuMemAlloc(&d_C, mem_size_C));

    // allocate mem for the result on host side
    float *h_C = reinterpret_cast<float *>(malloc(mem_size_C));

    // create and start timer
    StopWatchInterface *timer = NULL;
    sdkCreateTimer(&timer);

    // start the timer
    sdkStartTimer(&timer);

    // There are two ways to launch CUDA kernels via the Driver API.
    // In this CUDA Sample, we illustrate both ways to pass parameters
    // and specify parameters.  By default we use the simpler method.
    dim3 block(block_size, block_size, 1);
    dim3 grid(WC / block_size, HC / block_size, 1);

    if (1) {
        // This is the new CUDA 4.0 API for Kernel Parameter passing and Kernel
        // Launching (simplier method)
        size_t Matrix_Width_A = (size_t)WA;
        size_t Matrix_Width_B = (size_t)WB;
        void  *args[5]        = {&d_C, &d_A, &d_B, &Matrix_Width_A, &Matrix_Width_B};
        // new CUDA 4.0 Driver API Kernel launch call
        checkCudaErrors(cuLaunchKernel(matrixMul,
                                       grid.x,
                                       grid.y,
                                       grid.z,
                                       block.x,
                                       block.y,
                                       block.z,
                                       2 * block_size * block_size * sizeof(float),
                                       NULL,
                                       args,
                                       NULL));
    }
    else {
        // This is the new CUDA 4.0 API for Kernel Parameter passing and Kernel
        // Launching (advanced method)
        int  offset = 0;
        char argBuffer[256];

        // pass in launch parameters (not actually de-referencing CUdeviceptr).
        // CUdeviceptr is storing the value of the parameters
        *(reinterpret_cast<CUdeviceptr *>(&argBuffer[offset])) = d_C;
        offset += sizeof(d_C);
        *(reinterpret_cast<CUdeviceptr *>(&argBuffer[offset])) = d_A;
        offset += sizeof(d_A);
        *(reinterpret_cast<CUdeviceptr *>(&argBuffer[offset])) = d_B;
        offset += sizeof(d_B);

        size_t Matrix_Width_A = (size_t)WA;
        size_t Matrix_Width_B = (size_t)WB;

        *(reinterpret_cast<CUdeviceptr *>(&argBuffer[offset])) = Matrix_Width_A;
        offset += sizeof(Matrix_Width_A);
        *(reinterpret_cast<CUdeviceptr *>(&argBuffer[offset])) = Matrix_Width_B;
        offset += sizeof(Matrix_Width_B);

        void *kernel_launch_config[5] = {
            CU_LAUNCH_PARAM_BUFFER_POINTER, argBuffer, CU_LAUNCH_PARAM_BUFFER_SIZE, &offset, CU_LAUNCH_PARAM_END};

        // new CUDA 4.0 Driver API Kernel launch call
        checkCudaErrors(cuLaunchKernel(matrixMul,
                                       grid.x,
                                       grid.y,
                                       grid.z,
                                       block.x,
                                       block.y,
                                       block.z,
                                       2 * block_size * block_size * sizeof(float),
                                       NULL,
                                       NULL,
                                       reinterpret_cast<void **>(&kernel_launch_config)));
    }

    // copy result from device to host
    checkCudaErrors(cuMemcpyDtoH(reinterpret_cast<void *>(h_C), d_C, mem_size_C));

    // stop and destroy timer
    sdkStopTimer(&timer);
    printf("Processing time: %f (ms)\n", sdkGetTimerValue(&timer));
    sdkDeleteTimer(&timer);

    printf("Checking computed result for correctness: ");
    bool correct = true;

    for (int i = 0; i < static_cast<int>(WC * HC); i++) {
        if (fabs(h_C[i] - (WA * valB)) > 1e-5) {
            printf("Error! Matrix[%05d]=%.8f, ref=%.8f error term is > 1e-5\n", i, h_C[i], WA * valB);
            correct = false;
        }
    }

    printf("%s\n", correct ? "Result = PASS" : "Result = FAIL");

    printf("\nNOTE: The CUDA Samples are not meant for performance measurements. "
           "Results may vary when GPU Boost is enabled.\n");

    // clean up memory
    free(h_A);
    free(h_B);
    free(h_C);
    checkCudaErrors(cuMemFree(d_A));
    checkCudaErrors(cuMemFree(d_B));
    checkCudaErrors(cuMemFree(d_C));
    checkCudaErrors(cuCtxDestroy(cuContext));
}

// Allocates a matrix with random float entries.
void randomInit(float *data, int size)
{
    for (int i = 0; i < size; ++i) {
        data[i] = rand() / static_cast<float>(RAND_MAX);
    }
}

static int initCUDA(int argc, char **argv, CUfunction *pMatrixMul, int *blk_size)
{
    CUfunction        cuFunction = 0;
    int               major = 0, minor = 0;
    char              deviceName[100];
    CUctxCreateParams ctxCreateParams = {};

    cuDevice = findCudaDeviceDRV(argc, (const char **)argv);

    // get compute capabilities and the devicename
    checkCudaErrors(cuDeviceGetAttribute(&major, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR, cuDevice));
    checkCudaErrors(cuDeviceGetAttribute(&minor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR, cuDevice));
    checkCudaErrors(cuDeviceGetName(deviceName, sizeof(deviceName), cuDevice));
    printf("> GPU Device has SM %d.%d compute capability\n", major, minor);

    checkCudaErrors(cuDeviceTotalMem(&totalGlobalMem, cuDevice));
    printf("  Total amount of global memory:     %llu bytes\n", (long long unsigned int)totalGlobalMem);

    checkCudaErrors(cuCtxCreate(&cuContext, &ctxCreateParams, 0, cuDevice));

    // first search for the module path before we load the results
    std::string        module_path;
    std::ostringstream fatbin;

    if (!findFatbinPath(FATBIN_FILE, module_path, argv, fatbin)) {
        exit(EXIT_FAILURE);
    }
    else {
        printf("> initCUDA loading module: <%s>\n", module_path.c_str());
    }

    if (!fatbin.str().size()) {
        printf("fatbin file empty. exiting..\n");
        exit(EXIT_FAILURE);
    }

    // Create module from binary file (FATBIN)
    checkCudaErrors(cuModuleLoadData(&cuModule, fatbin.str().c_str()));

    // select the suitable kernel function
    const char *kernels[] = {"matrixMul_bs32_64bit", "matrixMul_bs16_64bit", "matrixMul_bs8_64bit"};

    int idx        = 0;
    int block_size = 32;
    while (idx < 3) {
        int threadsPerBlock = 0;
        int blocksPerGrid   = 0;

        checkCudaErrors(cuModuleGetFunction(&cuFunction, cuModule, kernels[idx]));
        checkCudaErrors(cuOccupancyMaxPotentialBlockSize(
            &blocksPerGrid, &threadsPerBlock, cuFunction, 0, 2 * block_size * block_size * sizeof(float), 0));
        if (block_size * block_size <= threadsPerBlock) {
            printf("> %d block size selected\n", block_size);
            break;
        }
        else {
            block_size /= 2;
        }
        idx++;
    }

    *pMatrixMul = cuFunction;
    *blk_size   = block_size;

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/matrixMulDrv/matrixMulDrv.cpp`.

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

- **Total Lines**: 335
- **Approximate Size**: 12211 bytes

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
