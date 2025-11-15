# Documentation for Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/sample.cfg

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/sample.cfg`
- **Type**: .cfg
- **Location**: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia
- **Binary**: No

## Purpose and Role

This is a .cfg file in the repository.

## Original Source Content

```cfg
###################################################################################################
#
# Copyright (c) 2022, NVIDIA CORPORATION.  All Rights Reserved.
#
# NVIDIA CORPORATION and its licensors retain all intellectual property
# and proprietary rights in and to this software, related documentation
# and any modifications thereto.  Any use, reproduction, disclosure or
# distribution of this software and related documentation without an express
# license agreement from NVIDIA CORPORATION is strictly prohibited.
#
# 2D configuration file for nvimg_2d application.
#
# Plese see nvmedia_2d.h for detailed information about parameters and datatypes
#
###################################################################################################


###################################################################################################
# Top level application parameters
#
# inputFile name limited to 1024 chars
inputfile = "teapot.rgba"


# The following bit-masks can be ORed:
# 1 => If the user wants to enable Filtering mode
# 2 => Reserved for future use
# 4 => If the user wants to enable Transformation mode
# 8 => If the user wants to enable color space conversion standard
validOperations = 0

# Transformation Mode for Blit2D
# 0 => IDENTITY
# 1 => ROTATE_90
# 2 => ROTATE_180
# 3 => ROTATE_270
# 4 => FLIP_HORIZONTAL
# 5 => INV_TRANSPOSE
# 6 => FLIP_VERTICAL
# 7 => TRANSPOSE
transformMode = 0

# Filtering mode for Blit2D
# 1 => FILTER_OFF
# 2 => FILTER_LOW
# 3 => FILTER_MEDIUM
# 4 => FILTER_HIGH
filterMode = 1

# color space conversion standard
# 0 => ITU BT.601
# 1 => ITU BT.709
# 2 => SMTE 240M
# 3 => BT.601 Extended Range
colorStd = 0

####################### source image properties##########################
srcWidth = 1024
srcHeight = 1024
# 1 => Uncached (mapped) access type flag
# 2 => Cached (mapped) access type flag
# 3 => Unmapped access type flag
srcCPUAccess = 3
# Allocation type
# 0 => none
# 1 => isochronous
srcAllocType = 1
# Surface scan type
# Only progressive scan is supported
# 1 => Progressive
srcScanType = 1
# Color Standard type
# 1 => sRGB
# 2 => YCbCr Rec.601 (Studio Range)
# 3 => YCbCr Rec.601 (Extended Range)
# 4 => YCbCr Rec.709 (Studio Range)
# 5 => YCbCr Rec.709 (Extended Range)
# 11 => Sensor RGBA
srcColorStd = 1

# 1 => YUV
# 2 => RGBA
# 3 => RAW
srcSurfType = 2
# 1 => Block Linear
# 2 => Pitch Linear
srcLayout = 2
# 1 => uint
# 2 => int
srcDataType = 1
# 1 => planar
# 2 => Semi Planar
# 3 => Packed
srcMemory = 3
# Sub-Sampling type of the input surface
# 1 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_420
# 2 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_422
# 3 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_444
# 4 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_422R
# 0 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_NONE
srcSubSamplingType = 0
# Bits per component of the input surface
# 1 => NVM_SURF_ATTR_BITS_PER_COMPONENT_8
# 2 => NVM_SURF_ATTR_BITS_PER_COMPONENT_10
# 3 => NVM_SURF_ATTR_BITS_PER_COMPONENT_12
# 5 => NVM_SURF_ATTR_BITS_PER_COMPONENT_16
srcBitsPerComponent =  1
# 1 => NVM_SURF_ATTR_COMPONENT_ORDER_LUMA
# 2 => NVM_SURF_ATTR_COMPONENT_ORDER_YUV
# 3 => NVM_SURF_ATTR_COMPONENT_ORDER_YVU
# 4 => NVM_SURF_ATTR_COMPONENT_ORDER_YUYV
# 5 => NVM_SURF_ATTR_COMPONENT_ORDER_YVYU
# 6 => NVM_SURF_ATTR_COMPONENT_ORDER_VYUY
# 10 => NVM_SURF_ATTR_COMPONENT_ORDER_VUYX
# 18 => NVM_SURF_ATTR_COMPONENT_ORDER_RGBA
# 20 => NVM_SURF_ATTR_COMPONENT_ORDER_BGRA
srcComponentOrder = 18

