# Documentation: Samples/0_Introduction/UnifiedMemoryStreams/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/0_Introduction/UnifiedMemoryStreams/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1696 bytes
- **Lines**: 52
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(UnifiedMemoryStreams LANGUAGES C CXX CUDA)

find_package(CUDAToolkit REQUIRED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_CUDA_ARCHITECTURES 75 80 86 87 89 90 100 110 120)
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -Wno-deprecated-gpu-targets")
if(ENABLE_CUDA_DEBUG)
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -G")        # enable cuda-gdb (may significantly affect performance on some targets)
else()
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -lineinfo") # add line information to all builds for debug tools (exclusive to -G option)
endif()

# Include directories and libraries
include_directories(../../../Common)

# This sample is not supported on QNX
if(CMAKE_SYSTEM_NAME STREQUAL "QNX")
    message(STATUS "Will not build sample ${PROJECT_NAME} - not supported on QNX")
    return()
endif()

# Source file
if(CMAKE_GENERATOR MATCHES "Visual Studio")
    find_package(OpenMP REQUIRED C CXX)
else()
    find_package(OpenMP REQUIRED)
endif()

if(${OpenMP_FOUND})
    # Add target for UnifiedMemoryStreams
    add_executable(UnifiedMemoryStreams UnifiedMemoryStreams.cu)

target_compile_options(UnifiedMemoryStreams PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

target_compile_features(UnifiedMemoryStreams PRIVATE cxx_std_17 cuda_std_17)

    set_target_properties(UnifiedMemoryStreams PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

    target_link_libraries(UnifiedMemoryStreams PUBLIC
        CUDA::cublas
        OpenMP::OpenMP_CXX
    )
else()
    message(STATUS "OpenMP not found - will not build sample 'UnifiedMemoryStreams'")
endif()

```

---
## High-Level Overview
This file is a text source file in the CUDA Samples repository.


---
## Detailed Walkthrough

---
## Usage Examples
Refer to the repository documentation for usage instructions.


---
## Performance & Security Notes
### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

