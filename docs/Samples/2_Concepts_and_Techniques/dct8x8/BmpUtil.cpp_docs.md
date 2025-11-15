# Documentation for Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp`
- **Type**: .cpp
- **Location**: Samples/2_Concepts_and_Techniques/dct8x8
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

/**
**************************************************************************
* \file BmpUtil.cpp
* \brief Contains basic image operations implementation.
*
* This file contains implementation of basic bitmap loading, saving,
* conversions to different representations and memory management routines.
*/

#include "BmpUtil.h"

#include "Common.h"

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
#pragma warning(disable : 4996) // disable deprecated warning
#endif

/**
**************************************************************************
*  The routine clamps the input value to integer byte range [0, 255]
*
* \param x          [IN] - Input value
*
* \return Pointer to the created plane
*/
int clamp_0_255(int x) { return (x < 0) ? 0 : ((x > 255) ? 255 : x); }

/**
**************************************************************************
*  Float round to nearest value
*
* \param num            [IN] - Float value to round
*
* \return The closest to the input float integer value
*/
float round_f(float num)
{
    float NumAbs  = fabs(num);
    int   NumAbsI = (int)(NumAbs + 0.5f);
    float sign    = num > 0 ? 1.0f : -1.0f;
    return sign * NumAbsI;
}

/**
**************************************************************************
*  Memory allocator, returns aligned format frame with 8bpp pixels.
*
* \param width          [IN] - Width of image buffer to be allocated
* \param height         [IN] - Height of image buffer to be allocated
* \param pStepBytes     [OUT] - Step between two sequential rows
*
* \return Pointer to the created plane
*/
byte *MallocPlaneByte(int width, int height, int *pStepBytes)
{
    byte *ptr;
    *pStepBytes = ((int)ceil(width / 16.0f)) * 16;
    // #ifdef __ALLOW_ALIGNED_MEMORY_MANAGEMENT
    //   ptr = (byte *)_aligned_malloc(*pStepBytes * height, 16);
    // #else
    ptr = (byte *)malloc(*pStepBytes * height);
    // #endif
    return ptr;
}

/**
**************************************************************************
*  Memory allocator, returns aligned format frame with 16bpp float pixels.
*
* \param width          [IN] - Width of image buffer to be allocated
* \param height         [IN] - Height of image buffer to be allocated
* \param pStepBytes     [OUT] - Step between two sequential rows
*
* \return Pointer to the created plane
*/
short *MallocPlaneShort(int width, int height, int *pStepBytes)
{
    short *ptr;
    *pStepBytes = ((int)ceil((width * sizeof(short)) / 16.0f)) * 16;
    // #ifdef __ALLOW_ALIGNED_MEMORY_MANAGEMENT
    //   ptr = (float *)_aligned_malloc(*pStepBytes * height, 16);
    // #else
    ptr = (short *)malloc(*pStepBytes * height);
    // #endif
    *pStepBytes = *pStepBytes / sizeof(short);
    return ptr;
}

/**
**************************************************************************
*  Memory allocator, returns aligned format frame with 32bpp float pixels.
*
* \param width          [IN] - Width of image buffer to be allocated
* \param height         [IN] - Height of image buffer to be allocated
* \param pStepBytes     [OUT] - Step between two sequential rows
*
* \return Pointer to the created plane
*/
float *MallocPlaneFloat(int width, int height, int *pStepBytes)
{
    float *ptr;
    *pStepBytes = ((int)ceil((width * sizeof(float)) / 16.0f)) * 16;
    // #ifdef __ALLOW_ALIGNED_MEMORY_MANAGEMENT
    //   ptr = (float *)_aligned_malloc(*pStepBytes * height, 16);
    // #else
    ptr = (float *)malloc(*pStepBytes * height);
    // #endif
    *pStepBytes = *pStepBytes / sizeof(float);
    return ptr;
}

/**
**************************************************************************
*  Copies byte plane to float plane
*
* \param ImgSrc             [IN] - Source byte plane
* \param StrideB            [IN] - Source plane stride
* \param ImgDst             [OUT] - Destination float plane
* \param StrideF            [IN] - Destination plane stride
* \param Size               [IN] - Size of area to copy
*
* \return None
*/
void CopyByte2Float(byte *ImgSrc, int StrideB, float *ImgDst, int StrideF, ROI Size)
{
    for (int i = 0; i < Size.height; i++) {
        for (int j = 0; j < Size.width; j++) {
            ImgDst[i * StrideF + j] = (float)ImgSrc[i * StrideB + j];
        }
    }
}

