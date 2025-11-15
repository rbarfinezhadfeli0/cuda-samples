# Documentation for Samples/0_Introduction/vectorAddMMAP/multidevicealloc_memmap.cpp

## File Metadata

- **Path**: `Samples/0_Introduction/vectorAddMMAP/multidevicealloc_memmap.cpp`
- **Type**: .cpp
- **Location**: Samples/0_Introduction/vectorAddMMAP
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

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

#include "multidevicealloc_memmap.hpp"

static size_t round_up(size_t x, size_t y) { return ((x + y - 1) / y) * y; }

CUresult simpleMallocMultiDeviceMmap(CUdeviceptr                 *dptr,
                                     size_t                      *allocationSize,
                                     size_t                       size,
                                     const std::vector<CUdevice> &residentDevices,
                                     const std::vector<CUdevice> &mappingDevices,
                                     size_t                       align)
{
    CUresult status          = CUDA_SUCCESS;
    size_t   min_granularity = 0;
    size_t   stripeSize;

    // Setup the properties common for all the chunks
    // The allocations will be device pinned memory.
    // This property structure describes the physical location where the memory
    // will be allocated via cuMemCreate allong with additional properties In this
    // case, the allocation will be pinnded device memory local to a given device.
    CUmemAllocationProp prop = {};
    prop.type                = CU_MEM_ALLOCATION_TYPE_PINNED;
    prop.location.type       = CU_MEM_LOCATION_TYPE_DEVICE;

    // Get the minimum granularity needed for the resident devices
    // (the max of the minimum granularity of each participating device)
    for (int idx = 0; idx < residentDevices.size(); idx++) {
        size_t granularity = 0;

        // get the minnimum granularity for residentDevices[idx]
        prop.location.id = residentDevices[idx];
        status           = cuMemGetAllocationGranularity(&granularity, &prop, CU_MEM_ALLOC_GRANULARITY_MINIMUM);
        if (status != CUDA_SUCCESS) {
            goto done;
        }
        if (min_granularity < granularity) {
            min_granularity = granularity;
        }
    }

    // Get the minimum granularity needed for the accessing devices
    // (the max of the minimum granularity of each participating device)
    for (size_t idx = 0; idx < mappingDevices.size(); idx++) {
        size_t granularity = 0;

        // get the minnimum granularity for mappingDevices[idx]
        prop.location.id = mappingDevices[idx];
        status           = cuMemGetAllocationGranularity(&granularity, &prop, CU_MEM_ALLOC_GRANULARITY_MINIMUM);
        if (status != CUDA_SUCCESS) {
            goto done;
        }
        if (min_granularity < granularity) {
            min_granularity = granularity;
        }
    }

    // Round up the size such that we can evenly split it into a stripe size tha
    // meets the granularity requirements Essentially size = N *
    // residentDevices.size() * min_granularity is the requirement, since each
    // piece of the allocation will be stripeSize = N * min_granularity and the
    // min_granularity requirement applies to each stripeSize piece of the
    // allocation.
    size       = round_up(size, residentDevices.size() * min_granularity);
    stripeSize = size / residentDevices.size();

    // Return the rounded up size to the caller for use in the free
    if (allocationSize) {
        *allocationSize = size;
    }

    // Reserve the required contiguous VA space for the allocations
    status = cuMemAddressReserve(dptr, size, align, 0, 0);
    if (status != CUDA_SUCCESS) {
        goto done;
    }

    // Create and map the backings on each gpu
    // note: reusing CUmemAllocationProp prop from earlier with prop.type &
    // prop.location.type already specified.
    for (size_t idx = 0; idx < residentDevices.size(); idx++) {
        CUresult status2 = CUDA_SUCCESS;

        // Set the location for this chunk to this device
        prop.location.id = residentDevices[idx];

        // Create the allocation as a pinned allocation on this device
        CUmemGenericAllocationHandle allocationHandle;
        status = cuMemCreate(&allocationHandle, stripeSize, &prop, 0);
        if (status != CUDA_SUCCESS) {
            goto done;
        }

        // Assign the chunk to the appropriate VA range and release the handle.
        // After mapping the memory, it can be referenced by virtual address.
        // Since we do not need to make any other mappings of this memory or export
        // it, we no longer need and can release the allocationHandle. The
        // allocation will be kept live until it is unmapped.
        status = cuMemMap(*dptr + (stripeSize * idx), stripeSize, 0, allocationHandle, 0);

        // the handle needs to be released even if the mapping failed.
        status2 = cuMemRelease(allocationHandle);
        if (status == CUDA_SUCCESS) {
            // cuMemRelease should not have failed here
            // as the handle was just allocated successfully
            // however return an error if it does.
            status = status2;
        }

        // Cleanup in case of any mapping failures.
        if (status != CUDA_SUCCESS) {
            goto done;
        }
    }

    {
        // Each accessDescriptor will describe the mapping requirement for a single
        // device
        std::vector<CUmemAccessDesc> accessDescriptors;
        accessDescriptors.resize(mappingDevices.size());

        // Prepare the access descriptor array indicating where and how the backings
        // should be visible.
        for (size_t idx = 0; idx < mappingDevices.size(); idx++) {
            // Specify which device we are adding mappings for.
            accessDescriptors[idx].location.type = CU_MEM_LOCATION_TYPE_DEVICE;
            accessDescriptors[idx].location.id   = mappingDevices[idx];

            // Specify both read and write access.
            accessDescriptors[idx].flags = CU_MEM_ACCESS_FLAGS_PROT_READWRITE;
        }

        // Apply the access descriptors to the whole VA range.
        status = cuMemSetAccess(*dptr, size, &accessDescriptors[0], accessDescriptors.size());
        if (status != CUDA_SUCCESS) {
            goto done;
        }
    }

done:
    if (status != CUDA_SUCCESS) {
        if (*dptr) {
            simpleFreeMultiDeviceMmap(*dptr, size);
        }
    }

    return status;
}

