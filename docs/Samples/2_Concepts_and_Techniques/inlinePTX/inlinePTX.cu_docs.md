# Documentation: Samples/2_Concepts_and_Techniques/inlinePTX/inlinePTX.cu
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/inlinePTX/inlinePTX.cu`
- **Filename**: `inlinePTX.cu`
- **Language**: cuda
- **Size**: 3554 bytes
- **Lines**: 109
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

/*
 * Demonstration of inline PTX (assembly language) usage in CUDA kernels
 */

// System includes
#include <assert.h>
#include <stdio.h>

// CUDA runtime
#include <cuda_runtime.h>

// helper functions and utilities to work with CUDA
#include <helper_cuda.h>
#include <helper_functions.h>

__global__ void sequence_gpu(int *d_ptr, int length)
{
    int elemID = blockIdx.x * blockDim.x + threadIdx.x;

    if (elemID < length) {
        unsigned int laneid;
        // This command gets the lane ID within the current warp
        asm("mov.u32 %0, %%laneid;" : "=r"(laneid));
        d_ptr[elemID] = laneid;
    }
}


void sequence_cpu(int *h_ptr, int length)
{
    for (int elemID = 0; elemID < length; elemID++) {
        h_ptr[elemID] = elemID % 32;
    }
}

int main(int argc, char **argv)
{
    printf("CUDA inline PTX assembler sample\n");

    const int N = 1000;

    int dev = findCudaDevice(argc, (const char **)argv);

    if (dev == -1) {
        return EXIT_FAILURE;
    }

    int *d_ptr;
    checkCudaErrors(cudaMalloc(&d_ptr, N * sizeof(int)));

    int *h_ptr;
    checkCudaErrors(cudaMallocHost(&h_ptr, N * sizeof(int)));

    dim3 cudaBlockSize(256, 1, 1);
    dim3 cudaGridSize((N + cudaBlockSize.x - 1) / cudaBlockSize.x, 1, 1);
    sequence_gpu<<<cudaGridSize, cudaBlockSize>>>(d_ptr, N);
    checkCudaErrors(cudaGetLastError());
    checkCudaErrors(cudaDeviceSynchronize());

    sequence_cpu(h_ptr, N);

    int *h_d_ptr;
    checkCudaErrors(cudaMallocHost(&h_d_ptr, N * sizeof(int)));
    checkCudaErrors(cudaMemcpy(h_d_ptr, d_ptr, N * sizeof(int), cudaMemcpyDeviceToHost));

    bool bValid = true;

    for (int i = 0; i < N && bValid; i++) {
        if (h_ptr[i] != h_d_ptr[i]) {
            bValid = false;
        }
    }

    printf("Test %s.\n", bValid ? "Successful" : "Failed");

    checkCudaErrors(cudaFree(d_ptr));
    checkCudaErrors(cudaFreeHost(h_ptr));
    checkCudaErrors(cudaFreeHost(h_d_ptr));

    return bValid ? EXIT_SUCCESS : EXIT_FAILURE;
}

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 3 function(s) in the CUDA Samples repository.

**Dependencies**: 5 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `assert.h`
- `stdio.h`
- `cuda_runtime.h`
- `helper_cuda.h`
- `helper_functions.h`

### CUDA Kernels
#### `sequence_gpu`
- **Return Type**: `void`
- **Parameters**: `int *d_ptr, int length`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void sequence_gpu(int *d_ptr, int length)`
- Function in Samples/2_Concepts_and_Techniques/inlinePTX/inlinePTX.cu

#### `void sequence_cpu(int *h_ptr, int length)`
- Function in Samples/2_Concepts_and_Techniques/inlinePTX/inlinePTX.cu

#### `int main(int argc, char **argv)`
- Function in Samples/2_Concepts_and_Techniques/inlinePTX/inlinePTX.cu


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

