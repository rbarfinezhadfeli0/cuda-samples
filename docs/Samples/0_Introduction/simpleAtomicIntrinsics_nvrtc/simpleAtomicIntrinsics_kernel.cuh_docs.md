# Documentation: Samples/0_Introduction/simpleAtomicIntrinsics_nvrtc/simpleAtomicIntrinsics_kernel.cuh
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleAtomicIntrinsics_nvrtc/simpleAtomicIntrinsics_kernel.cuh`
- **Filename**: `simpleAtomicIntrinsics_kernel.cuh`
- **Language**: cuda
- **Size**: 3018 bytes
- **Lines**: 82
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

/* Simple kernel demonstrating atomic functions in device code. */

#ifndef _SIMPLEATOMICS_KERNEL_H_
#define _SIMPLEATOMICS_KERNEL_H_

////////////////////////////////////////////////////////////////////////////////
//! Simple test kernel for atomic instructions
//! @param g_idata  input data in global memory
//! @param g_odata  output data in global memory
////////////////////////////////////////////////////////////////////////////////

extern "C" __global__ void testKernel(int *g_odata)
{
    // access thread id
    const unsigned int tid = blockDim.x * blockIdx.x + threadIdx.x;

    // Test various atomic instructions
    // Arithmetic atomic instructions
    // Atomic addition
    atomicAdd(&g_odata[0], 10);

    // Atomic subtraction (final should be 0)
    atomicSub(&g_odata[1], 10);

    // Atomic exchange
    atomicExch(&g_odata[2], tid);

    // Atomic maximum
    atomicMax(&g_odata[3], tid);

    // Atomic minimum
    atomicMin(&g_odata[4], tid);

    // Atomic increment (modulo 17+1)
    atomicInc((unsigned int *)&g_odata[5], 17);

    // Atomic decrement
    atomicDec((unsigned int *)&g_odata[6], 137);

    // Atomic compare-and-swap
    atomicCAS(&g_odata[7], tid - 1, tid);

    // Bitwise atomic instructions
    // Atomic AND
    atomicAnd(&g_odata[8], 2 * tid + 7);

    // Atomic OR
    atomicOr(&g_odata[9], 1 << tid);

    // Atomic XOR
    atomicXor(&g_odata[10], tid);
}

#endif // #ifndef _SIMPLEATOMICS_KERNEL_H_

```

---
## High-Level Overview
This file is a cuda source file containing 1 CUDA kernel(s) with 1 function(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **_SIMPLEATOMICS_KERNEL_H_**: `////////////////////////////////////////////////////////////////////////////////`

### CUDA Kernels
#### `testKernel`
- **Return Type**: `void`
- **Parameters**: `int *g_odata`
- **Description**: CUDA kernel function for GPU execution

### Functions
#### `void testKernel(int *g_odata)`
- Function in Samples/0_Introduction/simpleAtomicIntrinsics_nvrtc/simpleAtomicIntrinsics_kernel.cuh


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