# srcRect Structure containing co-ordinates of the rectangle in the source image.
# Left X co-ordinate.
srcRectx0 = 0
# Top Y co-ordinate.
srcRecty0 =  0
# Right X co-ordinate.
srcRectx1 =  1024
# Bottom Y co-ordinate.
srcRecty1 =  1024

####################### output image properties##########################
dstWidth = 1024
dstHeight = 1024
# 1 => Uncached (mapped) access type flag
# 2 => Cached (mapped) access type flag
# 3 => Unmapped access type flag
dstCPUAccess = 3
# Allocation type
# 0 => none
# 1 => isochronous
dstAllocType = 1
# Surface scan type
# Only progressive scan is supported
# 1 => Progressive
dstScanType = 1
# Color Standard type
# 1 => sRGB
# 2 => YCbCr Rec.601 (Studio Range)
# 3 => YCbCr Rec.601 (Extended Range)
# 4 => YCbCr Rec.709 (Studio Range)
# 5 => YCbCr Rec.709 (Extended Range)
# 11 => Sensor RGBA
dstColorStd = 2

# 1 => YUV
# 2 => RGBA
# 3 => RAW
dstSurfType = 1
# 1 => Block Linear
# 2 => Pitch Linear
dstLayout = 1
# 1 => uint
# 2 => int
dstDataType = 1
# 1 => planar
# 2 => Semi Planar
# 3 => Packed
dstMemory = 1
# Sub-Sampling type of the output surface
# 1 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_420
# 2 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_422
# 3 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_444
# 4 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_422R
# 0 => NVM_SURF_ATTR_SUB_SAMPLING_TYPE_NONE
dstSubSamplingType = 1
# Bits per component of the output surface
# 1 => NVM_SURF_ATTR_BITS_PER_COMPONENT_8
# 2 => NVM_SURF_ATTR_BITS_PER_COMPONENT_10
# 3 => NVM_SURF_ATTR_BITS_PER_COMPONENT_12
# 5 => NVM_SURF_ATTR_BITS_PER_COMPONENT_16
dstBitsPerComponent = 1
# 1 => NVM_SURF_ATTR_COMPONENT_ORDER_LUMA
# 2 => NVM_SURF_ATTR_COMPONENT_ORDER_YUV
# 3 => NVM_SURF_ATTR_COMPONENT_ORDER_YVU
# 4 => NVM_SURF_ATTR_COMPONENT_ORDER_YUYV
# 5 => NVM_SURF_ATTR_COMPONENT_ORDER_YVYU
# 6 => NVM_SURF_ATTR_COMPONENT_ORDER_VYUY
# 10 => NVM_SURF_ATTR_COMPONENT_ORDER_VUYX
# 18 => NVM_SURF_ATTR_COMPONENT_ORDER_RGBA
# 20 => NVM_SURF_ATTR_COMPONENT_ORDER_BGRA
dstComponentOrder = 2
# dstRect Structure containing co-ordinates of the rectangle in the destination image.
# Left X co-ordinate.
dstRectx0 = 0
# Top Y co-ordinate.
dstRecty0 = 0
# Right X co-ordinate.
dstRectx1 = 1024
# Bottom Y co-ordinate.
dstRecty1 = 1024

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/sample.cfg`.

## Detailed Analysis

### File Statistics

- **Total Lines**: 199
- **Approximate Size**: 5647 bytes

### Content Structure

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

This file type generally has minimal direct security implications.

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
