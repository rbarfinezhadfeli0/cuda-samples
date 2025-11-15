# Documentation for Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu`
- **Type**: .cu
- **Location**: Samples/4_CUDA_Libraries/simpleCUFFT_MGPU
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

/* Example showing the use of CUFFT for fast 1D-convolution using FFT. */

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

static __device__ __host__ inline Complex ComplexAdd(Complex, Complex);
static __device__ __host__ inline Complex ComplexScale(Complex, float);
static __device__ __host__ inline Complex ComplexMul(Complex, Complex);
static __global__ void                    ComplexPointwiseMulAndScale(cufftComplex *, cufftComplex *, int, float);

// Kernel for GPU
void multiplyCoefficient(cudaLibXtDesc *, cudaLibXtDesc *, int, float, int);

// Filtering functions
void Convolve(const Complex *, int, const Complex *, int, Complex *);

// Padding functions
int PadData(const Complex *, Complex **, int, const Complex *, Complex **, int);

////////////////////////////////////////////////////////////////////////////////
// Data configuration
// The filter size is assumed to be a number smaller than the signal size
///////////////////////////////////////////////////////////////////////////////
const int SIGNAL_SIZE        = 1018;
const int FILTER_KERNEL_SIZE = 11;
const int GPU_COUNT          = 2;

