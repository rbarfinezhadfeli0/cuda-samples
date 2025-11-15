# Documentation: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu`
- **Filename**: `kernel.cu`
- **Language**: cuda
- **Size**: 6018 bytes
- **Lines**: 151
- **Generated**: 2025-11-15 12:53:54 UTC

---
## Original Source
```cuda
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

#include <EGL/egl.h>
#include <EGL/eglext.h>
#include <cuda.h>
#include <cuda_runtime.h>
#include <stdio.h>
#include <string.h>

#include "eglstrm_common.h"

extern bool isCrossDevice;

__device__ static unsigned int numErrors = 0, errorFound = 0;
__device__ void                checkProducerDataGPU(char *data, int size, char expectedVal, int frameNumber)
{
    if ((data[blockDim.x * blockIdx.x + threadIdx.x] != expectedVal) && (!errorFound)) {
        printf("Producer FOUND:%d expected: %d at %d for trial %d %d\n",
               data[blockDim.x * blockIdx.x + threadIdx.x],
               expectedVal,
               (blockDim.x * blockIdx.x + threadIdx.x),
               frameNumber,
               numErrors);
        numErrors++;
        errorFound = 1;
        return;
    }
}

__device__ void checkConsumerDataGPU(char *data, int size, char expectedVal, int frameNumber)
{
    if ((data[blockDim.x * blockIdx.x + threadIdx.x] != expectedVal) && (!errorFound)) {
        printf("Consumer FOUND:%d expected: %d at %d for trial %d %d\n",
               data[blockDim.x * blockIdx.x + threadIdx.x],
               expectedVal,
               (blockDim.x * blockIdx.x + threadIdx.x),
               frameNumber,
               numErrors);
        numErrors++;
        errorFound = 1;
        return;
    }
}

__global__ void writeDataToBuffer(char *pSrc, char newVal) { pSrc[blockDim.x * blockIdx.x + threadIdx.x] = newVal; }

__global__ void testKernelConsumer(char *pSrc, char size, char expectedVal, char newVal, int frameNumber)
{
    checkConsumerDataGPU(pSrc, size, expectedVal, frameNumber);
}

__global__ void testKernelProducer(char *pSrc, char size, char expectedVal, char newVal, int frameNumber)
{
    checkProducerDataGPU(pSrc, size, expectedVal, frameNumber);
}
__global__ void getNumErrors(int *numErr) { *numErr = numErrors; }

cudaError_t cudaProducer_filter(cudaStream_t pStream,
                                char        *pSrc,
                                int          width,
                                int          height,
                                char         expectedVal,
                                char         newVal,
                                int          frameNumber)
{
    // in case where consumer is on dgpu and producer is on igpu when return is
    // called the frame is not copied back to igpu. So the consumer changes is not
    // visible to producer
    if (isCrossDevice == 0) {
        testKernelProducer<<<(width * height) / 1024, 1024, 1, pStream>>>(
            pSrc, width * height, expectedVal, newVal, frameNumber);
    }
    writeDataToBuffer<<<(width * height) / 1024, 1024, 1, pStream>>>(pSrc, newVal);
    return cudaSuccess;
};

cudaError_t cudaConsumer_filter(cudaStream_t cStream,
                                char        *pSrc,
                                int          width,
                                int          height,
                                char         expectedVal,
                                char         newVal,
                                int          frameNumber)
{
    testKernelConsumer<<<(width * height) / 1024, 1024, 1, cStream>>>(
        pSrc, width * height, expectedVal, newVal, frameNumber);
    writeDataToBuffer<<<(width * height) / 1024, 1024, 1, cStream>>>(pSrc, newVal);
    return cudaSuccess;
};

cudaError_t cudaGetValueMismatch()
{
    int         numErr_h;
    int        *numErr_d = NULL;
    cudaError_t err      = cudaSuccess;
    err                  = cudaMalloc(&numErr_d, sizeof(int));
    if (err != cudaSuccess) {
        printf("Cuda Main: cudaMalloc failed with %s\n", cudaGetErrorString(err));
        return err;
    }
    getNumErrors<<<1, 1>>>(numErr_d);
    err = cudaDeviceSynchronize();
    if (err != cudaSuccess) {
        printf("Cuda Main: cudaDeviceSynchronize failed with %s\n", cudaGetErrorString(err));
    }
    err = cudaMemcpy(&numErr_h, numErr_d, sizeof(int), cudaMemcpyDeviceToHost);
    if (err != cudaSuccess) {
        printf("Cuda Main: cudaMemcpy failed with %s\n", cudaGetErrorString(err));
        cudaFree(numErr_d);
        return err;
    }
    err = cudaFree(numErr_d);
    if (err != cudaSuccess) {
        printf("Cuda Main: cudaFree failed with %s\n", cudaGetErrorString(err));
        return err;
    }
    if (numErr_h > 0) {
        return cudaErrorUnknown;
    }
    return cudaSuccess;
}

```

---
## High-Level Overview
This file is a cuda source file containing 4 CUDA kernel(s) with 10 function(s) in the CUDA Samples repository.

**Dependencies**: 7 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `EGL/egl.h`
- `EGL/eglext.h`
- `cuda.h`
- `cuda_runtime.h`
- `stdio.h`
- `string.h`
- `eglstrm_common.h`

### CUDA Kernels
#### `writeDataToBuffer`
- **Return Type**: `void`
- **Parameters**: `char *pSrc, char newVal`
- **Description**: CUDA kernel function for GPU execution

#### `testKernelConsumer`
- **Return Type**: `void`
- **Parameters**: `char *pSrc, char size, char expectedVal, char newVal, int frameNumber`
- **Description**: CUDA kernel function for GPU execution

#### `testKernelProducer`
- **Return Type**: `void`
- **Parameters**: `char *pSrc, char size, char expectedVal, char newVal, int frameNumber`
- **Description**: CUDA kernel function for GPU execution

#### `getNumErrors`
- **Return Type**: `void`
- **Parameters**: `int *numErr`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void checkProducerDataGPU(char *data, int size, char expectedVal, int frameNumber)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `void checkConsumerDataGPU(char *data, int size, char expectedVal, int frameNumber)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `void writeDataToBuffer(char *pSrc, char newVal)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `void testKernelConsumer(char *pSrc, char size, char expectedVal, char newVal, int frameNumber)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `void testKernelProducer(char *pSrc, char size, char expectedVal, char newVal, int frameNumber)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `void getNumErrors(int *numErr)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `cudaError_t cudaProducer_filter(cudaStream_t pStream,
                                char        *pSrc,
                                int          width,
                                int          height,
                                char         expectedVal,
                                char         newVal,
                                int          frameNumber)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `producer if(isCrossDevice == 0)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `cudaError_t cudaConsumer_filter(cudaStream_t cStream,
                                char        *pSrc,
                                int          width,
                                int          height,
                                char         expectedVal,
                                char         newVal,
                                int          frameNumber)`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu

#### `cudaError_t cudaGetValueMismatch()`
- Function in Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu


---
## Usage Examples
This is a CUDA source file. Typical usage involves:
1. Compiling with nvcc (NVIDIA CUDA Compiler)
2. Linking with CUDA runtime libraries
3. Executing on NVIDIA GPU hardware


---
## Performance & Security Notes
### Performance Considerations
- CUDA kernel execution is asynchronous
- Memory transfers between host and device can be a bottleneck
- Thread block and grid dimensions affect performance

### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

