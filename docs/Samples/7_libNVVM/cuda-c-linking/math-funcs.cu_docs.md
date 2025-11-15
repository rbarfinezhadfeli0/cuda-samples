# Documentation: Samples/7_libNVVM/cuda-c-linking/math-funcs.cu
---
## File Metadata
- **Path**: `Samples/7_libNVVM/cuda-c-linking/math-funcs.cu`
- **Filename**: `math-funcs.cu`
- **Language**: cuda
- **Size**: 3035 bytes
- **Lines**: 85
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```cuda
// Copyright (c) 1993-2023, NVIDIA CORPORATION. All rights reserved.
//
// Redistribution and use in source and binary forms, with or without
// modification, are permitted provided that the following conditions
// are met:
//  * Redistributions of source code must retain the above copyright
//    notice, this list of conditions and the following disclaimer.
//  * Redistributions in binary form must reproduce the above copyright
//    notice, this list of conditions and the following disclaimer in the
//    documentation and/or other materials provided with the distribution.
//  * Neither the name of NVIDIA CORPORATION nor the names of its
//    contributors may be used to endorse or promote products derived
//    from this software without specific prior written permission.
//
// THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
// EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
// IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
// PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
// CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
// EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
// PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
// PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
// OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
// (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
// OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

// Simple implementation of Mandelbrot set from Wikipedia
// http://en.wikipedia.org/wiki/Mandelbrot_set

// Note that this kernel is meant to be a simple, straight-forward
// implementation, and so may not represent optimized GPU code.
extern "C" __device__ void mandelbrot(float *Data)
{

    // Which pixel am I?
    unsigned DataX  = blockIdx.x * blockDim.x + threadIdx.x;
    unsigned DataY  = blockIdx.y * blockDim.y + threadIdx.y;
    unsigned Width  = gridDim.x * blockDim.x;
    unsigned Height = gridDim.y * blockDim.y;

    float R, G, B, A;

    // Scale coordinates to (-2.5, 1) and (-1, 1)

    float NormX = (float)DataX / (float)Width;
    NormX *= 3.5f;
    NormX -= 2.5f;

    float NormY = (float)DataY / (float)Height;
    NormY *= 2.0f;
    NormY -= 1.0f;

    float X0 = NormX;
    float Y0 = NormY;

    float X = 0.0f;
    float Y = 0.0f;

    unsigned Iter    = 0;
    unsigned MaxIter = 1000;

    // Iterate
    while (X * X + Y * Y < 4.0f && Iter < MaxIter) {
        float XTemp = X * X - Y * Y + X0;
        Y           = 2.0f * X * Y + Y0;

        X = XTemp;

        Iter++;
    }

    unsigned ColorG = Iter % 50;
    unsigned ColorB = Iter % 25;

    R = 0.0f;
    G = (float)ColorG / 50.0f;
    B = (float)ColorB / 25.0f;
    A = 1.0f;

    Data[DataY * Width * 4 + DataX * 4 + 0] = R;
    Data[DataY * Width * 4 + DataX * 4 + 1] = G;
    Data[DataY * Width * 4 + DataX * 4 + 2] = B;
    Data[DataY * Width * 4 + DataX * 4 + 3] = A;
}

```

---
## High-Level Overview
This file is a cuda source file with 2 function(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Functions
#### `void mandelbrot(float *Data)`
- Function in Samples/7_libNVVM/cuda-c-linking/math-funcs.cu

#### `Iterate while(X * X + Y * Y < 4.0f && Iter < MaxIter)`
- Function in Samples/7_libNVVM/cuda-c-linking/math-funcs.cu


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

