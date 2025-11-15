# Documentation: Samples/4_CUDA_Libraries/histEqualizationNPP/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/histEqualizationNPP/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 2184 bytes
- **Lines**: 69
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(histEqualizationNPP LANGUAGES CXX)

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
include_directories(
    ../../../Common
    ../../../Common/UtilNPP
)

# Source file
find_package(FreeImage)

if(${FreeImage_FOUND})
    # Add target for histEqualizationNPP
    add_executable(histEqualizationNPP histEqualizationNPP.cpp)

    target_compile_options(histEqualizationNPP PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

    target_compile_features(histEqualizationNPP PRIVATE cxx_std_17 cuda_std_17)

    set_target_properties(histEqualizationNPP PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

    target_include_directories(histEqualizationNPP PRIVATE
        ${CUDAToolkit_INCLUDE_DIRS}
        ${FreeImage_INCLUDE_DIRS}
    )

    target_link_libraries(histEqualizationNPP PRIVATE
        CUDA::nppc
        CUDA::nppisu
        CUDA::nppist
        CUDA::nppicc
        CUDA::cudart
        ${FreeImage_LIBRARIES}
    )

    # Copy data files to output directory
    add_custom_command(TARGET histEqualizationNPP POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${CMAKE_CURRENT_SOURCE_DIR}/../../../Common/data/teapot512.pgm
        ${CMAKE_CURRENT_BINARY_DIR}/
    )
    if(WIN32)
        add_custom_command(TARGET histEqualizationNPP
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        ${FreeImage_LIBRARY}/../FreeImage.dll
        ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIGURATION>
        )
    endif()
else()
    message(STATUS "FreeImage not found - will not build sample 'histEqualizationNPP'")
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