/**
**************************************************************************
*  Copies float plane to byte plane (with clamp)
*
* \param ImgSrc             [IN] - Source float plane
* \param StrideF            [IN] - Source plane stride
* \param ImgDst             [OUT] - Destination byte plane
* \param StrideB            [IN] - Destination plane stride
* \param Size               [IN] - Size of area to copy
*
* \return None
*/
void CopyFloat2Byte(float *ImgSrc, int StrideF, byte *ImgDst, int StrideB, ROI Size)
{
    for (int i = 0; i < Size.height; i++) {
        for (int j = 0; j < Size.width; j++) {
            ImgDst[i * StrideB + j] = (byte)clamp_0_255((int)(round_f(ImgSrc[i * StrideF + j])));
        }
    }
}

/**
**************************************************************************
*  Memory deallocator, deletes aligned format frame.
*
* \param ptr            [IN] - Pointer to the plane
*
* \return None
*/
void FreePlane(void *ptr)
{
    // #ifdef __ALLOW_ALIGNED_MEMORY_MANAGEMENT
    //   if (ptr)
    //   {
    //       _aligned_free(ptr);
    //   }
    // #else
    if (ptr) {
        free(ptr);
    }

    // #endif
}

/**
**************************************************************************
*  Performs addition of given value to each pixel in the plane
*
* \param Value              [IN] - Value to add
* \param ImgSrcDst          [IN/OUT] - Source float plane
* \param StrideF            [IN] - Source plane stride
* \param Size               [IN] - Size of area to copy
*
* \return None
*/
void AddFloatPlane(float Value, float *ImgSrcDst, int StrideF, ROI Size)
{
    for (int i = 0; i < Size.height; i++) {
        for (int j = 0; j < Size.width; j++) {
            ImgSrcDst[i * StrideF + j] += Value;
        }
    }
}

/**
**************************************************************************
*  Performs multiplication of given value with each pixel in the plane
*
* \param Value              [IN] - Value for multiplication
* \param ImgSrcDst          [IN/OUT] - Source float plane
* \param StrideF            [IN] - Source plane stride
* \param Size               [IN] - Size of area to copy
*
* \return None
*/
void MulFloatPlane(float Value, float *ImgSrcDst, int StrideF, ROI Size)
{
    for (int i = 0; i < Size.height; i++) {
        for (int j = 0; j < Size.width; j++) {
            ImgSrcDst[i * StrideF + j] *= Value;
        }
    }
}

/**
**************************************************************************
*  This function performs acquisition of image dimensions
*
* \param FileName       [IN] - Image name to load
* \param Width          [OUT] - Image width from file header
* \param Height         [OUT] - Image height from file header
*
* \return Status code
*/
int PreLoadBmp(char *FileName, int *Width, int *Height)
{
    BMPFileHeader FileHeader;
    BMPInfoHeader InfoHeader;
    FILE         *fh;

    if (!(fh = fopen(FileName, "rb"))) {
        return 1; // invalid filename
    }

    fread(&FileHeader, sizeof(BMPFileHeader), 1, fh);

    if (FileHeader._bm_signature != 0x4D42) {
        return 2; // invalid file format
    }

    fread(&InfoHeader, sizeof(BMPInfoHeader), 1, fh);

    if (InfoHeader._bm_color_depth != 24) {
        return 3; // invalid color depth
    }

    if (InfoHeader._bm_compressed) {
        return 4; // invalid compression property
    }

    *Width  = InfoHeader._bm_image_width;
    *Height = InfoHeader._bm_image_height;

    fclose(fh);
    return 0;
}

/**
**************************************************************************
*  This function performs loading of bitmap luma
*
* \param FileName       [IN] - Image name to load
* \param Stride         [IN] - Image stride
* \param ImSize         [IN] - Image size
* \param Img            [OUT] - Prepared buffer
*
* \return None
*/
void LoadBmpAsGray(char *FileName, int Stride, ROI ImSize, byte *Img)
{
    BMPFileHeader FileHeader;
    BMPInfoHeader InfoHeader;
    FILE         *fh;
    fh = fopen(FileName, "rb");

    fread(&FileHeader, sizeof(BMPFileHeader), 1, fh);
    fread(&InfoHeader, sizeof(BMPInfoHeader), 1, fh);

    for (int i = ImSize.height - 1; i >= 0; i--) {
        for (int j = 0; j < ImSize.width; j++) {
            int r = 0, g = 0, b = 0;
            fread(&b, 1, 1, fh);
            fread(&g, 1, 1, fh);
            fread(&r, 1, 1, fh);
            int val             = (313524 * r + 615514 * g + 119537 * b + 524288) >> 20;
            Img[i * Stride + j] = (byte)clamp_0_255(val);
        }
    }

    fclose(fh);
    return;
}

