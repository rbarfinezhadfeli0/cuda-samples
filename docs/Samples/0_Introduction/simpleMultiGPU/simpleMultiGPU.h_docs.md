# Documentation: Samples/0_Introduction/simpleMultiGPU/simpleMultiGPU.h
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleMultiGPU/simpleMultiGPU.h`
- **Filename**: `simpleMultiGPU.h`
- **Language**: h
- **Size**: 2438 bytes
- **Lines**: 63
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```h
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
 * This application demonstrates how to use the CUDA API to use multiple GPUs.
 *
 * Note that in order to detect multiple GPUs in your system you have to disable
 * SLI in the nvidia control panel. Otherwise only one GPU is visible to the
 * application. On the other side, you can still extend your desktop to screens
 * attached to both GPUs.
 */

#ifndef SIMPLEMULTIGPU_H
#define SIMPLEMULTIGPU_H

typedef struct
{
    // Host-side input data
    int    dataN;
    float *h_Data;

    // Partial sum for this GPU
    float *h_Sum;

    // Device buffers
    float *d_Data, *d_Sum;

    // Reduction copied back from GPU
    float *h_Sum_from_device;

    // Stream for asynchronous command execution
    cudaStream_t stream;

} TGPUplan;

extern "C" void launch_reduceKernel(float *d_Result, float *d_Input, int N, int BLOCK_N, int THREAD_N, cudaStream_t &s);

#endif

```

---
## High-Level Overview
This file is a h source file in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **SIMPLEMULTIGPU_H**: `typedef struct`


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


---
## Performance & Security Notes
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