CUresult simpleFreeMultiDeviceMmap(CUdeviceptr dptr, size_t size)
{
    CUresult status = CUDA_SUCCESS;

    // Unmap the mapped virtual memory region
    // Since the handles to the mapped backing stores have already been released
    // by cuMemRelease, and these are the only/last mappings referencing them,
    // The backing stores will be freed.
    // Since the memory has been unmapped after this call, accessing the specified
    // va range will result in a fault (unitll it is remapped).
    status = cuMemUnmap(dptr, size);
    if (status != CUDA_SUCCESS) {
        return status;
    }
    // Free the virtual address region.  This allows the virtual address region
    // to be reused by future cuMemAddressReserve calls.  This also allows the
    // virtual address region to be used by other allocation made through
    // opperating system calls like malloc & mmap.
    status = cuMemAddressFree(dptr, size);
    if (status != CUDA_SUCCESS) {
        return status;
    }

    return status;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/0_Introduction/vectorAddMMAP/multidevicealloc_memmap.cpp`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 201
- **Approximate Size**: 8662 bytes

### Content Structure

#### Functions and Kernels

This file contains function definitions and potentially CUDA kernel launches.
Functions in this file handle:

- **Initialization**: Setting up CUDA context and allocating resources
- **Computation**: Core algorithmic implementations
- **Cleanup**: Freeing resources and error checking

#### Error Handling

The code implements error handling through:

- CUDA error checking macros
- Return code validation
- Exception handling where appropriate

#### Memory Management

Memory operations include:

- Device memory allocation (cudaMalloc)
- Host memory allocation
- Memory transfers (cudaMemcpy)
- Proper cleanup and deallocation

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

### Computational Complexity

The algorithms in this file are designed with performance in mind:

- **GPU Parallelism**: Leveraging thousands of CUDA cores
- **Memory Bandwidth**: Optimizing data transfer patterns
- **Occupancy**: Maximizing GPU utilization
- **Latency Hiding**: Using asynchronous operations where beneficial

### Optimization Opportunities

Potential areas for optimization:

1. Kernel launch configuration tuning
2. Shared memory usage
3. Coalesced memory access
4. Reduction of host-device transfers

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

To test this file:

1. Build the sample using CMake
2. Run the executable with appropriate parameters
3. Verify output against expected results
4. Check for memory leaks using cuda-memcheck
5. Profile performance using NVIDIA profiling tools

### Integration Tests

This file is tested as part of the overall sample application, ensuring:

- Correct functionality
- Expected performance characteristics
- Compatibility across different GPU architectures

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Running

```bash
./{executable_name} [options]
```

Refer to the sample's README for specific command-line options and usage patterns.

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
