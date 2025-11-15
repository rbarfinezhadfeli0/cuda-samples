# Documentation for Samples/7_libNVVM/CMakeLists.txt

## File Metadata

- **Path**: `Samples/7_libNVVM/CMakeLists.txt`
- **Type**: .txt
- **Location**: Samples/7_libNVVM
- **Binary**: No

## Purpose and Role

This is a CMake build configuration file.

## Original Source Content

```txt
# Copyright (c) 1993-2023, NVIDIA CORPORATION. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#  * Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
#  * Neither the name of NVIDIA CORPORATION nor the names of its
#    contributors may be used to endorse or promote products derived
#    from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
# CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
# OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

cmake_minimum_required(VERSION 3.10.0)

project(libnvvm-samples)
enable_testing()
#set_property(GLOBAL PROPERTY ALLOW_DUPLICATE_CUSTOM_TARGETS TRUE)
set(CMAKE_C_STANDARD 11)

# Initialize the CUDA Toolkit search using CUDA_HOME if the user specified it.
if (DEFINED ENV{CUDA_HOME})
  if (${CMAKE_VERSION} VERSION_LESS 3.18.0)
    set(CUDA_TOOLKIT_ROOT_DIR  "$ENV{CUDA_HOME}" CACHE PATH "Path to CUDA Toolkit.")
  else ()  # cmake 3.17 introduced FindCUDAToolkit
    set(CUDAToolkit_ROOT "$ENV{CUDA_HOME}" CACHE PATH "Path to CUDA Toolkit.")
  endif ()
endif ()

# Locate the root directory of the CUDA Toolkit.
# cmake 3.18 introduces the CUDAToolkit_LIBRARY_ROOT variable.
# This will update CUDA_HOME to a value discovered by find_package.
if (${CMAKE_VERSION} VERSION_LESS 3.18.0)
  find_package(CUDA REQUIRED)
  set(CUDA_HOME "${CUDA_TOOLKIT_ROOT_DIR}")
  get_filename_component(CUDA_LIB_ROOT "${CUDA_cudart_static_LIBRARY}" DIRECTORY)
  find_library(CUDA_LIB NAMES cuda cuda.lib PATHS "${CUDA_LIB_ROOT}" "${CUDA_LIB_ROOT}/stubs")
  include_directories("${CUDA_INCLUDE_DIRS}")
  find_library(CUDADEVRT_LIB cudadevrt PATHS "${CUDA_LIB_ROOT}" "${CUDA_LIB_ROOT}/stubs")
else () # Else, we're using cmake versions >= 3.18.
  cmake_policy(SET CMP0074 NEW) # Use CUDAToolkit_ROOT as a cmake prefix.
  find_package(CUDAToolkit REQUIRED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
  set(CUDA_LIB "${CUDA_cuda_driver_LIBRARY}")
  include_directories("${CUDAToolkit_INCLUDE_DIRS}")
  get_filename_component(CUDA_HOME "${CUDAToolkit_BIN_DIR}" DIRECTORY)
  find_library(CUDADEVRT_LIB cudadevrt PATH "${CUDAToolkit_LIBRARY_DIR}")
endif ()
message(STATUS "Using CUDA_HOME: ${CUDA_HOME}")
message(STATUS "Using CUDA_LIB:  ${CUDA_LIB}")
if (("${CUDA_HOME}" STREQUAL "") OR ("${CUDA_LIB}" STREQUAL ""))
  message(FATAL_ERROR "Failed to locate paths to the CUDA toolkit and nvcc.")
endif ()

# Find the nvvm directory in the toolkit.
find_file(LIBNVVM_HOME nvvm PATHS "$ENV{LIBNVVM_HOME}" "${CUDA_HOME}")
message(STATUS "Using LIBNVVM_HOME: ${LIBNVVM_HOME}")

# Find libNVVM and nvvm.h.
# (Linux: nvvm/lib64, windows: nvvm/lib/x64)
find_library(NVVM_LIB nvvm PATHS "${LIBNVVM_HOME}/lib64" "${LIBNVVM_HOME}/lib/x64")
find_file(NVVM_H nvvm.h PATH "${LIBNVVM_HOME}/include")
get_filename_component(NVVM_INCLUDE_DIR ${NVVM_H} DIRECTORY)
include_directories(${NVVM_INCLUDE_DIR})
message(STATUS "Using libnvvm header:      ${NVVM_H}")
message(STATUS "Using libnvvm header path: ${NVVM_INCLUDE_DIR}")
message(STATUS "Using libnvvm library:     ${NVVM_LIB}")

# Set the rpath to libnvvm.
find_path(LIBNVVM_RPATH lib lib64 PATHS "$ENV{LIBNVVM_HOME}" "${CUDA_HOME}")
if(DEFINED RUNTIME_LIBNVVM_PATH)
  message(STATUS "Using RUNTIME_LIBNVVM_PATH: ${RUNTIME_LIBNVVM_PATH}")
  set(LIBNVVM_RPATH ${RUNTIME_LIBNVVM_PATH})
else()
  get_filename_component(LIBNVVM_RPATH ${NVVM_LIB} DIRECTORY)
endif()
set(CMAKE_INSTALL_RPATH "${LIBNVVM_RPATH}")
message(STATUS "Using rpath: ${CMAKE_INSTALL_RPATH}")

# On Windows, locate the nvvm.dll so we can install it.
if (WIN32)
  find_file(NVVM_DLL nvvm64_40_0.dll PATHS "${LIBNVVM_HOME}/bin" "${LIBNVVM_HOME}/bin/x64/")
  if (NOT NVVM_DLL)
    message(FATAL_ERROR "Found nvvm .h/.lib, but not .dll")
  endif()
  install(FILES ${NVVM_DLL} DESTINATION bin)
  file(COPY ${NVVM_DLL} DESTINATION "${CMAKE_BINARY_DIR}")
endif (WIN32)

add_definitions(-DLIBDEVICE_MAJOR_VERSION=1)
add_definitions(-DLIBDEVICE_MINOR_VERSION=0)
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/common/include")

# If you wish to build the cuda-c-linking sample and have the LLVM dependencies
# met, then set the ENABLE_CUDA_C_LINKING_SAMPLE variable. This variable can be
# set locally or by adding "-DENABLE_CUDA_C_LINKING_SAMPLE=1" to your cmake
# invocation. See the note about "cuda-c-linking" in README.md.
if (ENABLE_CUDA_C_LINKING_SAMPLE)
  # Include the LLVM dev package which is required to build cuda-c-linking.
  find_package(LLVM CONFIG PATHS "$ENV{LLVM_HOME}")
  if (LLVM_FOUND)
    add_subdirectory(cuda-c-linking)
  else ()
    message(STATUS "Skipping the build of the cuda-c-linking sample: Failed to locate the LLVM package.")
  endif ()
else ()
  message(STATUS "Skipping the build of the cuda-c-linking sample.")
endif ()

# Samples
add_subdirectory(cuda-shared-memory)
add_subdirectory(device-side-launch)
add_subdirectory(simple)
add_subdirectory(syscalls)
add_subdirectory(ptxgen)
add_subdirectory(uvmlite)

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/7_libNVVM/CMakeLists.txt`.

### Build System Role

This CMake file is responsible for:

1. Defining build targets
2. Configuring compilation options
3. Managing dependencies
4. Setting up CUDA-specific build parameters
5. Organizing sample projects

## Detailed Analysis

### File Statistics

- **Total Lines**: 131
- **Approximate Size**: 5950 bytes

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
