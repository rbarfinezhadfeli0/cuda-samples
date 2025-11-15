# Documentation: Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu
---
## File Metadata
- **Path**: `Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu`
- **Filename**: `matrixMul_kernel.cu`
- **Language**: cuda
- **Size**: 4884 bytes
- **Lines**: 129
- **Generated**: 2025-11-15 12:53:53 UTC

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

/* Matrix multiplication: C = A * B.
 * Device code.
 */

#ifndef _MATRIXMUL_KERNEL_H_
#define _MATRIXMUL_KERNEL_H_

#include <stdio.h>

#define AS(i, j) As[i][j]
#define BS(i, j) Bs[i][j]

////////////////////////////////////////////////////////////////////////////////
//! Matrix multiplication on the device: C = A * B
//! wA is A's width and wB is B's width
////////////////////////////////////////////////////////////////////////////////
template <int block_size, typename size_type>
__device__ void matrixMul(float *C, float *A, float *B, size_type wA, size_type wB)
{
    // Block index
    size_type bx = blockIdx.x;
    size_type by = blockIdx.y;

    // Thread index
    size_type tx = threadIdx.x;
    size_type ty = threadIdx.y;

    // Index of the first sub-matrix of A processed by the block
    size_type aBegin = wA * block_size * by;

    // Index of the last sub-matrix of A processed by the block
    size_type aEnd = aBegin + wA - 1;

    // Step size used to iterate through the sub-matrices of A
    size_type aStep = block_size;

    // Index of the first sub-matrix of B processed by the block
    size_type bBegin = block_size * bx;

    // Step size used to iterate through the sub-matrices of B
    size_type bStep = block_size * wB;

    // Csub is used to store the element of the block sub-matrix
    // that is computed by the thread
    float Csub = 0;

    // Loop over all the sub-matrices of A and B
    // required to compute the block sub-matrix
    for (size_type a = aBegin, b = bBegin; a <= aEnd; a += aStep, b += bStep) {
        // Declaration of the shared memory array As used to
        // store the sub-matrix of A
        __shared__ float As[block_size][block_size];

        // Declaration of the shared memory array Bs used to
        // store the sub-matrix of B
        __shared__ float Bs[block_size][block_size];

        // Load the matrices from device memory
        // to shared memory; each thread loads
        // one element of each matrix
        AS(ty, tx) = A[a + wA * ty + tx];
        BS(ty, tx) = B[b + wB * ty + tx];

        // Synchronize to make sure the matrices are loaded
        __syncthreads();

        // Multiply the two matrices together;
        // each thread computes one element
        // of the block sub-matrix
#pragma unroll

        for (size_type k = 0; k < block_size; ++k)
            Csub += AS(ty, k) * BS(k, tx);

        // Synchronize to make sure that the preceding
        // computation is done before loading two new
        // sub-matrices of A and B in the next iteration
        __syncthreads();
    }

    // Write the block sub-matrix to device memory;
    // each thread writes one element
    size_type c         = wB * block_size * by + block_size * bx;
    C[c + wB * ty + tx] = Csub;
}

// C wrappers around our template kernel
extern "C" __global__ void matrixMul_bs8_64bit(float *C, float *A, float *B, size_t wA, size_t wB)
{
    matrixMul<8, size_t>(C, A, B, wA, wB);
}
extern "C" __global__ void matrixMul_bs16_64bit(float *C, float *A, float *B, size_t wA, size_t wB)
{
    matrixMul<16, size_t>(C, A, B, wA, wB);
}
extern "C" __global__ void matrixMul_bs32_64bit(float *C, float *A, float *B, size_t wA, size_t wB)
{
    matrixMul<32, size_t>(C, A, B, wA, wB);
}

#endif // #ifndef _MATRIXMUL_KERNEL_H_

```

---
## High-Level Overview
This file is a cuda source file containing 3 CUDA kernel(s) with 5 function(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `stdio.h`

### Preprocessor Definitions
- **_MATRIXMUL_KERNEL_H_**: `#include <stdio.h>`
- **AS**: ``
- **BS**: ``

### CUDA Kernels
#### `matrixMul_bs8_64bit`
- **Return Type**: `void`
- **Parameters**: `float *C, float *A, float *B, size_t wA, size_t wB`
- **Description**: CUDA kernel function for GPU execution

#### `matrixMul_bs16_64bit`
- **Return Type**: `void`
- **Parameters**: `float *C, float *A, float *B, size_t wA, size_t wB`
- **Description**: CUDA kernel function for GPU execution

#### `matrixMul_bs32_64bit`
- **Return Type**: `void`
- **Parameters**: `float *C, float *A, float *B, size_t wA, size_t wB`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void matrixMul(float *C, float *A, float *B, size_type wA, size_type wB)`
- Function in Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu

#### `matrix for(size_type a = aBegin, b = bBegin; a <= aEnd; a += aStep, b += bStep)`
- Function in Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu

#### `void matrixMul_bs8_64bit(float *C, float *A, float *B, size_t wA, size_t wB)`
- Function in Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu

#### `void matrixMul_bs16_64bit(float *C, float *A, float *B, size_t wA, size_t wB)`
- Function in Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu

#### `void matrixMul_bs32_64bit(float *C, float *A, float *B, size_t wA, size_t wB)`
- Function in Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu


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

