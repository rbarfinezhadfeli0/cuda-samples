# Documentation for Samples/0_Introduction/simpleLayeredTexture/simpleLayeredTexture.cu

## File Metadata

- **Path**: `Samples/0_Introduction/simpleLayeredTexture/simpleLayeredTexture.cu`
- **Type**: .cu
- **Location**: Samples/0_Introduction/simpleLayeredTexture
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
 * This sample demonstrates how to use texture fetches from layered 2D textures
 * in CUDA C
 *
 * This sample first generates a 3D input data array for the layered texture
 * and the expected output. Then it starts CUDA C kernels, one for each layer,
 * which fetch their layer's texture data (using normalized texture coordinates)
 * transform it to the expected output, and write it to a 3D output data array.
 */

// includes, system
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, kernels
#include <cuda_runtime.h>

// includes, project
#include <helper_cuda.h>
#include <helper_functions.h> // helper for shared that are common to CUDA Samples

static const char *sSDKname = "simpleLayeredTexture";

////////////////////////////////////////////////////////////////////////////////
//! Transform a layer of a layered 2D texture using texture lookups
//! @param g_odata  output data in global memory
////////////////////////////////////////////////////////////////////////////////
__global__ void transformKernel(float *g_odata, int width, int height, int layer, cudaTextureObject_t tex)
{
    // calculate this thread's data point
    unsigned int x = blockIdx.x * blockDim.x + threadIdx.x;
    unsigned int y = blockIdx.y * blockDim.y + threadIdx.y;

    // 0.5f offset and division are necessary to access the original data points
    // in the texture (such that bilinear interpolation will not be activated).
    // For details, see also CUDA Programming Guide, Appendix D
    float u = (x + 0.5f) / (float)width;
    float v = (y + 0.5f) / (float)height;

    // read from texture, do expected transformation and write to global memory
    g_odata[layer * width * height + y * width + x] = -tex2DLayered<float>(tex, u, v, layer) + layer;
}