////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    printf("\n[simpleCUFFT_MGPU] is starting...\n\n");

    int GPU_N;
    checkCudaErrors(cudaGetDeviceCount(&GPU_N));

    if (GPU_N < GPU_COUNT) {
        printf("No. of GPU on node %d\n", GPU_N);
        printf("Two GPUs are required to run simpleCUFFT_MGPU sample code\n");
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

    // Allocate host memory for the signal
    Complex *h_signal = (Complex *)malloc(sizeof(Complex) * SIGNAL_SIZE);

    // Initialize the memory for the signal
    for (int i = 0; i < SIGNAL_SIZE; ++i) {
        h_signal[i].x = rand() / (float)RAND_MAX;
        h_signal[i].y = 0;
    }

    // Allocate host memory for the filter
    Complex *h_filter_kernel = (Complex *)malloc(sizeof(Complex) * FILTER_KERNEL_SIZE);

    // Initialize the memory for the filter
    for (int i = 0; i < FILTER_KERNEL_SIZE; ++i) {
        h_filter_kernel[i].x = rand() / (float)RAND_MAX;
        h_filter_kernel[i].y = 0;
    }

    // Pad signal and filter kernel
    Complex *h_padded_signal;
    Complex *h_padded_filter_kernel;
    int      new_size =
        PadData(h_signal, &h_padded_signal, SIGNAL_SIZE, h_filter_kernel, &h_padded_filter_kernel, FILTER_KERNEL_SIZE);

    // cufftCreate() - Create an empty plan
    cufftResult result;
    cufftHandle plan_input;
    checkCudaErrors(cufftCreate(&plan_input));

    // cufftXtSetGPUs() - Define which GPUs to use
    result = cufftXtSetGPUs(plan_input, nGPUs, whichGPUs);

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
    for (int i = 0; i < nGPUs; i++) {
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

    // cufftMakePlan1d() - Create the plan
    checkCudaErrors(cufftMakePlan1d(plan_input, new_size, CUFFT_C2C, 1, worksize));

    // cufftXtMalloc() - Malloc data on multiple GPUs
    cudaLibXtDesc *d_signal;
    checkCudaErrors(cufftXtMalloc(plan_input, (cudaLibXtDesc **)&d_signal, CUFFT_XT_FORMAT_INPLACE));
    cudaLibXtDesc *d_out_signal;
    checkCudaErrors(cufftXtMalloc(plan_input, (cudaLibXtDesc **)&d_out_signal, CUFFT_XT_FORMAT_INPLACE));
    cudaLibXtDesc *d_filter_kernel;
    checkCudaErrors(cufftXtMalloc(plan_input, (cudaLibXtDesc **)&d_filter_kernel, CUFFT_XT_FORMAT_INPLACE));
    cudaLibXtDesc *d_out_filter_kernel;
    checkCudaErrors(cufftXtMalloc(plan_input, (cudaLibXtDesc **)&d_out_filter_kernel, CUFFT_XT_FORMAT_INPLACE));

    // cufftXtMemcpy() - Copy data from host to multiple GPUs
    checkCudaErrors(cufftXtMemcpy(plan_input, d_signal, h_padded_signal, CUFFT_COPY_HOST_TO_DEVICE));
    checkCudaErrors(cufftXtMemcpy(plan_input, d_filter_kernel, h_padded_filter_kernel, CUFFT_COPY_HOST_TO_DEVICE));

    // cufftXtExecDescriptorC2C() - Execute FFT on data on multiple GPUs
    checkCudaErrors(cufftXtExecDescriptorC2C(plan_input, d_signal, d_signal, CUFFT_FORWARD));
    checkCudaErrors(cufftXtExecDescriptorC2C(plan_input, d_filter_kernel, d_filter_kernel, CUFFT_FORWARD));

    // cufftXtMemcpy() - Copy the data to natural order on GPUs
    checkCudaErrors(cufftXtMemcpy(plan_input, d_out_signal, d_signal, CUFFT_COPY_DEVICE_TO_DEVICE));
    checkCudaErrors(cufftXtMemcpy(plan_input, d_out_filter_kernel, d_filter_kernel, CUFFT_COPY_DEVICE_TO_DEVICE));

    printf("\n\nValue of Library Descriptor\n");
    printf("Number of GPUs %d\n", d_out_signal->descriptor->nGPUs);
    printf("Device id  %d %d\n", d_out_signal->descriptor->GPUs[0], d_out_signal->descriptor->GPUs[1]);
    printf("Data size on GPU %ld %ld\n",
           (long)(d_out_signal->descriptor->size[0] / sizeof(cufftComplex)),
           (long)(d_out_signal->descriptor->size[1] / sizeof(cufftComplex)));

    // Multiply the coefficients together and normalize the result
    printf("Launching ComplexPointwiseMulAndScale<<< >>>\n");
    multiplyCoefficient(d_out_signal, d_out_filter_kernel, new_size, 1.0f / new_size, nGPUs);

    // cufftXtExecDescriptorC2C() - Execute inverse  FFT on data on multiple GPUs
    printf("Transforming signal back cufftExecC2C\n");
    checkCudaErrors(cufftXtExecDescriptorC2C(plan_input, d_out_signal, d_out_signal, CUFFT_INVERSE));

    // Create host pointer pointing to padded signal
    Complex *h_convolved_signal = h_padded_signal;

    // Allocate host memory for the convolution result
    Complex *h_convolved_signal_ref = (Complex *)malloc(sizeof(Complex) * SIGNAL_SIZE);

    // cufftXtMemcpy() - Copy data from multiple GPUs to host
    checkCudaErrors(cufftXtMemcpy(plan_input, h_convolved_signal, d_out_signal, CUFFT_COPY_DEVICE_TO_HOST));

    // Convolve on the host
    Convolve(h_signal, SIGNAL_SIZE, h_filter_kernel, FILTER_KERNEL_SIZE, h_convolved_signal_ref);

    // Compare CPU and GPU result
    bool bTestResult =
        sdkCompareL2fe((float *)h_convolved_signal_ref, (float *)h_convolved_signal, 2 * SIGNAL_SIZE, 1e-5f);
    printf("\nvalue of TestResult %d\n", bTestResult);

    // Cleanup memory
    free(whichGPUs);
    free(worksize);
    free(h_signal);
    free(h_filter_kernel);
    free(h_padded_signal);
    free(h_padded_filter_kernel);
    free(h_convolved_signal_ref);

    // cudaXtFree() - Free GPU memory
    checkCudaErrors(cufftXtFree(d_signal));
    checkCudaErrors(cufftXtFree(d_filter_kernel));
    checkCudaErrors(cufftXtFree(d_out_signal));
    checkCudaErrors(cufftXtFree(d_out_filter_kernel));

    // cufftDestroy() - Destroy FFT plan
    checkCudaErrors(cufftDestroy(plan_input));

    exit(bTestResult ? EXIT_SUCCESS : EXIT_FAILURE);
}

///////////////////////////////////////////////////////////////////////////////////
// Function for padding original data
//////////////////////////////////////////////////////////////////////////////////
int PadData(const Complex *signal,
            Complex      **padded_signal,
            int            signal_size,
            const Complex *filter_kernel,
            Complex      **padded_filter_kernel,
            int            filter_kernel_size)
{
    int minRadius = filter_kernel_size / 2;
    int maxRadius = filter_kernel_size - minRadius;
    int new_size  = signal_size + maxRadius;

    // Pad signal
    Complex *new_data = (Complex *)malloc(sizeof(Complex) * new_size);
    memcpy(new_data + 0, signal, signal_size * sizeof(Complex));
    memset(new_data + signal_size, 0, (new_size - signal_size) * sizeof(Complex));
    *padded_signal = new_data;

    // Pad filter
    new_data = (Complex *)malloc(sizeof(Complex) * new_size);
    memcpy(new_data + 0, filter_kernel + minRadius, maxRadius * sizeof(Complex));
    memset(new_data + maxRadius, 0, (new_size - filter_kernel_size) * sizeof(Complex));
    memcpy(new_data + new_size - minRadius, filter_kernel, minRadius * sizeof(Complex));
    *padded_filter_kernel = new_data;

    return new_size;
}

////////////////////////////////////////////////////////////////////////////////
// Filtering operations - Computing Convolution on the host
////////////////////////////////////////////////////////////////////////////////
void Convolve(const Complex *signal,
              int            signal_size,
              const Complex *filter_kernel,
              int            filter_kernel_size,
              Complex       *filtered_signal)
{
    int minRadius = filter_kernel_size / 2;
    int maxRadius = filter_kernel_size - minRadius;

    // Loop over output element indices
    for (int i = 0; i < signal_size; ++i) {
        filtered_signal[i].x = filtered_signal[i].y = 0;

        // Loop over convolution indices
        for (int j = -maxRadius + 1; j <= minRadius; ++j) {
            int k = i + j;

            if (k >= 0 && k < signal_size) {
                filtered_signal[i] =
                    ComplexAdd(filtered_signal[i], ComplexMul(signal[k], filter_kernel[minRadius - j]));
            }
        }
    }
}

////////////////////////////////////////////////////////////////////////////////
//  Launch Kernel on multiple GPU
////////////////////////////////////////////////////////////////////////////////
void multiplyCoefficient(cudaLibXtDesc *d_signal, cudaLibXtDesc *d_filter_kernel, int new_size, float val, int nGPUs)
{
    int device;
    // Launch the ComplexPointwiseMulAndScale<<< >>> kernel on multiple GPU
    for (int i = 0; i < nGPUs; i++) {
        device = d_signal->descriptor->GPUs[i];

        // Set device
        checkCudaErrors(cudaSetDevice(device));

        // Perform GPU computations
        ComplexPointwiseMulAndScale<<<32, 256>>>((cufftComplex *)d_signal->descriptor->data[i],
                                                 (cufftComplex *)d_filter_kernel->descriptor->data[i],
                                                 int(d_signal->descriptor->size[i] / sizeof(cufftComplex)),
                                                 val);
    }

    // Wait for device to finish all operation
    for (int i = 0; i < nGPUs; i++) {
        device = d_signal->descriptor->GPUs[i];
        checkCudaErrors(cudaSetDevice(device));
        cudaDeviceSynchronize();
        // Check if kernel execution generated and error
        getLastCudaError("Kernel execution failed [ ComplexPointwiseMulAndScale ]");
    }
}

////////////////////////////////////////////////////////////////////////////////
// Complex operations
////////////////////////////////////////////////////////////////////////////////

// Complex addition
static __device__ __host__ inline Complex ComplexAdd(Complex a, Complex b)
{
    Complex c;
    c.x = a.x + b.x;
    c.y = a.y + b.y;
    return c;
}

// Complex scale
static __device__ __host__ inline Complex ComplexScale(Complex a, float s)
{
    Complex c;
    c.x = s * a.x;
    c.y = s * a.y;
    return c;
}

// Complex multiplication
static __device__ __host__ inline Complex ComplexMul(Complex a, Complex b)
{
    Complex c;
    c.x = a.x * b.x - a.y * b.y;
    c.y = a.x * b.y + a.y * b.x;
    return c;
}
// Complex pointwise multiplication
static __global__ void ComplexPointwiseMulAndScale(cufftComplex *a, cufftComplex *b, int size, float scale)
{
    const int numThreads = blockDim.x * gridDim.x;
    const int threadID   = blockIdx.x * blockDim.x + threadIdx.x;
    for (int i = threadID; i < size; i += numThreads) {
        a[i] = ComplexScale(ComplexMul(a[i], b[i]), scale);
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu`.

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

- **Total Lines**: 388
- **Approximate Size**: 15329 bytes

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
