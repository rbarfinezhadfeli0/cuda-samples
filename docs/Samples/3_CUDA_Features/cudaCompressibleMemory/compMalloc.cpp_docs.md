# Documentation: Samples/3_CUDA_Features/cudaCompressibleMemory/compMalloc.cpp
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/cudaCompressibleMemory/compMalloc.cpp`
- **Filename**: `compMalloc.cpp`
- **Language**: cpp
- **Size**: 4770 bytes
- **Lines**: 116
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```cpp
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

#include <cuda.h>
#include <cuda_runtime_api.h>
#include <helper_cuda.h>
#include <stdio.h>
#include <string.h>

cudaError_t setProp(CUmemAllocationProp *prop, bool UseCompressibleMemory)
{
    CUdevice currentDevice;
    if (cuCtxGetDevice(&currentDevice) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;

    memset(prop, 0, sizeof(CUmemAllocationProp));
    prop->type          = CU_MEM_ALLOCATION_TYPE_PINNED;
    prop->location.type = CU_MEM_LOCATION_TYPE_DEVICE;
    prop->location.id   = currentDevice;

    if (UseCompressibleMemory)
        prop->allocFlags.compressionType = CU_MEM_ALLOCATION_COMP_GENERIC;

    return cudaSuccess;
}

cudaError_t allocateCompressible(void **adr, size_t size, bool UseCompressibleMemory)
{
    CUmemAllocationProp prop = {};
    cudaError_t         err  = setProp(&prop, UseCompressibleMemory);
    if (err != cudaSuccess)
        return err;

    size_t granularity = 0;
    if (cuMemGetAllocationGranularity(&granularity, &prop, CU_MEM_ALLOC_GRANULARITY_MINIMUM) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;
    size = ((size - 1) / granularity + 1) * granularity;
    CUdeviceptr dptr;
    if (cuMemAddressReserve(&dptr, size, 0, 0, 0) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;

    CUmemGenericAllocationHandle allocationHandle;
    if (cuMemCreate(&allocationHandle, size, &prop, 0) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;

    // Check if cuMemCreate was able to allocate compressible memory.
    if (UseCompressibleMemory) {
        CUmemAllocationProp allocationProp = {};
        cuMemGetAllocationPropertiesFromHandle(&allocationProp, allocationHandle);
        if (allocationProp.allocFlags.compressionType != CU_MEM_ALLOCATION_COMP_GENERIC) {
            printf("Could not allocate compressible memory... so waiving execution\n");
            exit(EXIT_WAIVED);
        }
    }

    if (cuMemMap(dptr, size, 0, allocationHandle, 0) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;

    if (cuMemRelease(allocationHandle) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;

    CUmemAccessDesc accessDescriptor;
    accessDescriptor.location.id   = prop.location.id;
    accessDescriptor.location.type = prop.location.type;
    accessDescriptor.flags         = CU_MEM_ACCESS_FLAGS_PROT_READWRITE;

    if (cuMemSetAccess(dptr, size, &accessDescriptor, 1) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;

    *adr = (void *)dptr;
    return cudaSuccess;
}

cudaError_t freeCompressible(void *ptr, size_t size, bool UseCompressibleMemory)
{
    CUmemAllocationProp prop = {};
    cudaError_t         err  = setProp(&prop, UseCompressibleMemory);
    if (err != cudaSuccess)
        return err;

    size_t granularity = 0;
    if (cuMemGetAllocationGranularity(&granularity, &prop, CU_MEM_ALLOC_GRANULARITY_MINIMUM) != CUDA_SUCCESS)
        return cudaErrorMemoryAllocation;
    size = ((size - 1) / granularity + 1) * granularity;

    if (ptr == NULL)
        return cudaSuccess;
    if (cuMemUnmap((CUdeviceptr)ptr, size) != CUDA_SUCCESS || cuMemAddressFree((CUdeviceptr)ptr, size) != CUDA_SUCCESS)
        return cudaErrorInvalidValue;
    return cudaSuccess;
}

```

---
## High-Level Overview
This file is a cpp source file with 3 function(s) in the CUDA Samples repository.

**Dependencies**: 5 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda.h`
- `cuda_runtime_api.h`
- `helper_cuda.h`
- `stdio.h`
- `string.h`

### Functions
#### `cudaError_t setProp(CUmemAllocationProp *prop, bool UseCompressibleMemory)`
- Function in Samples/3_CUDA_Features/cudaCompressibleMemory/compMalloc.cpp

#### `cudaError_t allocateCompressible(void **adr, size_t size, bool UseCompressibleMemory)`
- Function in Samples/3_CUDA_Features/cudaCompressibleMemory/compMalloc.cpp

#### `cudaError_t freeCompressible(void *ptr, size_t size, bool UseCompressibleMemory)`
- Function in Samples/3_CUDA_Features/cudaCompressibleMemory/compMalloc.cpp


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

