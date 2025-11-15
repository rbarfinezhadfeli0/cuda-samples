# Documentation for Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu`
- **Type**: .cu
- **Location**: Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU
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

////////////////////////////////////////////////////////////////////////////////
//
//  simpleCUFFT_2d_MGPU.cu
//
//  This sample code demonstrate the use of CUFFT library for 2D data on multiple GPU.
//  Example showing the use of CUFFT for solving 2D-POISSON equation using FFT on multiple GPU.
//  For reference we have used the equation given in http://www.bu.edu/pasi/files/2011/07/
//  Lecture83.pdf
//
////////////////////////////////////////////////////////////////////////////////


// System includes
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// CUDA runtime
#include <cuda_runtime.h>

// CUFFT Header file
#include <cufftXt.h>

// helper functions and utilities to work with CUDA
#include <helper_cuda.h>
#include <helper_functions.h>

// Complex data type
typedef float2 Complex;

// Data configuration
const int GPU_COUNT = 2;
const int BSZ_Y     = 4;
const int BSZ_X     = 4;

// Forward Declaration
void solvePoissonEquation(cudaLibXtDesc *, cudaLibXtDesc *, float **, int, int);

__global__ void solvePoisson(cufftComplex *, cufftComplex *, float *, int, int, int n_gpu);

///////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    printf("\nPoisson equation using CUFFT library on Multiple GPUs is "
           "starting...\n\n");

    int GPU_N;
    checkCudaErrors(cudaGetDeviceCount(&GPU_N));

    if (GPU_N < GPU_COUNT) {
        printf("No. of GPU on node %d\n", GPU_N);
        printf("Two GPUs are required to run simpleCUFFT_2d_MGPU sample code\n");
        exit(EXIT_WAIVED);
    }

    int *major_minor         = (int *)malloc(sizeof(int) * GPU_N * 2);
    int  found2IdenticalGPUs = 0;
    int  nGPUs               = 2;
    int *whichGPUs;
    whichGPUs = (int *)malloc(sizeof(int) * nGPUs);

    for (int i = 0; i < GPU_N; i++) {
        cudaDeviceProp deviceProp;
        checkCudaErrors(cudaGetDeviceProperties(&deviceProp, i));
        major_minor[i * 2]     = deviceProp.major;
        major_minor[i * 2 + 1] = deviceProp.minor;
        printf("GPU Device %d: \"%s\" with compute capability %d.%d\n",
               i,
               deviceProp.name,
               deviceProp.major,
               deviceProp.minor);
    }

    for (int i = 0; i < GPU_N; i++) {
        for (int j = i + 1; j < GPU_N; j++) {
            if ((major_minor[i * 2] == major_minor[j * 2]) && (major_minor[i * 2 + 1] == major_minor[j * 2 + 1])) {
                whichGPUs[0]        = i;
                whichGPUs[1]        = j;
                found2IdenticalGPUs = 1;
                break;
            }
        }
        if (found2IdenticalGPUs) {
            break;
        }
    }

    free(major_minor);
    if (!found2IdenticalGPUs) {
        printf("No Two GPUs with same architecture found\nWaiving simpleCUFFT_2d_MGPU "
               "sample\n");
        exit(EXIT_WAIVED);
    }

    int    N    = 64;
    float  xMAX = 1.0f, xMIN = 0.0f, yMIN = 0.0f, h = (xMAX - xMIN) / ((float)N), s = 0.1f, s2 = s * s;
    float *x, *y, *f, *u_a, r2;

    x   = (float *)malloc(sizeof(float) * N * N);
    y   = (float *)malloc(sizeof(float) * N * N);
    f   = (float *)malloc(sizeof(float) * N * N);
    u_a = (float *)malloc(sizeof(float) * N * N);

    for (int j = 0; j < N; j++)
        for (int i = 0; i < N; i++) {
            x[N * j + i] = xMIN + i * h;
            y[N * j + i] = yMIN + j * h;
            r2 = (x[N * j + i] - 0.5f) * (x[N * j + i] - 0.5f) + (y[N * j + i] - 0.5f) * (y[N * j + i] - 0.5f);
            f[N * j + i]   = (r2 - 2 * s2) / (s2 * s2) * exp(-r2 / (2 * s2));
            u_a[N * j + i] = exp(-r2 / (2 * s2)); // analytical solution
        }

    float *k, *d_k[GPU_COUNT];
    k = (float *)malloc(sizeof(float) * N);
    for (int i = 0; i <= N / 2; i++) {
        k[i] = i * 2 * (float)M_PI;
    }
    for (int i = N / 2 + 1; i < N; i++) {
        k[i] = (i - N) * 2 * (float)M_PI;
    }

    // Create a complex variable on host
    Complex *h_f = (Complex *)malloc(sizeof(Complex) * N * N);

    // Initialize the memory for the signal
    for (int i = 0; i < (N * N); i++) {
        h_f[i].x = f[i];
        h_f[i].y = 0.0f;
    }

    // cufftCreate() - Create an empty plan
    cufftResult result;
    cufftHandle planComplex;
    result = cufftCreate(&planComplex);
    if (result != CUFFT_SUCCESS) {
        printf("cufftCreate failed\n");
        exit(EXIT_FAILURE);
    }

    // cufftXtSetGPUs() - Define which GPUs to use
    result = cufftXtSetGPUs(planComplex, nGPUs, whichGPUs);

    if (result == CUFFT_INVALID_DEVICE) {
        printf("This sample requires two GPUs on the same board.\n");
        printf("No such board was found. Waiving sample.\n");
        exit(EXIT_WAIVED);
    }
    else if (result != CUFFT_SUCCESS) {
        printf("cufftXtSetGPUs failed\n");
        exit(EXIT_FAILURE);
    }

    // Print the device information to run the code
    printf("\nRunning on GPUs\n");
    for (int i = 0; i < 2; i++) {
        cudaDeviceProp deviceProp;
        checkCudaErrors(cudaGetDeviceProperties(&deviceProp, whichGPUs[i]));
        printf("GPU Device %d: \"%s\" with compute capability %d.%d\n",
               whichGPUs[i],
               deviceProp.name,
               deviceProp.major,
               deviceProp.minor);
    }

    size_t *worksize;
    worksize = (size_t *)malloc(sizeof(size_t) * nGPUs);

    // cufftMakePlan2d() - Create the plan
    result = cufftMakePlan2d(planComplex, N, N, CUFFT_C2C, worksize);
    if (result != CUFFT_SUCCESS) {
        printf("*MakePlan* failed\n");
        exit(EXIT_FAILURE);
    }

    for (int i = 0; i < nGPUs; i++) {
        cudaSetDevice(whichGPUs[i]);
        cudaMalloc((void **)&d_k[i], sizeof(float) * N);
        cudaMemcpy(d_k[i], k, sizeof(float) * N, cudaMemcpyHostToDevice);
    }

    // Create a variable on device
    // d_f - variable on device to store the input data
    // d_d_f - variable that store the natural order of d_f data
    // d_out - device output
    cudaLibXtDesc *d_f, *d_d_f, *d_out;

    // cufftXtMalloc() - Malloc data on multiple GPUs

    result = cufftXtMalloc(planComplex, (cudaLibXtDesc **)&d_f, CUFFT_XT_FORMAT_INPLACE);
    if (result != CUFFT_SUCCESS) {
        printf("*XtMalloc failed\n");
        exit(EXIT_FAILURE);
    }

    result = cufftXtMalloc(planComplex, (cudaLibXtDesc **)&d_d_f, CUFFT_XT_FORMAT_INPLACE);
    if (result != CUFFT_SUCCESS) {
        printf("*XtMalloc failed\n");
        exit(EXIT_FAILURE);
    }

    result = cufftXtMalloc(planComplex, (cudaLibXtDesc **)&d_out, CUFFT_XT_FORMAT_INPLACE);
    if (result != CUFFT_SUCCESS) {
        printf("*XtMalloc failed\n");
        exit(EXIT_FAILURE);
    }

    // cufftXtMemcpy() - Copy the data from host to device
    result = cufftXtMemcpy(planComplex, d_f, h_f, CUFFT_COPY_HOST_TO_DEVICE);
    if (result != CUFFT_SUCCESS) {
        printf("*XtMemcpy failed\n");
        exit(EXIT_FAILURE);
    }

    // cufftXtExecDescriptorC2C() - Execute FFT on data on multiple GPUs
    printf("Forward 2d FFT on multiple GPUs\n");
    result = cufftXtExecDescriptorC2C(planComplex, d_f, d_f, CUFFT_FORWARD);
    if (result != CUFFT_SUCCESS) {
        printf("*XtExecC2C  failed\n");
        exit(EXIT_FAILURE);
    }

    // cufftXtMemcpy() - Copy the data to natural order on GPUs
    result = cufftXtMemcpy(planComplex, d_d_f, d_f, CUFFT_COPY_DEVICE_TO_DEVICE);
    if (result != CUFFT_SUCCESS) {
        printf("*XtMemcpy failed\n");
        exit(EXIT_FAILURE);
    }

    printf("Solve Poisson Equation\n");
    solvePoissonEquation(d_d_f, d_out, d_k, N, nGPUs);

    printf("Inverse 2d FFT on multiple GPUs\n");
    // cufftXtExecDescriptorC2C() - Execute inverse  FFT on data on multiple GPUs
    result = cufftXtExecDescriptorC2C(planComplex, d_out, d_out, CUFFT_INVERSE);
    if (result != CUFFT_SUCCESS) {
        printf("*XtExecC2C  failed\n");
        exit(EXIT_FAILURE);
    }

    // Create a variable on host to copy the data from device
    // h_d_out - variable store the output of device
    Complex *h_d_out = (Complex *)malloc(sizeof(Complex) * N * N);

    // cufftXtMemcpy() - Copy data from multiple GPUs to host
    result = cufftXtMemcpy(planComplex, h_d_out, d_out, CUFFT_COPY_DEVICE_TO_HOST);
    if (result != CUFFT_SUCCESS) {
        printf("*XtMemcpy failed\n");
        exit(EXIT_FAILURE);
    }

    float *out      = (float *)malloc(sizeof(float) * N * N);
    float  constant = h_d_out[0].x / N * N;
    for (int i = 0; i < N * N; i++) {
        // subtract u[0] to force the arbitrary constant to be 0
        out[i] = (h_d_out[i].x / (N * N)) - constant;
    }

    // cleanup memory

    free(h_f);
    free(k);
    free(out);
    free(h_d_out);
    free(x);
    free(whichGPUs);
    free(y);
    free(f);
    free(u_a);
    free(worksize);

    // cudaXtFree() - Free GPU memory
    for (int i = 0; i < GPU_COUNT; i++) {
        cudaFree(d_k[i]);
    }
    result = cufftXtFree(d_out);
    if (result != CUFFT_SUCCESS) {
        printf("*XtFree failed\n");
        exit(EXIT_FAILURE);
    }
    result = cufftXtFree(d_f);
    if (result != CUFFT_SUCCESS) {
        printf("*XtFree failed\n");
        exit(EXIT_FAILURE);
    }
    result = cufftXtFree(d_d_f);
    if (result != CUFFT_SUCCESS) {
        printf("*XtFree failed\n");
        exit(EXIT_FAILURE);
    }

    // cufftDestroy() - Destroy FFT plan
    result = cufftDestroy(planComplex);
    if (result != CUFFT_SUCCESS) {
        printf("cufftDestroy failed: code %d\n", (int)result);
        exit(EXIT_FAILURE);
    }

    exit(EXIT_SUCCESS);
}

