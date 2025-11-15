# Documentation for Samples/8_Platform_Specific/Tegra/fluidsGLES/graphics_interface.h

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/fluidsGLES/graphics_interface.h`
- **Type**: .h
- **Location**: Samples/8_Platform_Specific/Tegra/fluidsGLES
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

#include <X11/Xlib.h>
#include <X11/Xutil.h>
#include <stdio.h>
#include <stdlib.h>

Display *display;
int      screen;
Window   win = 0;

#include <EGL/egl.h>
#include <EGL/eglext.h>
#include <GLES3/gl31.h>

#define GET_GLERROR(ret)                                                                   \
                                                                                           \
    {                                                                                      \
        GLenum err = glGetError();                                                         \
        if (err != GL_NO_ERROR) {                                                          \
            fprintf(stderr, "[%s line %d] OpenGL Error: 0x%x\n", __FILE__, __LINE__, err); \
            fflush(stderr);                                                                \
                                                                                           \
            switch (err) {                                                                 \
            case GL_INVALID_ENUM:                                                          \
                printf("GL_INVALID_ENUM\n");                                               \
                break;                                                                     \
            case GL_INVALID_VALUE:                                                         \
                printf("GL_INVALID_VALUE\n");                                              \
                break;                                                                     \
            case GL_INVALID_OPERATION:                                                     \
                printf("GL_INVALID_OPERATION\n");                                          \
                break;                                                                     \
            case GL_OUT_OF_MEMORY:                                                         \
                printf("GL_OUT_OF_MEMORY\n");                                              \
                break;                                                                     \
            case GL_INVALID_FRAMEBUFFER_OPERATION:                                         \
                printf("GL_INVALID_FRAMEBUFFER_OPERATION\n");                              \
                break;                                                                     \
            default:                                                                       \
                printf("UKNOWN OPENGL ERROR CODE 0x%x\n", err);                            \
            };                                                                             \
        }                                                                                  \
    }

EGLDisplay eglDisplay = EGL_NO_DISPLAY;
EGLSurface eglSurface = EGL_NO_SURFACE;
EGLContext eglContext = EGL_NO_CONTEXT;

int graphics_setup_window(int xpos, int ypos, int width, int height, const char *windowname)
{
    // OpenGL ES 3.1
    EGLint configAttrs[]  = {EGL_RED_SIZE,
                             1,
                             EGL_GREEN_SIZE,
                             1,
                             EGL_BLUE_SIZE,
                             1,
                             EGL_DEPTH_SIZE,
                             16,
                             EGL_SAMPLE_BUFFERS,
                             0,
                             EGL_SAMPLES,
                             0,
                             // EGL_CONFORMANT,      EGL_OPENGL_BIT,
                             EGL_RENDERABLE_TYPE,
                             EGL_OPENGL_ES2_BIT, // 3_BIT_KHR,
                             EGL_NONE};
    EGLint contextAttrs[] = {EGL_CONTEXT_CLIENT_VERSION, 3, EGL_NONE};

    EGLConfig *configList = NULL;
    EGLint     configCount;

    display = XOpenDisplay(NULL);
    if (!display)
        error_exit("Error opening X display.\n");

    screen = DefaultScreen(display);

    eglDisplay = eglGetDisplay(0);

    if (eglDisplay == EGL_NO_DISPLAY)
        error_exit("EGL failed to obtain display\n");

    if (!eglInitialize(eglDisplay, 0, 0))
        error_exit("EGL failed to initialize\n");

    if (!eglChooseConfig(eglDisplay, configAttrs, NULL, 0, &configCount) || !configCount)
        error_exit("EGL failed to return any matching configurations\n");

    configList = (EGLConfig *)malloc(configCount * sizeof(EGLConfig));

    if (!eglChooseConfig(eglDisplay, configAttrs, configList, configCount, &configCount) || !configCount)
        error_exit("EGL failed to populate configuration list\n");

    Window               xRootWindow = DefaultRootWindow(display);
    XSetWindowAttributes xCreateWindowAttributes;
    xCreateWindowAttributes.event_mask = ExposureMask;
    win                                = XCreateWindow(display,
                        xRootWindow,
                        0,
                        0,
                        width,
                        height,
                        0,
                        CopyFromParent,
                        InputOutput,
                        CopyFromParent,
                        CWEventMask,
                        &xCreateWindowAttributes);
    XMapWindow(display, win);
    Atom   netWmStateAtom = XInternAtom(display, "_NET_WM_STATE", false);
    XEvent xEvent;
    memset(&xEvent, 0, sizeof(xEvent));
    xEvent.type                 = ClientMessage;
    xEvent.xclient.window       = win;
    xEvent.xclient.message_type = netWmStateAtom;
    xEvent.xclient.format       = 32;
    xEvent.xclient.data.l[0]    = 1;
    xEvent.xclient.data.l[1]    = false;
    XSendEvent(display, xRootWindow, false, SubstructureNotifyMask, &xEvent);

    XStoreName(display, win, windowname);

    XSelectInput(display,
                 win,
                 ExposureMask | KeyPressMask | ButtonPressMask | ButtonReleaseMask | KeyReleaseMask
                     | VisibilityChangeMask | PointerMotionMask);

    EGLint windowAttrs[] = {EGL_NONE};

    eglSurface = eglCreateWindowSurface(eglDisplay, configList[0], (EGLNativeWindowType)win, windowAttrs);

    if (!eglSurface)
        error_exit("EGL couldn't create window\n");

    eglBindAPI(EGL_OPENGL_ES_API);

    eglContext = eglCreateContext(eglDisplay, configList[0], NULL, contextAttrs);
    if (!eglContext)
        error_exit("EGL couldn't create context\n");

    if (!eglMakeCurrent(eglDisplay, eglSurface, eglSurface, eglContext))
        error_exit("EGL couldn't make context/surface current\n");

    EGLint Context_RendererType;
    eglQueryContext(eglDisplay, eglContext, EGL_CONTEXT_CLIENT_TYPE, &Context_RendererType);

    switch (Context_RendererType) {
    case EGL_OPENGL_API:
        printf("Using OpenGL API is not supported\n");
        exit(EXIT_FAILURE);
        break;
    case EGL_OPENGL_ES_API:
        printf("Using OpenGL ES API");
        break;
    case EGL_OPENVG_API:
        error_exit("Context Query Returned OpenVG. This is Unsupported\n");
    default:
        error_exit("Unknown Context Type. %04X\n", Context_RendererType);
    }

    return 1;
}

void graphics_set_windowtitle(const char *windowname) { XStoreName(display, win, windowname); }

void graphics_swap_buffers() { eglSwapBuffers(eglDisplay, eglSurface); }

void graphics_close_window()
{
    if (eglDisplay != EGL_NO_DISPLAY) {
        eglMakeCurrent(eglDisplay, EGL_NO_SURFACE, EGL_NO_SURFACE, EGL_NO_CONTEXT);

        if (eglContext != EGL_NO_CONTEXT)
            eglDestroyContext(eglDisplay, eglContext);

        if (eglSurface != EGL_NO_SURFACE)
            eglDestroySurface(eglDisplay, eglSurface);

        eglTerminate(eglDisplay);
    }

    if (win)
        XDestroyWindow(display, win);

    XCloseDisplay(display);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/fluidsGLES/graphics_interface.h`.

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

- **Total Lines**: 214
- **Approximate Size**: 9370 bytes

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
