# Documentation for Common/UtilNPP/SignalAllocatorsNPP.h

## File Metadata

- **Path**: `Common/UtilNPP/SignalAllocatorsNPP.h`
- **Type**: .h
- **Location**: Common/UtilNPP
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

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


#ifndef NV_UTIL_NPP_SIGNAL_ALLOCATORS_NPP_H
#define NV_UTIL_NPP_SIGNAL_ALLOCATORS_NPP_H


#include "Exceptions.h"

#include <npps.h>
#include <cuda_runtime.h>

namespace npp
{

    template <typename D>
    class SignalAllocator
    {
    };

    template<>
    class SignalAllocator<Npp8u>
    {
        public:
            static
            Npp8u *
            Malloc1D(size_t nSize)
            {
                Npp8u *pResult = nppsMalloc_8u(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp8u *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp8u *pDst, const Npp8u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp8u),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp8u *pDst, const Npp8u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp8u), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp8u *pDst, const Npp8u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp8u), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp16s>
    {
        public:
            static
            Npp16s *
            Malloc1D(size_t nSize)
            {
                Npp16s *pResult = nppsMalloc_16s(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp16s *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp16s *pDst, const Npp16s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16s),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp16s *pDst, const Npp16s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16s), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp16s *pDst, const Npp16s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16s), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp16u>
    {
        public:
            static
            Npp16u *
            Malloc1D(size_t nSize)
            {
                Npp16u *pResult = nppsMalloc_16u(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp16u *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp16u *pDst, const Npp16u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16u),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp16u *pDst, const Npp16u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16u), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp16u *pDst, const Npp16u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16u), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp16sc>
    {
        public:
            static
            Npp16sc *
            Malloc1D(size_t nSize)
            {
                Npp16sc *pResult = nppsMalloc_16sc(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp16sc *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp16sc *pDst, const Npp16sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16sc),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp16sc *pDst, const Npp16sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16sc), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp16sc *pDst, const Npp16sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp16sc), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp32u>
    {
        public:
            static
            Npp32u *
            Malloc1D(size_t nSize)
            {
                Npp32u *pResult = nppsMalloc_32u(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp32u *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp32u *pDst, const Npp32u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32u),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp32u *pDst, const Npp32u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32u), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp32u *pDst, const Npp32u *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32u), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp32s>
    {
        public:
            static
            Npp32s *
            Malloc1D(size_t nSize)
            {
                Npp32s *pResult = nppsMalloc_32s(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp32s *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp32s *pDst, const Npp32s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32s),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp32s *pDst, const Npp32s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32s), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp32s *pDst, const Npp32s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32s), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp32sc>
    {
        public:
            static
            Npp32sc *
            Malloc1D(size_t nSize)
            {
                Npp32sc *pResult = nppsMalloc_32sc(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp32sc *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp32sc *pDst, const Npp32sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32sc),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp32sc *pDst, const Npp32sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32sc), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp32sc *pDst, const Npp32sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32sc), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp32f>
    {
        public:
            static
            Npp32f *
            Malloc1D(size_t nSize)
            {
                Npp32f *pResult = nppsMalloc_32f(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp32f *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp32f *pDst, const Npp32f *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32f),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp32f *pDst, const Npp32f *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32f), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp32f *pDst, const Npp32f *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32f), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp32fc>
    {
        public:
            static
            Npp32fc *
            Malloc1D(size_t nSize)
            {
                Npp32fc *pResult = nppsMalloc_32fc(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp32fc *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp32fc *pDst, const Npp32fc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32fc),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp32fc *pDst, const Npp32fc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32fc), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp32fc *pDst, const Npp32fc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp32fc), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp64s>
    {
        public:
            static
            Npp64s *
            Malloc1D(size_t nSize)
            {
                Npp64s *pResult = nppsMalloc_64s(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp64s *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp64s *pDst, const Npp64s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64s),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp64s *pDst, const Npp64s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64s), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp64s *pDst, const Npp64s *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64s), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp64sc>
    {
        public:
            static
            Npp64sc *
            Malloc1D(size_t nSize)
            {
                Npp64sc *pResult = nppsMalloc_64sc(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp64sc *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp64sc *pDst, const Npp64sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64sc),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp64sc *pDst, const Npp64sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64sc), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp64sc *pDst, const Npp64sc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64sc), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp64f>
    {
        public:
            static
            Npp64f *
            Malloc1D(size_t nSize)
            {
                Npp64f *pResult = nppsMalloc_64f(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp64f *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp64f *pDst, const Npp64f *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64f),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp64f *pDst, const Npp64f *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64f), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp64f *pDst, const Npp64f *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64f), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };

    template<>
    class SignalAllocator<Npp64fc>
    {
        public:
            static
            Npp64fc *
            Malloc1D(size_t nSize)
            {
                Npp64fc *pResult = nppsMalloc_64fc(static_cast<int>(nSize));
                NPP_ASSERT(pResult != 0);

                return pResult;
            };

            static
            void
            Free1D(Npp64fc *pValues)
            {
                nppsFree(pValues);
            };

            static
            void
            Copy1D(Npp64fc *pDst, const Npp64fc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64fc),cudaMemcpyDeviceToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            HostToDeviceCopy1D(Npp64fc *pDst, const Npp64fc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64fc), cudaMemcpyHostToDevice);
                NPP_ASSERT(cudaSuccess == eResult);
            };

            static
            void
            DeviceToHostCopy1D(Npp64fc *pDst, const Npp64fc *pSrc, size_t nSize)
            {
                cudaError_t eResult;
                eResult = cudaMemcpy(pDst, pSrc, nSize * sizeof(Npp64fc), cudaMemcpyDeviceToHost);
                NPP_ASSERT(cudaSuccess == eResult);
            };
    };
} // npp namespace

#endif // NV_UTIL_NPP_SIGNAL_ALLOCATORS_NPP_H

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Common/UtilNPP/SignalAllocatorsNPP.h`.

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

- **Total Lines**: 685
- **Approximate Size**: 20843 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

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

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

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
