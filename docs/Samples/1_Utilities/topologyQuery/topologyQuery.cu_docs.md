# Documentation: Samples/1_Utilities/topologyQuery/topologyQuery.cu
---
## File Metadata
- **Path**: `Samples/1_Utilities/topologyQuery/topologyQuery.cu`
- **Filename**: `topologyQuery.cu`
- **Language**: cuda
- **Size**: 3485 bytes
- **Lines**: 79
- **Generated**: 2025-11-15 12:53:51 UTC

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
 * This sample demonstrates how to use query information on the current system
 * topology using a SDK 8.0 API.
 */

// includes CUDA
#include <cuda_runtime.h>

// includes, project
#include <helper_cuda.h>
#include <helper_functions.h> // helper for shared that are common to CUDA Samples

int main(int argc, char **argv)
{
    int deviceCount = 0;
    checkCudaErrors(cudaGetDeviceCount(&deviceCount));

    // Enumerates Device <-> Device links
    for (int device1 = 0; device1 < deviceCount; device1++) {
        for (int device2 = 0; device2 < deviceCount; device2++) {
            if (device1 == device2)
                continue;

            int perfRank        = 0;
            int atomicSupported = 0;
            int accessSupported = 0;

            checkCudaErrors(
                cudaDeviceGetP2PAttribute(&accessSupported, cudaDevP2PAttrAccessSupported, device1, device2));
            checkCudaErrors(cudaDeviceGetP2PAttribute(&perfRank, cudaDevP2PAttrPerformanceRank, device1, device2));
            checkCudaErrors(
                cudaDeviceGetP2PAttribute(&atomicSupported, cudaDevP2PAttrNativeAtomicSupported, device1, device2));

            if (accessSupported) {
                std::cout << "GPU" << device1 << " <-> GPU" << device2 << ":" << std::endl;
                std::cout << "  * Atomic Supported: " << (atomicSupported ? "yes" : "no") << std::endl;
                std::cout << "  * Perf Rank: " << perfRank << std::endl;
            }
        }
    }

    // Enumerates Device <-> Host links
    for (int device = 0; device < deviceCount; device++) {
        int atomicSupported = 0;
        checkCudaErrors(cudaDeviceGetAttribute(&atomicSupported, cudaDevAttrHostNativeAtomicSupported, device));
        std::cout << "GPU" << device << " <-> CPU:" << std::endl;
        std::cout << "  * Atomic Supported: " << (atomicSupported ? "yes" : "no") << std::endl;
    }

    return 0;
}

```

---
## High-Level Overview
This file is a cuda source file with 3 function(s) in the CUDA Samples repository.

**Dependencies**: 3 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda_runtime.h`
- `helper_cuda.h`
- `helper_functions.h`

### Functions
#### `int main(int argc, char **argv)`
- Function in Samples/1_Utilities/topologyQuery/topologyQuery.cu

#### `links for(int device1 = 0; device1 < deviceCount; device1++)`
- Function in Samples/1_Utilities/topologyQuery/topologyQuery.cu

#### `links for(int device = 0; device < deviceCount; device++)`
- Function in Samples/1_Utilities/topologyQuery/topologyQuery.cu


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

