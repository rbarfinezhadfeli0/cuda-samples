# Documentation: Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu`
- **Filename**: `BezierLineCDP.cu`
- **Language**: cuda
- **Size**: 6818 bytes
- **Lines**: 206
- **Generated**: 2025-11-15 12:53:50 UTC

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

#include <cuda_runtime_api.h>
#include <helper_cuda.h>
#include <stdio.h>
#include <string.h>

__forceinline__ __device__ float2 operator+(float2 a, float2 b)
{
    float2 c;
    c.x = a.x + b.x;
    c.y = a.y + b.y;
    return c;
}

__forceinline__ __device__ float2 operator-(float2 a, float2 b)
{
    float2 c;
    c.x = a.x - b.x;
    c.y = a.y - b.y;
    return c;
}

__forceinline__ __device__ float2 operator*(float a, float2 b)
{
    float2 c;
    c.x = a * b.x;
    c.y = a * b.y;
    return c;
}

__forceinline__ __device__ float length(float2 a) { return sqrtf(a.x * a.x + a.y * a.y); }

#define MAX_TESSELLATION 32
struct BezierLine
{
    float2  CP[3];
    float2 *vertexPos;
    int     nVertices;
};

__global__ void computeBezierLinePositions(int lidx, BezierLine *bLines, int nTessPoints)
{
    int idx = threadIdx.x + blockDim.x * blockIdx.x;

    if (idx < nTessPoints) {
        float u   = (float)idx / (float)(nTessPoints - 1);
        float omu = 1.0f - u;

        float B3u[3];

        B3u[0] = omu * omu;
        B3u[1] = 2.0f * u * omu;
        B3u[2] = u * u;

        float2 position = {0, 0};

        for (int i = 0; i < 3; i++) {
            position = position + B3u[i] * bLines[lidx].CP[i];
        }

        bLines[lidx].vertexPos[idx] = position;
    }
}

__global__ void computeBezierLinesCDP(BezierLine *bLines, int nLines)
{
    int lidx = threadIdx.x + blockDim.x * blockIdx.x;

    if (lidx < nLines) {
        float curvature = length(bLines[lidx].CP[1] - 0.5f * (bLines[lidx].CP[0] + bLines[lidx].CP[2]))
                        / length(bLines[lidx].CP[2] - bLines[lidx].CP[0]);
        int nTessPoints = min(max((int)(curvature * 16.0f), 4), MAX_TESSELLATION);

        if (bLines[lidx].vertexPos == NULL) {
            bLines[lidx].nVertices = nTessPoints;
            cudaMalloc((void **)&bLines[lidx].vertexPos, nTessPoints * sizeof(float2));
        }

        computeBezierLinePositions<<<ceilf((float)bLines[lidx].nVertices / 32.0f), 32>>>(
            lidx, bLines, bLines[lidx].nVertices);
    }
}

__global__ void freeVertexMem(BezierLine *bLines, int nLines)
{
    int lidx = threadIdx.x + blockDim.x * blockIdx.x;

    if (lidx < nLines)
        cudaFree(bLines[lidx].vertexPos);
}

unsigned int checkCapableSM35Device(int argc, char **argv)
{
    // Get device properties
    cudaDeviceProp properties;
    int            device_count = 0, device = -1;

    if (checkCmdLineFlag(argc, (const char **)argv, "device")) {
        device = getCmdLineArgumentInt(argc, (const char **)argv, "device");

        cudaDeviceProp properties;
        checkCudaErrors(cudaGetDeviceProperties(&properties, device));

        if (properties.major > 3 || (properties.major == 3 && properties.minor >= 5)) {
            printf("Running on GPU  %d (%s)\n", device, properties.name);
        }
        else {
            printf("cdpBezierTessellation requires GPU devices with compute SM 3.5 or "
                   "higher.");
            printf("Current GPU device has compute SM %d.%d. Exiting...\n", properties.major, properties.minor);
            return EXIT_FAILURE;
        }
    }
    else {
        checkCudaErrors(cudaGetDeviceCount(&device_count));

        for (int i = 0; i < device_count; ++i) {
            checkCudaErrors(cudaGetDeviceProperties(&properties, i));

            if (properties.major > 3 || (properties.major == 3 && properties.minor >= 5)) {
                device = i;
                printf("Running on GPU %d (%s)\n", i, properties.name);
                break;
            }

            printf("GPU %d %s does not support CUDA Dynamic Parallelism\n", i, properties.name);
        }
    }
    if (device == -1) {
        fprintf(stderr,
                "cdpBezierTessellation requires GPU devices with compute SM 3.5 or "
                "higher.  Exiting...\n");
        return EXIT_WAIVED;
    }

    return EXIT_SUCCESS;
}

