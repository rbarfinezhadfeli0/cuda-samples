# Documentation: Samples/3_CUDA_Features/bf16TensorCoreGemm/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/bf16TensorCoreGemm/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1234 bytes
- **Lines**: 36
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(bf16TensorCoreGemm LANGUAGES C CXX CUDA)

find_package(CUDAToolkit REQUIRED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_CUDA_ARCHITECTURES 75 80 86 87 89 90 100 110 120)
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
# Add target for bf16TensorCoreGemm
add_executable(bf16TensorCoreGemm bf16TensorCoreGemm.cu)

target_compile_options(bf16TensorCoreGemm PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

target_compile_features(bf16TensorCoreGemm PRIVATE cxx_std_17 cuda_std_17)

set_target_properties(bf16TensorCoreGemm PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

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