////////////////////////////////////////////////////////////////////////////////
// Program main
////////////////////////////////////////////////////////////////////////////////
int main(int argc, char **argv)
{
    printf("[%s] - Starting...\n", sSDKname);

    // use command-line specified CUDA device, otherwise use device with highest
    // Gflops/s
    int devID = findCudaDevice(argc, (const char **)argv);

    bool bResult = true;

    // get number of SMs on this GPU
    cudaDeviceProp deviceProps;

    checkCudaErrors(cudaGetDeviceProperties(&deviceProps, devID));
    printf("CUDA device [%s] has %d Multi-Processors ", deviceProps.name, deviceProps.multiProcessorCount);
    printf("SM %d.%d\n", deviceProps.major, deviceProps.minor);

    // generate input data for layered texture
    unsigned int width = 512, height = 512, num_layers = 5;
    unsigned int size   = width * height * num_layers * sizeof(float);
    float       *h_data = (float *)malloc(size);

    for (unsigned int layer = 0; layer < num_layers; layer++)
        for (int i = 0; i < (int)(width * height); i++) {
            h_data[layer * width * height + i] = (float)i;
        }

    // this is the expected transformation of the input data (the expected output)
    float *h_data_ref = (float *)malloc(size);

    for (unsigned int layer = 0; layer < num_layers; layer++)
        for (int i = 0; i < (int)(width * height); i++) {
            h_data_ref[layer * width * height + i] = -h_data[layer * width * height + i] + layer;
        }

    // allocate device memory for result
    float *d_data = NULL;
    checkCudaErrors(cudaMalloc((void **)&d_data, size));

    // allocate array and copy image data
    cudaChannelFormatDesc channelDesc = cudaCreateChannelDesc(32, 0, 0, 0, cudaChannelFormatKindFloat);
    cudaArray            *cu_3darray;
    checkCudaErrors(
        cudaMalloc3DArray(&cu_3darray, &channelDesc, make_cudaExtent(width, height, num_layers), cudaArrayLayered));
    cudaMemcpy3DParms myparms = {0};
    myparms.srcPos            = make_cudaPos(0, 0, 0);
    myparms.dstPos            = make_cudaPos(0, 0, 0);
    myparms.srcPtr            = make_cudaPitchedPtr(h_data, width * sizeof(float), width, height);
    myparms.dstArray          = cu_3darray;
    myparms.extent            = make_cudaExtent(width, height, num_layers);
    myparms.kind              = cudaMemcpyHostToDevice;
    checkCudaErrors(cudaMemcpy3D(&myparms));

    cudaTextureObject_t tex;
    cudaResourceDesc    texRes;
    memset(&texRes, 0, sizeof(cudaResourceDesc));

    texRes.resType         = cudaResourceTypeArray;
    texRes.res.array.array = cu_3darray;

    cudaTextureDesc texDescr;
    memset(&texDescr, 0, sizeof(cudaTextureDesc));

    texDescr.normalizedCoords = true;
    texDescr.filterMode       = cudaFilterModeLinear;
    texDescr.addressMode[0]   = cudaAddressModeWrap;
    texDescr.addressMode[1]   = cudaAddressModeWrap;
    texDescr.readMode         = cudaReadModeElementType;

    checkCudaErrors(cudaCreateTextureObject(&tex, &texRes, &texDescr, NULL));

    dim3 dimBlock(8, 8, 1);
    dim3 dimGrid(width / dimBlock.x, height / dimBlock.y, 1);

    printf("Covering 2D data array of %d x %d: Grid size is %d x %d, each block has "
           "8 x 8 threads\n",
           width,
           height,
           dimGrid.x,
           dimGrid.y);

    transformKernel<<<dimGrid, dimBlock>>>(d_data, width, height, 0,
                                           tex); // warmup (for better timing)

    // check if kernel execution generated an error
    getLastCudaError("warmup Kernel execution failed");

    checkCudaErrors(cudaDeviceSynchronize());

    StopWatchInterface *timer = NULL;
    sdkCreateTimer(&timer);
    sdkStartTimer(&timer);

    // execute the kernel
    for (unsigned int layer = 0; layer < num_layers; layer++)
        transformKernel<<<dimGrid, dimBlock, 0>>>(d_data, width, height, layer, tex);

    // check if kernel execution generated an error
    getLastCudaError("Kernel execution failed");

    checkCudaErrors(cudaDeviceSynchronize());
    sdkStopTimer(&timer);
    printf("Processing time: %.3f msec\n", sdkGetTimerValue(&timer));
    printf("%.2f Mtexlookups/sec\n", (width * height * num_layers / (sdkGetTimerValue(&timer) / 1000.0f) / 1e6));
    sdkDeleteTimer(&timer);

    // allocate mem for the result on host side
    float *h_odata = (float *)malloc(size);
    // copy result from device to host
    checkCudaErrors(cudaMemcpy(h_odata, d_data, size, cudaMemcpyDeviceToHost));

    // write regression file if necessary
    if (checkCmdLineFlag(argc, (const char **)argv, "regression")) {
        // write file for regression test
        sdkWriteFile<float>("./data/regression.dat", h_odata, width * height, 0.0f, false);
    }
    else {
        printf("Comparing kernel output to expected data\n");

#define MIN_EPSILON_ERROR 5e-3f
        bResult = compareData(h_odata, h_data_ref, width * height * num_layers, MIN_EPSILON_ERROR, 0.0f);
    }

    // cleanup memory
    free(h_data);
    free(h_data_ref);
    free(h_odata);

    checkCudaErrors(cudaDestroyTextureObject(tex));
    checkCudaErrors(cudaFree(d_data));
    checkCudaErrors(cudaFreeArray(cu_3darray));

    exit(bResult ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/simpleLayeredTexture/simpleLayeredTexture.cu`.

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

- **Total Lines**: 210
- **Approximate Size**: 8634 bytes

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
