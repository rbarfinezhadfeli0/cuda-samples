# Documentation: Samples/0_Introduction/clock/clock.cu
---
## File Metadata
- **Path**: `Samples/0_Introduction/clock/clock.cu`
- **Filename**: `clock.cu`
- **Language**: cuda
- **Size**: 5266 bytes
- **Lines**: 153
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

/*
 * This example shows how to use the clock function to measure the performance
 * of block of threads of a kernel accurately. Blocks are executed in parallel
 * and out of order. Since there's no synchronization mechanism between blocks,
 * we measure the clock once for each block. The clock samples are written to
 * device memory.
 */

// System includes
#include <assert.h>
#include <stdint.h>
#include <stdio.h>

// CUDA runtime
#include <cuda_runtime.h>

// helper functions and utilities to work with CUDA
#include <helper_cuda.h>
#include <helper_functions.h>

// This kernel computes a standard parallel reduction and evaluates the
// time it takes to do that for each block. The timing results are stored
// in device memory.
__global__ static void timedReduction(const float *input, float *output, clock_t *timer)
{
    // __shared__ float shared[2 * blockDim.x];
    extern __shared__ float shared[];

    const int tid = threadIdx.x;
    const int bid = blockIdx.x;

    if (tid == 0)
        timer[bid] = clock();

    // Copy input.
    shared[tid]              = input[tid];
    shared[tid + blockDim.x] = input[tid + blockDim.x];

    // Perform reduction to find minimum.
    for (int d = blockDim.x; d > 0; d /= 2) {
        __syncthreads();

        if (tid < d) {
            float f0 = shared[tid];
            float f1 = shared[tid + d];

            if (f1 < f0) {
                shared[tid] = f1;
            }
        }
    }

    // Write result.
    if (tid == 0)
        output[bid] = shared[0];

    __syncthreads();

    if (tid == 0)
        timer[bid + gridDim.x] = clock();
}

#define NUM_BLOCKS  64
#define NUM_THREADS 256

// It's interesting to change the number of blocks and the number of threads to
// understand how to keep the hardware busy.
//
// Here are some numbers I get on my G80:
//    blocks - clocks
//    1 - 3096
//    8 - 3232
//    16 - 3364
//    32 - 4615
//    64 - 9981
//
// With less than 16 blocks some of the multiprocessors of the device are idle.
// With more than 16 you are using all the multiprocessors, but there's only one
// block per multiprocessor and that doesn't allow you to hide the latency of
// the memory. With more than 32 the speed scales linearly.

// Start the main CUDA Sample here
int main(int argc, char **argv)
{
    printf("CUDA Clock sample\n");

    // This will pick the best possible CUDA capable device
    int dev = findCudaDevice(argc, (const char **)argv);

    float   *dinput  = NULL;
    float   *doutput = NULL;
    clock_t *dtimer  = NULL;

    clock_t timer[NUM_BLOCKS * 2];
    float   input[NUM_THREADS * 2];

    for (int i = 0; i < NUM_THREADS * 2; i++) {
        input[i] = (float)i;
    }

    checkCudaErrors(cudaMalloc((void **)&dinput, sizeof(float) * NUM_THREADS * 2));
    checkCudaErrors(cudaMalloc((void **)&doutput, sizeof(float) * NUM_BLOCKS));
    checkCudaErrors(cudaMalloc((void **)&dtimer, sizeof(clock_t) * NUM_BLOCKS * 2));

    checkCudaErrors(cudaMemcpy(dinput, input, sizeof(float) * NUM_THREADS * 2, cudaMemcpyHostToDevice));

    timedReduction<<<NUM_BLOCKS, NUM_THREADS, sizeof(float) * 2 * NUM_THREADS>>>(dinput, doutput, dtimer);

    checkCudaErrors(cudaMemcpy(timer, dtimer, sizeof(clock_t) * NUM_BLOCKS * 2, cudaMemcpyDeviceToHost));

    checkCudaErrors(cudaFree(dinput));
    checkCudaErrors(cudaFree(doutput));
    checkCudaErrors(cudaFree(dtimer));

    long double avgElapsedClocks = 0;

    for (int i = 0; i < NUM_BLOCKS; i++) {
        avgElapsedClocks += (long double)(timer[i + NUM_BLOCKS] - timer[i]);
    }

    avgElapsedClocks = avgElapsedClocks / NUM_BLOCKS;
    printf("Average clocks/block = %Lf\n", avgElapsedClocks);

    return EXIT_SUCCESS;
}

```

---
## High-Level Overview
This file is a cuda source file with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 6 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `assert.h`
- `stdint.h`
- `stdio.h`
- `cuda_runtime.h`
- `helper_cuda.h`
- `helper_functions.h`

### Preprocessor Definitions
- **NUM_BLOCKS**: `64`
- **NUM_THREADS**: `256`

### Functions
#### `void timedReduction(const float *input, float *output, clock_t *timer)`
- Function in Samples/0_Introduction/clock/clock.cu

#### `int main(int argc, char **argv)`
- Function in Samples/0_Introduction/clock/clock.cu


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

