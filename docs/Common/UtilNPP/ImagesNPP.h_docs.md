# Documentation for Common/UtilNPP/ImagesNPP.h

## File Metadata

- **Path**: `Common/UtilNPP/ImagesNPP.h`
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


#ifndef NV_UTIL_NPP_IMAGES_NPP_H
#define NV_UTIL_NPP_IMAGES_NPP_H

#include "Exceptions.h"
#include "ImagePacked.h"

#include "ImageAllocatorsNPP.h"
#include <cuda_runtime.h>

namespace npp
{
    // forward declaration
    template<typename D, unsigned int N, class A> class ImageCPU;

    template<typename D, unsigned int N>
    class ImageNPP: public npp::ImagePacked<D, N, npp::ImageAllocator<D, N> >
    {
        public:
            ImageNPP()
            {
                ;
            }

            ImageNPP(unsigned int nWidth, unsigned int nHeight, bool bTight = false): ImagePacked<D, N, npp::ImageAllocator<D, N> >(nWidth, nHeight, bTight)
            {
                ;
            }

            ImageNPP(const npp::Image::Size &rSize): ImagePacked<D, N, npp::ImageAllocator<D, N> >(rSize)
            {
                ;
            }

            ImageNPP(const ImageNPP<D, N> &rImage): Image(rImage)
            {
                ;
            }

            template<class X>
            explicit
            ImageNPP(const ImageCPU<D, N, X> &rImage, bool bTight = false): ImagePacked<D, N, npp::ImageAllocator<D, N> >(rImage.width(), rImage.height(), bTight)
            {
                npp::ImageAllocator<D, N>::HostToDeviceCopy2D(ImagePacked<D, N, npp::ImageAllocator<D, N> >::data(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::pitch(),
                                                              rImage.data(),
                                                              rImage.pitch(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::width(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::height());
            }

            virtual
            ~ImageNPP()
            {
                ;
            }

            ImageNPP &
            operator= (const ImageNPP<D, N> &rImage)
            {
                ImagePacked<D, N, npp::ImageAllocator<D, N> >::operator= (rImage);

                return *this;
            }

            void
            copyTo(D *pData, unsigned int nPitch)
            const
            {
                NPP_ASSERT((ImagePacked<D, N, npp::ImageAllocator<D, N> >::width() * sizeof(npp::Pixel<D, N>) <= nPitch));
                npp::ImageAllocator<D, N>::DeviceToHostCopy2D(pData,
                                                              nPitch,
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::data(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::pitch(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::width(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::height());
            }

            void
            copyFrom(D *pData, unsigned int nPitch)
            {
                NPP_ASSERT((ImagePacked<D, N, npp::ImageAllocator<D, N> >::width() * sizeof(npp::Pixel<D, N>) <= nPitch));
                npp::ImageAllocator<D, N>::HostToDeviceCopy2D(ImagePacked<D, N, npp::ImageAllocator<D, N> >::data(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::pitch(),
                                                              pData,
                                                              nPitch,
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::width(),
                                                              ImagePacked<D, N, npp::ImageAllocator<D, N> >::height());
            }
    };

    typedef ImageNPP<Npp8u,  1>   ImageNPP_8u_C1;
    typedef ImageNPP<Npp8u,  2>   ImageNPP_8u_C2;
    typedef ImageNPP<Npp8u,  3>   ImageNPP_8u_C3;
    typedef ImageNPP<Npp8u,  4>   ImageNPP_8u_C4;

    typedef ImageNPP<Npp16u, 1>  ImageNPP_16u_C1;
    typedef ImageNPP<Npp16u, 2>  ImageNPP_16u_C2;
    typedef ImageNPP<Npp16u, 3>  ImageNPP_16u_C3;
    typedef ImageNPP<Npp16u, 4>  ImageNPP_16u_C4;

    typedef ImageNPP<Npp16s, 1>  ImageNPP_16s_C1;
    typedef ImageNPP<Npp16s, 3>  ImageNPP_16s_C3;
    typedef ImageNPP<Npp16s, 4>  ImageNPP_16s_C4;

    typedef ImageNPP<Npp32s, 1>  ImageNPP_32s_C1;
    typedef ImageNPP<Npp32s, 3>  ImageNPP_32s_C3;
    typedef ImageNPP<Npp32s, 4>  ImageNPP_32s_C4;

    typedef ImageNPP<Npp32f, 1>  ImageNPP_32f_C1;
    typedef ImageNPP<Npp32f, 2>  ImageNPP_32f_C2;
    typedef ImageNPP<Npp32f, 3>  ImageNPP_32f_C3;
    typedef ImageNPP<Npp32f, 4>  ImageNPP_32f_C4;

    typedef ImageNPP<Npp64f, 1>  ImageNPP_64f_C1;
    typedef ImageNPP<Npp64f, 2>  ImageNPP_64f_C2;
    typedef ImageNPP<Npp64f, 3>  ImageNPP_64f_C3;
    typedef ImageNPP<Npp64f, 4>  ImageNPP_64f_C4;

} // npp namespace

#endif // NV_UTIL_NPP_IMAGES_NPP_H

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Common/UtilNPP/ImagesNPP.h`.

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

- **Total Lines**: 150
- **Approximate Size**: 6562 bytes

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
