# Documentation for Samples/7_libNVVM/cuda-c-linking/CMakeLists.txt

## File Metadata

- **Path**: `Samples/7_libNVVM/cuda-c-linking/CMakeLists.txt`
- **Type**: .txt
- **Location**: Samples/7_libNVVM/cuda-c-linking
- **Binary**: No

## Purpose and Role

This is a CMake build configuration file.

## Original Source Content

```txt
# Copyright (c) 1993-2025, NVIDIA CORPORATION. All rights reserved.
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

message(STATUS "Found LLVM Version ${LLVM_PACKAGE_VERSION}")
if (LLVM_PACKAGE_VERSION VERSION_GREATER_EQUAL "15" OR
    LLVM_PACKAGE_VERSION VERSION_LESS "7")
  message(STATUS "The cuda-c-linking sample is expected to build with "
                 "LLVM development libraries v7 to v14, opaque pointers are "
                 "not supported in libNVVM for pre-Blackwell architectures.")
  return()
endif ()

add_executable(cuda-c-linking cuda-c-linking.cpp)

add_test(NAME cuda-c-linking
   COMMAND cuda-c-linking
   WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}")
target_link_libraries(cuda-c-linking ${NVVM_LIB} ${CUDA_LIB})

# See https://llvm.org/docs/CMake.html#developing-llvm-passes-out-of-source
separate_arguments(LLVM_DEFINITIONS_LIST NATIVE_COMMAND ${LLVM_DEFINITIONS})
add_definitions(${LLVM_DEFINITIONS_LIST})
include_directories(${LLVM_INCLUDE_DIRS})
llvm_map_components_to_libnames(llvm_libs core support)
target_link_libraries(cuda-c-linking ${llvm_libs})

if (UNIX)
  set_target_properties(cuda-c-linking PROPERTIES
                        LINK_FLAGS "-Wl,-rpath,${LIBNVVM_RPATH}")
endif()

##############################
### Math Lib
##############################

### The math library can be built to support a number of GPU platforms using the
### common compute_75 architecture.
### For a collection of specific architecutres, the compiler options
### "-gencode=compute_XX,code=sm_XX"... can be used multiple times in the same
### nvcc invocation.
### The result is bundled into a system library using the 'nvcc -lib' feature.
### >> nvcc -m64 -arch=compute_75 -dc math-funcs.cu -o math-funcs64.o
### >> nvcc -m64 -lib math-funcs64.o -o libmathfuncs64.a

enable_language(CUDA)
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -dc") # -dc: Build for device only.
if (${CMAKE_VERSION} VERSION_GREATER_EQUAL 3.18.0)
  set(CMAKE_CUDA_ARCHITECTURES 75)
  set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -Wno-deprecated-gpu-targets")
endif ()
if (WIN32)
  set(CMAKE_CUDA_USE_RESPONSE_FILE_FOR_INCLUDES 0)
  set(CMAKE_CUDA_USE_RESPONSE_FILE_FOR_LIBRARIES 0)
  set(CMAKE_CUDA_USE_RESPONSE_FILE_FOR_OBJECTS 0)
endif ()
add_library(mathfuncs64 STATIC math-funcs.cu)
set_target_properties(mathfuncs64 PROPERTIES PREFIX "lib"
                      OUTPUT_NAME "mathfuncs64"
                      SUFFIX ".a" CUDA_SEPERABLE_COMPILATION ON)
install(TARGETS cuda-c-linking mathfuncs64 DESTINATION bin)

if (WIN32)
  add_custom_command(
      TARGET cuda-c-linking
      POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
              "${CMAKE_BINARY_DIR}/nvvm64_40_0.dll" "$<TARGET_FILE_DIR:cuda-c-linking>"
  )
endif ()

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/7_libNVVM/cuda-c-linking/CMakeLists.txt`.

### Build System Role

This CMake file is responsible for:

1. Defining build targets
2. Configuring compilation options
3. Managing dependencies
4. Setting up CUDA-specific build parameters
5. Organizing sample projects

## Detailed Analysis

### File Statistics

- **Total Lines**: 93
- **Approximate Size**: 4168 bytes

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