////////////////////////////////////////////////////////////////////////////////////
// Launch kernel on  multiple GPU
///////////////////////////////////////////////////////////////////////////////////
void solvePoissonEquation(cudaLibXtDesc *d_ft, cudaLibXtDesc *d_ft_k, float **k, int N, int nGPUs)
{
    int  device;
    dim3 dimGrid(int(N / BSZ_X), int((N / 2) / BSZ_Y));
    dim3 dimBlock(BSZ_X, BSZ_Y);

    for (int i = 0; i < nGPUs; i++) {
        device = d_ft_k->descriptor->GPUs[i];
        cudaSetDevice(device);
        solvePoisson<<<dimGrid, dimBlock>>>(
            (cufftComplex *)d_ft->descriptor->data[i], (cufftComplex *)d_ft_k->descriptor->data[i], k[i], N, i, nGPUs);
    }

    // Wait for device to finish all operation
    for (int i = 0; i < nGPUs; i++) {
        device = d_ft_k->descriptor->GPUs[i];
        cudaSetDevice(device);
        cudaDeviceSynchronize();

        // Check if kernel execution generated and error
        getLastCudaError("Kernel execution failed [ solvePoisson ]");
    }
}

////////////////////////////////////////////////////////////////////////////////
// Kernel for Solving Poisson equation on GPU
////////////////////////////////////////////////////////////////////////////////
__global__ void solvePoisson(cufftComplex *ft, cufftComplex *ft_k, float *k, int N, int gpu_id, int n_gpu)
{
    int i     = threadIdx.x + blockIdx.x * blockDim.x;
    int j     = threadIdx.y + blockIdx.y * blockDim.y;
    int index = j * N + i;
    if (i < N && j < N / n_gpu) {
        float k2 = k[i] * k[i] + k[j + gpu_id * N / n_gpu] * k[j + gpu_id * N / n_gpu];
        if (i == 0 && j == 0 && gpu_id == 0) {
            k2 = 1.0f;
        }

        ft_k[index].x = -ft[index].x * 1 / k2;
        ft_k[index].y = -ft[index].y * 1 / k2;
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/simpleCUFFT_2d_MGPU/simpleCUFFT_2d_MGPU.cu`.

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

- **Total Lines**: 376
- **Approximate Size**: 13025 bytes

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