#define N_LINES   256
#define BLOCK_DIM 64
int main(int argc, char **argv)
{
    BezierLine *bLines_h = new BezierLine[N_LINES];

    float2 last = {0, 0};

    for (int i = 0; i < N_LINES; i++) {
        bLines_h[i].CP[0] = last;

        for (int j = 1; j < 3; j++) {
            bLines_h[i].CP[j].x = (float)rand() / (float)RAND_MAX;
            bLines_h[i].CP[j].y = (float)rand() / (float)RAND_MAX;
        }

        last                  = bLines_h[i].CP[2];
        bLines_h[i].vertexPos = NULL;
        bLines_h[i].nVertices = 0;
    }

    unsigned int sm35Ret = checkCapableSM35Device(argc, argv);
    if (sm35Ret != EXIT_SUCCESS) {
        exit(sm35Ret);
    }

    BezierLine *bLines_d;
    checkCudaErrors(cudaMalloc((void **)&bLines_d, N_LINES * sizeof(BezierLine)));
    checkCudaErrors(cudaMemcpy(bLines_d, bLines_h, N_LINES * sizeof(BezierLine), cudaMemcpyHostToDevice));
    printf("Computing Bezier Lines (CUDA Dynamic Parallelism Version) ... ");
    computeBezierLinesCDP<<<(unsigned int)ceil((float)N_LINES / (float)BLOCK_DIM), BLOCK_DIM>>>(bLines_d, N_LINES);
    printf("Done!\n");

    // Do something to draw the lines here

    freeVertexMem<<<(unsigned int)ceil((float)N_LINES / (float)BLOCK_DIM), BLOCK_DIM>>>(bLines_d, N_LINES);
    checkCudaErrors(cudaFree(bLines_d));
    delete[] bLines_h;

    exit(EXIT_SUCCESS);
}

```

---
## High-Level Overview
This file is a cuda source file containing 3 CUDA kernel(s) with 6 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda_runtime_api.h`
- `helper_cuda.h`
- `stdio.h`
- `string.h`

### Preprocessor Definitions
- **MAX_TESSELLATION**: `32`
- **N_LINES**: `256`
- **BLOCK_DIM**: `64`

### CUDA Kernels
#### `computeBezierLinePositions`
- **Return Type**: `void`
- **Parameters**: `int lidx, BezierLine *bLines, int nTessPoints`
- **Description**: CUDA kernel function for GPU execution

#### `computeBezierLinesCDP`
- **Return Type**: `void`
- **Parameters**: `BezierLine *bLines, int nLines`
- **Description**: CUDA kernel function for GPU execution

#### `freeVertexMem`
- **Return Type**: `void`
- **Parameters**: `BezierLine *bLines, int nLines`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `float length(float2 a)`
- Function in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

#### `void computeBezierLinePositions(int lidx, BezierLine *bLines, int nTessPoints)`
- Function in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

#### `void computeBezierLinesCDP(BezierLine *bLines, int nLines)`
- Function in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

#### `void freeVertexMem(BezierLine *bLines, int nLines)`
- Function in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

#### `int checkCapableSM35Device(int argc, char **argv)`
- Function in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

#### `int main(int argc, char **argv)`
- Function in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu

### Classes / Structures
#### `struct BezierLine`
- Defined in Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu


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

