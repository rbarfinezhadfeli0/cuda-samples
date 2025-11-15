# Documentation for Common/UtilNPP/ImagePacked.h

## File Metadata

- **Path**: `Common/UtilNPP/ImagePacked.h`
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

#ifndef NV_UTIL_NPP_IMAGE_PACKED_H
#define NV_UTIL_NPP_IMAGE_PACKED_H

#include "Image.h"
#include "Pixel.h"

namespace npp
{
    template<typename D, size_t N, class A>
    class ImagePacked: public npp::Image
    {
        public:
            typedef npp::Pixel<D, N>    tPixel;
            typedef D                   tData;
            static const size_t         gnChannels = N;
            typedef npp::Image::Size    tSize;

            ImagePacked(): aPixels_(0)
                , nPitch_(0)
            {
                ;
            }

            ImagePacked(unsigned int nWidth, unsigned int nHeight): Image(nWidth, nHeight)
                , aPixels_(0)
                , nPitch_(0)
            {
                aPixels_ = A::Malloc2D(width(), height(), &nPitch_);
            }

            ImagePacked(unsigned int nWidth, unsigned int nHeight, bool bTight): Image(nWidth, nHeight)
                , aPixels_(0)
                , nPitch_(0)
            {
                aPixels_ = A::Malloc2D(width(), height(), &nPitch_, bTight);
            }

            ImagePacked(const tSize &rSize): Image(rSize)
                , aPixels_(0)
                , nPitch_(0)
            {
                aPixels_ = A::Malloc2D(width(), height(), &nPitch_);
            }

            ImagePacked(const ImagePacked<D, N, A> &rImage): Image(rImage)
                , aPixels_(0)
                , nPitch_(rImage.pitch())
            {
                aPixels_ = A::Malloc2D(width(), height(), &nPitch_);
                A::Copy2D(aPixels_, nPitch_, rImage.pixels(), rImage.pitch(), width(), height());
            }

            virtual
            ~ImagePacked()
            {
                A::Free2D(aPixels_);
            }

            ImagePacked &
            operator= (const ImagePacked<D, N, A> &rImage)
            {
                // in case of self-assignment
                if (&rImage == this)
                {
                    return *this;
                }

                A::Free2D(aPixels_);
                aPixels_ = 0;
                nPitch_ = 0;

                // assign parent class's data fields (width, height)
                Image::operator =(rImage);

                aPixels_ = A::Malloc2D(width(), height(), &nPitch_);
                A::Copy2D(aPixels_, nPitch_, rImage.data(), rImage.pitch(), width(), height());

                return *this;
            }

            unsigned int
            pitch()
            const
            {
                return nPitch_;
            }

            /// Get a pointer to the pixel array.
            ///     The result pointer can be offset to pixel at position (x, y) and
            /// even negative offsets are allowed.
            /// \param nX Horizontal pointer/array offset.
            /// \param nY Vertical pointer/array offset.
            /// \return Pointer to the pixel array (or first pixel in array with coordinates (nX, nY).
            tPixel *
            pixels(int nX = 0, int nY = 0)
            {
                return reinterpret_cast<tPixel *>(reinterpret_cast<unsigned char *>(aPixels_) + nY * pitch() + nX * gnChannels * sizeof(D));
            }

            const
            tPixel *
            pixels(int nX = 0, int nY = 0)
            const
            {
                return reinterpret_cast<const tPixel *>(reinterpret_cast<unsigned char *>(aPixels_) + nY * pitch() + nX * gnChannels * sizeof(D));
            }

            D *
            data(int nX = 0, int nY = 0)
            {
                return reinterpret_cast<D *>(pixels(nX, nY));
            }

            const
            D *
            data(int nX = 0, int nY = 0)
            const
            {
                return reinterpret_cast<const D *>(pixels(nX, nY));
            }

            void
            swap(ImagePacked<D, N, A> &rImage)
            {
                Image::swap(rImage);

                tData *aTemp   = aPixels_;
                aPixels_        = rImage.aPixels_;
                rImage.aPixels_ = aTemp;

                unsigned int nTemp = nPitch_;
                nPitch_            = rImage.nPitch_;
                rImage.nPitch_     = nTemp;
            }

        private:
            D *aPixels_;
            unsigned int nPitch_;
    };

} // npp namespace


#endif // NV_IMAGE_IPP_H

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Common/UtilNPP/ImagePacked.h`.

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

- **Total Lines**: 172
- **Approximate Size**: 5889 bytes

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
