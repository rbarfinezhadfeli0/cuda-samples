# Documentation for Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h`
- **Type**: .h
- **Location**: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop
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

//
// DESCRIPTION:   Common EGL stream functions header file
//

#ifndef _EGLSTRM_COMMON_H_
#define _EGLSTRM_COMMON_H_

#include <signal.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <unistd.h>

#include "cuda.h"
#include "cudaEGL.h"
#include "helper_cuda_drvapi.h"

#define EXTENSION_LIST(T)                                                                  \
    T(PFNEGLCREATESTREAMKHRPROC, eglCreateStreamKHR)                                       \
    T(PFNEGLDESTROYSTREAMKHRPROC, eglDestroyStreamKHR)                                     \
    T(PFNEGLQUERYSTREAMKHRPROC, eglQueryStreamKHR)                                         \
    T(PFNEGLQUERYSTREAMU64KHRPROC, eglQueryStreamu64KHR)                                   \
    T(PFNEGLQUERYSTREAMTIMEKHRPROC, eglQueryStreamTimeKHR)                                 \
    T(PFNEGLSTREAMATTRIBKHRPROC, eglStreamAttribKHR)                                       \
    T(PFNEGLSTREAMCONSUMERACQUIREKHRPROC, eglStreamConsumerAcquireKHR)                     \
    T(PFNEGLSTREAMCONSUMERRELEASEKHRPROC, eglStreamConsumerReleaseKHR)                     \
    T(PFNEGLSTREAMCONSUMERGLTEXTUREEXTERNALKHRPROC, eglStreamConsumerGLTextureExternalKHR) \
    T(PFNEGLGETSTREAMFILEDESCRIPTORKHRPROC, eglGetStreamFileDescriptorKHR)                 \
    T(PFNEGLQUERYDEVICESEXTPROC, eglQueryDevicesEXT)                                       \
    T(PFNEGLGETPLATFORMDISPLAYEXTPROC, eglGetPlatformDisplayEXT)                           \
    T(PFNEGLQUERYDEVICEATTRIBEXTPROC, eglQueryDeviceAttribEXT)                             \
    T(PFNEGLCREATESTREAMFROMFILEDESCRIPTORKHRPROC, eglCreateStreamFromFileDescriptorKHR)

#define eglCreateStreamKHR                    my_eglCreateStreamKHR
#define eglDestroyStreamKHR                   my_eglDestroyStreamKHR
#define eglQueryStreamKHR                     my_eglQueryStreamKHR
#define eglQueryStreamu64KHR                  my_eglQueryStreamu64KHR
#define eglQueryStreamTimeKHR                 my_eglQueryStreamTimeKHR
#define eglStreamAttribKHR                    my_eglStreamAttribKHR
#define eglStreamConsumerAcquireKHR           my_eglStreamConsumerAcquireKHR
#define eglStreamConsumerReleaseKHR           my_eglStreamConsumerReleaseKHR
#define eglStreamConsumerGLTextureExternalKHR my_eglStreamConsumerGLTextureExternalKHR
#define eglGetStreamFileDescriptorKHR         my_eglGetStreamFileDescriptorKHR
#define eglCreateStreamFromFileDescriptorKHR  my_eglCreateStreamFromFileDescriptorKHR
#define eglQueryDevicesEXT                    my_eglQueryDevicesEXT
#define eglGetPlatformDisplayEXT              my_eglGetPlatformDisplayEXT
#define eglQueryDeviceAttribEXT               my_eglQueryDeviceAttribEXT

#define EXTLST_DECL(tx, x)   tx my_##x = NULL;
#define EXTLST_EXTERN(tx, x) extern tx my_##x;
#define EXTLST_ENTRY(tx, x)  {(extlst_fnptr_t *)&my_##x, #x},

#define MAX_STRING_SIZE 256
#define WIDTH           720
#define HEIGHT          480

typedef struct _TestArgs
{
    char        *infile1;
    char        *infile2;
    bool         isARGB;
    unsigned int inputWidth;
    unsigned int inputHeight;
    bool         pitchLinearOutput;
} TestArgs;

int  eglSetupExtensions(void);
int  EGLStreamInit(int *dev);
void EGLStreamFini(void);
#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.h`.

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

- **Total Lines**: 101
- **Approximate Size**: 4895 bytes

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
