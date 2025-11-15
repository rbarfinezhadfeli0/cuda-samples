# Documentation: Samples/0_Introduction/systemWideAtomics/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/0_Introduction/systemWideAtomics/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1398 bytes
- **Lines**: 39
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(systemWideAtomics LANGUAGES C CXX CUDA)

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

if(CMAKE_SYSTEM_PROCESSOR STREQUAL "aarch64")
    message(STATUS "Will not build sample systemWideAtomics - not supported on aarch64")
else()
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
        # Source file
        # Add target for systemWideAtomics
        add_executable(systemWideAtomics systemWideAtomics.cu)

        target_compile_options(systemWideAtomics PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

        target_compile_features(systemWideAtomics PRIVATE cxx_std_17 cuda_std_17)

        set_target_properties(systemWideAtomics PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

    else()
        message(STATUS "Will not build sample systemWideAtomics - requires Linux OS")
    endif()
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

