# Documentation: Samples/8_Platform_Specific/Tegra/cuDLAErrorReporting/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cuDLAErrorReporting/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1702 bytes
- **Lines**: 50
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../../cmake/Modules")

project(cuDLAErrorReporting LANGUAGES C CXX CUDA)

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

find_library(CUDLA_LIB cudla PATHS ${CUDAToolkit_LIBRARY_DIR} ${CMAKE_LIBRARY_PATH})

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    if(CUDLA_LIB)
        # Source file
        # Add target for cuDLAErrorReporting
        add_executable(cuDLAErrorReporting main.cu)

        target_compile_options(cuDLAErrorReporting PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

        target_compile_features(cuDLAErrorReporting PRIVATE cxx_std_17 cuda_std_17)

        set_target_properties(cuDLAErrorReporting PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

        target_include_directories(cuDLAErrorReporting PUBLIC
            ${CUDAToolkit_INCLUDE_DIRS}
        )

        target_link_libraries(cuDLAErrorReporting
            ${CUDLA_LIB}
        )
    else()
        message(STATUS "CUDLA not found - will not build sample 'cuDLAErrorReporting'")
    endif()
else()
    message(STATUS "Will not build sample cuDLAErrorReporting - requires Linux OS")
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