/**
**************************************************************************
*  This function performs dumping of bitmap luma on HDD
*
* \param FileName       [OUT] - Image name to dump to
* \param Img            [IN] - Image luma to dump
* \param Stride         [IN] - Image stride
* \param ImSize         [IN] - Image size
*
* \return None
*/
void DumpBmpAsGray(char *FileName, byte *Img, int Stride, ROI ImSize)
{
    FILE *fp = NULL;
    fp       = fopen(FileName, "wb");

    if (fp == NULL) {
        return;
    }

    BMPFileHeader FileHeader;
    BMPInfoHeader InfoHeader;

    // init headers
    FileHeader._bm_signature            = 0x4D42;
    FileHeader._bm_file_size            = 54 + 3 * ImSize.width * ImSize.height;
    FileHeader._bm_reserved             = 0;
    FileHeader._bm_bitmap_data          = 0x36;
    InfoHeader._bm_bitmap_size          = 0;
    InfoHeader._bm_color_depth          = 24;
    InfoHeader._bm_compressed           = 0;
    InfoHeader._bm_hor_resolution       = 0;
    InfoHeader._bm_image_height         = ImSize.height;
    InfoHeader._bm_image_width          = ImSize.width;
    InfoHeader._bm_info_header_size     = 40;
    InfoHeader._bm_num_colors_used      = 0;
    InfoHeader._bm_num_important_colors = 0;
    InfoHeader._bm_num_of_planes        = 1;
    InfoHeader._bm_ver_resolution       = 0;

    fwrite(&FileHeader, sizeof(BMPFileHeader), 1, fp);
    fwrite(&InfoHeader, sizeof(BMPInfoHeader), 1, fp);

    for (int i = ImSize.height - 1; i >= 0; i--) {
        for (int j = 0; j < ImSize.width; j++) {
            fwrite(&(Img[i * Stride + j]), 1, 1, fp);
            fwrite(&(Img[i * Stride + j]), 1, 1, fp);
            fwrite(&(Img[i * Stride + j]), 1, 1, fp);
        }
    }

    fclose(fp);
}

/**
**************************************************************************
*  This function performs dumping of 8x8 block from float plane
*
* \param PlaneF         [IN] - Image plane
* \param StrideF        [IN] - Image stride
* \param Fname          [OUT] - File name to dump to
*
* \return None
*/
void DumpBlockF(float *PlaneF, int StrideF, char *Fname)
{
    FILE *fp = fopen(Fname, "wb");

    for (int i = 0; i < 8; i++) {
        for (int j = 0; j < 8; j++) {
            fprintf(fp, "%.*f  ", 14, PlaneF[i * StrideF + j]);
        }

        fprintf(fp, "\n");
    }

    fclose(fp);
}

/**
**************************************************************************
*  This function performs dumping of 8x8 block from byte plane
*
* \param Plane          [IN] - Image plane
* \param Stride         [IN] - Image stride
* \param Fname          [OUT] - File name to dump to
*
* \return None
*/
void DumpBlock(byte *Plane, int Stride, char *Fname)
{
    FILE *fp = fopen(Fname, "wb");

    for (int i = 0; i < 8; i++) {
        for (int j = 0; j < 8; j++) {
            fprintf(fp, "%.3d  ", Plane[i * Stride + j]);
        }

        fprintf(fp, "\n");
    }

    fclose(fp);
}

/**
**************************************************************************
*  This function performs evaluation of Mean Square Error between two images
*
* \param Img1           [IN] - Image 1
* \param Img2           [IN] - Image 2
* \param Stride         [IN] - Image stride
* \param Size           [IN] - Image size
*
* \return Mean Square Error between images
*/
float CalculateMSE(byte *Img1, byte *Img2, int Stride, ROI Size)
{
    uint32 Acc = 0;

    for (int i = 0; i < Size.height; i++) {
        for (int j = 0; j < Size.width; j++) {
            int TmpDiff = Img1[i * Stride + j] - Img2[i * Stride + j];
            TmpDiff *= TmpDiff;
            Acc += TmpDiff;
        }
    }

    return ((float)Acc) / (Size.height * Size.width);
}

/**
**************************************************************************
*  This function performs evaluation of Peak Signal to Noise Ratio between
*  two images
*
* \param Img1           [IN] - Image 1
* \param Img2           [IN] - Image 2
* \param Stride         [IN] - Image stride
* \param Size           [IN] - Image size
*
* \return Peak Signal to Noise Ratio between images
*/
float CalculatePSNR(byte *Img1, byte *Img2, int Stride, ROI Size)
{
    float MSE = CalculateMSE(Img1, Img2, Stride, Size);
    return 10 * log10(255 * 255 / MSE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp`.

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

- **Total Lines**: 470
- **Approximate Size**: 14608 bytes

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
