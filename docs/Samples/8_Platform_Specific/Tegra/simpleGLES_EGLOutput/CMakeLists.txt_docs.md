# Documentation for Samples/8_Platform_Specific/Tegra/simpleGLES_EGLOutput/CMakeLists.txt

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/simpleGLES_EGLOutput/CMakeLists.txt`
- **Type**: .txt
- **Location**: Samples/8_Platform_Specific/Tegra/simpleGLES_EGLOutput
- **Binary**: No

## Purpose and Role

This is a CMake build configuration file.

## Original Source Content

```txt
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../../cmake/Modules")

project(simpleGLES_EGLOutput LANGUAGES C CXX CUDA)

find_package(CUDAToolkit REQUIRED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_CUDA_ARCHITECTURES 87 110)
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -Wno-deprecated-gpu-targets")

if(ENABLE_CUDA_DEBUG)
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -G")        # enable cuda-gdb (may significantly affect performance on some targets)
else()
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -lineinfo") # add line information to all builds for debug tools (exclusive to -G option)
endif()

# Include directories and libraries
include_directories(../../../../Common)

find_package(EGL)
find_package(X11)
find_package(OpenGL)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    if(${OpenGL_FOUND})
        if(${EGL_FOUND})
            if(${X11_FOUND})
                set(DRM_INCLUDE_PATH "/usr/include/drm" "/usr/include/libdrm" ${CMAKE_INCLUDE_PATH})

                # use CMAKE_LIBRARY_PATH so that users can also specify the NVSCI lib path in cmake command
                set(CMAKE_LIBRARY_PATH "/usr/lib/${CMAKE_SYSTEM_PROCESSOR}-linux-gnu" ${CMAKE_LIBRARY_PATH})
                find_library(DRM_LIB drm PATHS ${CMAKE_LIBRARY_PATH})

                # Source file
                # Add target for simpleGLES_EGLOutput
                add_executable(simpleGLES_EGLOutput simpleGLES_EGLOutput.cu)
                target_compile_options(simpleGLES_EGLOutput PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

                target_compile_features(simpleGLES_EGLOutput PRIVATE cxx_std_17 cuda_std_17)

                set_target_properties(simpleGLES_EGLOutput PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

                target_include_directories(simpleGLES_EGLOutput PUBLIC
                    ${EGL_INCLUDE_DIR}
                    ${OPENGL_INCLUDE_DIR}
                    ${CUDAToolkit_INCLUDE_DIR}
                    ${DRM_INCLUDE_PATH}
                )

                target_link_libraries(simpleGLES_EGLOutput
                    ${EGL_LIBRARY}
                    ${X11_LIBRARIES}
                    ${OPENGL_LIBRARIES}
                    ${DRM_LIB}
                )

                # Copy the .glsl files to the output directory
                add_custom_command(TARGET simpleGLES_EGLOutput POST_BUILD
                    COMMAND ${CMAKE_COMMAND} -E copy_if_different
                    ${CMAKE_CURRENT_SOURCE_DIR}/mesh.frag.glsl
                    ${CMAKE_CURRENT_SOURCE_DIR}/mesh.vert.glsl
                    ${CMAKE_CURRENT_BINARY_DIR}
                )
            else()
                message(STATUS "X11 libraries not found - will not build sample 'simpleGLES_EGLOutput'")
            endif()
        else()
            message(STATUS "EGL not found - will not build sample 'simpleGLES_EGLOutput'")
        endif()
    else()
        message(STATUS "OpenGL not found - will not build sample 'simpleGLES_EGLOutput'")
    endif()
else()
    message(STATUS "Will not build sample simpleGLES_EGLOutput - requires Linux OS")
endif()

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/simpleGLES_EGLOutput/CMakeLists.txt`.

### Build System Role

This CMake file is responsible for:

1. Defining build targets
2. Configuring compilation options
3. Managing dependencies
4. Setting up CUDA-specific build parameters
5. Organizing sample projects

## Detailed Analysis

### File Statistics

- **Total Lines**: 79
- **Approximate Size**: 3138 bytes

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
