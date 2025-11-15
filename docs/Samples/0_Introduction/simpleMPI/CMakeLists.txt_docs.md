# Documentation: Samples/0_Introduction/simpleMPI/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleMPI/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1402 bytes
- **Lines**: 47
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(simpleMPI LANGUAGES C CXX CUDA)

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

find_package(MPI)

# Source file
if(${MPI_FOUND})
    # Add target for simpleMPI
    add_executable(simpleMPI simpleMPI.cpp simpleMPI.cu)

target_compile_options(simpleMPI PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

target_compile_features(simpleMPI PRIVATE cxx_std_17 cuda_std_17)

    set_target_properties(simpleMPI PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

    target_include_directories(simpleMPI PUBLIC
        ${MPI_INCLUDE_PATH}
    )

    target_link_libraries(simpleMPI PUBLIC
        ${MPI_C_LIBRARIES}
        ${MPI_CXX_LIBRARIES}
    )

else()
    message(STATUS "MPI not found - will not build sample 'simpleMPI'")
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

