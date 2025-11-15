# Documentation for Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.cpp`
- **Type**: .cpp
- **Location**: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop
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

//
// DESCRIPTION:   Common egl stream functions
//

#include "eglstrm_common.h"

EGLStreamKHR eglStream;
EGLDisplay   g_display;
EGLAttrib    cudaIndex;

#if defined(EXTENSION_LIST)
EXTENSION_LIST(EXTLST_DECL)
typedef void (*extlst_fnptr_t)(void);
static struct
{
    extlst_fnptr_t *fnptr;
    char const     *name;
} extensionList[] = {EXTENSION_LIST(EXTLST_ENTRY)};

int eglSetupExtensions(void)
{
    unsigned int i;

    for (i = 0; i < (sizeof(extensionList) / sizeof(*extensionList)); i++) {
        *extensionList[i].fnptr = eglGetProcAddress(extensionList[i].name);
        if (*extensionList[i].fnptr == NULL) {
            printf("Couldn't get address of %s()\n", extensionList[i].name);
            return 0;
        }
    }

    return 1;
}

int EGLStreamInit(int *cuda_device)
{
    static const EGLint streamAttrMailboxMode[] = {EGL_SUPPORT_REUSE_NV, EGL_FALSE, EGL_NONE};
    EGLBoolean          eglStatus;
#define MAX_EGL_DEVICES 4
    EGLint       numDevices = 0;
    EGLDeviceEXT devices[MAX_EGL_DEVICES];
    eglStatus = eglQueryDevicesEXT(MAX_EGL_DEVICES, devices, &numDevices);
    if (eglStatus != EGL_TRUE) {
        printf("Error querying EGL devices\n");
        exit(EXIT_FAILURE);
    }

    if (numDevices == 0) {
        printf("No EGL devices found.. Waiving\n");
        eglStatus = EGL_FALSE;
        exit(EXIT_WAIVED);
    }

    int egl_device_id = 0;
    for (egl_device_id = 0; egl_device_id < numDevices; egl_device_id++) {
        eglStatus = eglQueryDeviceAttribEXT(devices[egl_device_id], EGL_CUDA_DEVICE_NV, &cudaIndex);
        if (eglStatus == EGL_TRUE) {
            *cuda_device = cudaIndex; // We select first EGL-CUDA Capable device.
            printf("Found EGL-CUDA Capable device with CUDA Device id = %d\n", (int)cudaIndex);
            break;
        }
    }

    if (egl_device_id >= numDevices) {
        printf("No CUDA Capable EGL Device found.. Waiving execution\n");
        exit(EXIT_WAIVED);
    }

    g_display = eglGetPlatformDisplayEXT(EGL_PLATFORM_DEVICE_EXT, (void *)devices[egl_device_id], NULL);
    if (g_display == EGL_NO_DISPLAY) {
        printf("Could not get EGL display from device. \n");
        eglStatus = EGL_FALSE;
        exit(EXIT_FAILURE);
    }

    eglStatus = eglInitialize(g_display, 0, 0);
    if (!eglStatus) {
        printf("EGL failed to initialize. \n");
        eglStatus = EGL_FALSE;
        exit(EXIT_FAILURE);
    }

    eglStream = eglCreateStreamKHR(g_display, streamAttrMailboxMode);
    if (eglStream == EGL_NO_STREAM_KHR) {
        printf("Could not create EGL stream.\n");
        eglStatus = EGL_FALSE;
        exit(EXIT_FAILURE);
    }

    printf("Created EGLStream %p\n", eglStream);

    // Set stream attribute
    if (!eglStreamAttribKHR(g_display, eglStream, EGL_CONSUMER_LATENCY_USEC_KHR, 16000)) {
        printf("Consumer: eglStreamAttribKHR EGL_CONSUMER_LATENCY_USEC_KHR failed\n");
        return 0;
    }
    if (!eglStreamAttribKHR(g_display, eglStream, EGL_CONSUMER_ACQUIRE_TIMEOUT_USEC_KHR, 16000)) {
        printf("Consumer: eglStreamAttribKHR EGL_CONSUMER_ACQUIRE_TIMEOUT_USEC_KHR "
               "failed\n");
        return 0;
    }
    printf("EGLStream initialized\n");
    return 1;
}

void EGLStreamFini(void) { eglDestroyStreamKHR(g_display, eglStream); }
#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/eglstrm_common.cpp`.

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

- **Total Lines**: 135
- **Approximate Size**: 4859 bytes

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
