# Documentation for Samples/5_Domain_Specific/stereoDisparity/stereoDisparity.cu

## File Metadata

- **Path**: `Samples/5_Domain_Specific/stereoDisparity/stereoDisparity.cu`
- **Type**: .cu
- **Location**: Samples/5_Domain_Specific/stereoDisparity
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

/* A CUDA program that demonstrates how to compute a stereo disparity map using
 * SIMD SAD (Sum of Absolute Difference) intrinsics
 */

// includes, system
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, kernels
#include <cuda_runtime.h>

#include "stereoDisparity_kernel.cuh"

// includes, project
#include <helper_cuda.h>      // helper for checking cuda initialization and error checking
#include <helper_functions.h> // helper for shared that are common to CUDA Samples
#include <helper_string.h>    // helper functions for string parsing

static const char *sSDKsample = "[stereoDisparity]\0";

int iDivUp(int a, int b) { return ((a % b) != 0) ? (a / b + 1) : (a / b); }

////////////////////////////////////////////////////////////////////////////////
// declaration, forward
void runTest(int argc, char **argv);

////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    printf("%s Starting...\n\n", sSDKsample);
    runTest(argc, argv);
}

////////////////////////////////////////////////////////////////////////////////
//! CUDA Sample for calculating depth maps
////////////////////////////////////////////////////////////////////////////////
void runTest(int argc, char **argv)
{
    cudaDeviceProp deviceProp;
    deviceProp.major = 0;
    deviceProp.minor = 0;
    int dev          = 0;

    // This will pick the best possible CUDA capable device
    dev = findCudaDevice(argc, (const char **)argv);

    checkCudaErrors(cudaGetDeviceProperties(&deviceProp, dev));

    // Statistics about the GPU device
    printf("> GPU device has %d Multi-Processors, SM %d.%d compute capabilities\n\n",
           deviceProp.multiProcessorCount,
           deviceProp.major,
           deviceProp.minor);

    StopWatchInterface *timer;
    sdkCreateTimer(&timer);

    // Search parameters
    int minDisp = -16;
    int maxDisp = 0;

    // Load image data
    // allocate mem for the images on host side
    // initialize pointers to NULL to request lib call to allocate as needed
    // PPM images are loaded into 4 byte/pixel memory (RGBX)
    unsigned char *h_img0 = NULL;
    unsigned char *h_img1 = NULL;
    unsigned int   w, h;
    char          *fname0 = sdkFindFilePath("stereo.im0.640x533.ppm", argv[0]);
    char          *fname1 = sdkFindFilePath("stereo.im1.640x533.ppm", argv[0]);

    printf("Loaded <%s> as image 0\n", fname0);

    if (!sdkLoadPPM4ub(fname0, &h_img0, &w, &h)) {
        fprintf(stderr, "Failed to load <%s>\n", fname0);
    }

    printf("Loaded <%s> as image 1\n", fname1);

    if (!sdkLoadPPM4ub(fname1, &h_img1, &w, &h)) {
        fprintf(stderr, "Failed to load <%s>\n", fname1);
    }

    dim3         numThreads = dim3(blockSize_x, blockSize_y, 1);
    dim3         numBlocks  = dim3(iDivUp(w, numThreads.x), iDivUp(h, numThreads.y));
    unsigned int numData    = w * h;
    unsigned int memSize    = sizeof(int) * numData;

    // allocate mem for the result on host side
    unsigned int *h_odata = (unsigned int *)malloc(memSize);

    // initialize the memory
    for (unsigned int i = 0; i < numData; i++)
        h_odata[i] = 0;

    // allocate device memory for result
    unsigned int *d_odata, *d_img0, *d_img1;

    checkCudaErrors(cudaMalloc((void **)&d_odata, memSize));
    checkCudaErrors(cudaMalloc((void **)&d_img0, memSize));
    checkCudaErrors(cudaMalloc((void **)&d_img1, memSize));

    // copy host memory to device to initialize to zeros
    checkCudaErrors(cudaMemcpy(d_img0, h_img0, memSize, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(d_img1, h_img1, memSize, cudaMemcpyHostToDevice));
    checkCudaErrors(cudaMemcpy(d_odata, h_odata, memSize, cudaMemcpyHostToDevice));

    cudaChannelFormatDesc ca_desc0 = cudaCreateChannelDesc<unsigned int>();
    cudaChannelFormatDesc ca_desc1 = cudaCreateChannelDesc<unsigned int>();

    cudaTextureObject_t tex2Dleft, tex2Dright;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                  = cudaResourceTypePitch2D;
    texRes.res.pitch2D.devPtr       = d_img0;
    texRes.res.pitch2D.desc         = ca_desc0;
    texRes.res.pitch2D.width        = w;
    texRes.res.pitch2D.height       = h;
    texRes.res.pitch2D.pitchInBytes = w * 4;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModePoint;
    texDescr.addressMode[0]   = cudaAddressModeClamp;
    texDescr.addressMode[1]   = cudaAddressModeClamp;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&tex2Dleft, &texRes, &texDescr, NULL));

    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType                  = cudaResourceTypePitch2D;
    texRes.res.pitch2D.devPtr       = d_img1;
    texRes.res.pitch2D.desc         = ca_desc1;
    texRes.res.pitch2D.width        = w;
    texRes.res.pitch2D.height       = h;
    texRes.res.pitch2D.pitchInBytes = w * 4;

    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = false;
    texDescr.filterMode       = cudaFilterModePoint;
    texDescr.addressMode[0]   = cudaAddressModeClamp;
    texDescr.addressMode[1]   = cudaAddressModeClamp;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&tex2Dright, &texRes, &texDescr, NULL));

    // First run the warmup kernel (which we'll use to get the GPU in the correct
    // max power state
    stereoDisparityKernel<<<numBlocks, numThreads>>>(
        d_img0, d_img1, d_odata, w, h, minDisp, maxDisp, tex2Dleft, tex2Dright);
    cudaDeviceSynchronize();

    // Allocate CUDA events that we'll use for timing
    cudaEvent_t start, stop;
    checkCudaErrors(cudaEventCreate(&start));
    checkCudaErrors(cudaEventCreate(&stop));

    printf("Launching CUDA stereoDisparityKernel()\n");

    // Record the start event
    checkCudaErrors(cudaEventRecord(start, NULL));

    // launch the stereoDisparity kernel
    stereoDisparityKernel<<<numBlocks, numThreads>>>(
        d_img0, d_img1, d_odata, w, h, minDisp, maxDisp, tex2Dleft, tex2Dright);

    // Record the stop event
    checkCudaErrors(cudaEventRecord(stop, NULL));

    // Wait for the stop event to complete
    checkCudaErrors(cudaEventSynchronize(stop));

    // Check to make sure the kernel didn't fail
    getLastCudaError("Kernel execution failed");

    float msecTotal = 0.0f;
    checkCudaErrors(cudaEventElapsedTime(&msecTotal, start, stop));

    // Copy result from device to host for verification
    checkCudaErrors(cudaMemcpy(h_odata, d_odata, memSize, cudaMemcpyDeviceToHost));

    printf("Input Size  [%dx%d], ", w, h);
    printf("Kernel size [%dx%d], ", (2 * RAD + 1), (2 * RAD + 1));
    printf("Disparities [%d:%d]\n", minDisp, maxDisp);

    printf("GPU processing time : %.4f (ms)\n", msecTotal);
    printf("Pixel throughput    : %.3f Mpixels/sec\n", ((float)(w * h * 1000.f) / msecTotal) / 1000000);

    // calculate sum of resultant GPU image
    unsigned int checkSum = 0;

    for (unsigned int i = 0; i < w * h; i++) {
        checkSum += h_odata[i];
    }

    printf("GPU Checksum = %u, ", checkSum);

    // write out the resulting disparity image.
    unsigned char *dispOut  = (unsigned char *)malloc(numData);
    int            mult     = 20;
    const char    *fnameOut = "output_GPU.pgm";

    for (unsigned int i = 0; i < numData; i++) {
        dispOut[i] = (int)h_odata[i] * mult;
    }

    printf("GPU image: <%s>\n", fnameOut);
    sdkSavePGM(fnameOut, dispOut, w, h);

    // compute reference solution
    printf("Computing CPU reference...\n");
    cpu_gold_stereo((unsigned int *)h_img0, (unsigned int *)h_img1, (unsigned int *)h_odata, w, h, minDisp, maxDisp);
    unsigned int cpuCheckSum = 0;

    for (unsigned int i = 0; i < w * h; i++) {
        cpuCheckSum += h_odata[i];
    }

    printf("CPU Checksum = %u, ", cpuCheckSum);
    const char *cpuFnameOut = "output_CPU.pgm";

    for (unsigned int i = 0; i < numData; i++) {
        dispOut[i] = (int)h_odata[i] * mult;
    }

    printf("CPU image: <%s>\n", cpuFnameOut);
    sdkSavePGM(cpuFnameOut, dispOut, w, h);

    // cleanup memory
    checkCudaErrors(cudaFree(d_odata));
    checkCudaErrors(cudaFree(d_img0));
    checkCudaErrors(cudaFree(d_img1));

    if (h_odata != NULL)
        free(h_odata);

    if (h_img0 != NULL)
        free(h_img0);

    if (h_img1 != NULL)
        free(h_img1);

    if (dispOut != NULL)
        free(dispOut);

    sdkDeleteTimer(&timer);

    exit((checkSum == cpuCheckSum) ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/stereoDisparity/stereoDisparity.cu`.

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

- **Total Lines**: 286
- **Approximate Size**: 10381 bytes

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
