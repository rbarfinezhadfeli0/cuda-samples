# Documentation: Samples/4_CUDA_Libraries/cudaNvSci/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/cudaNvSci/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 2257 bytes
- **Lines**: 64
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(cudaNvSci LANGUAGES C CXX CUDA)

find_package(CUDAToolkit REQUIRED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_CUDA_ARCHITECTURES 75 80 86 87 90 110)
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -Wno-deprecated-gpu-targets")

if(ENABLE_CUDA_DEBUG)
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -G")        # enable cuda-gdb (may significantly affect performance on some targets)
else()
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -lineinfo") # add line information to all builds for debug tools (exclusive to -G option)
endif()

# Include directories and libraries
include_directories(../../../Common)

find_package(NVSCI)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    if(NVSCI_FOUND)
        message(STATUS "FOUND NVSCI libs: ${NVSCIBUF_LIB} ${NVSCISYNC_LIB}")
        message(STATUS "Using NVSCI headers path: ${NVSCIBUF_INCLUDE_DIR} ${NVSCIBUF_INCLUDE_DIR}")
        # Source file
        # Add target for cudaNvSci
        add_executable(cudaNvSci imageKernels.cu cudaNvSci.cpp main.cpp)

        target_compile_options(cudaNvSci PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

        target_compile_features(cudaNvSci PRIVATE cxx_std_17 cuda_std_17)

        set_target_properties(cudaNvSci PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

        target_include_directories(cudaNvSci PUBLIC
            ${CUDAToolkit_INCLUDE_DIRS}
            ${NVSCI_INCLUDE_DIRS}
        )

        target_link_libraries(cudaNvSci
            CUDA::cuda_driver
            ${NVSCI_LIBRARIES}
        )
        # Copy teapot1024.ppm to the output directory
        add_custom_command(TARGET cudaNvSci POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/teapot1024.ppm ${CMAKE_CURRENT_BINARY_DIR}/teapot1024.ppm
        )

        # Specify additional clean files
        set_target_properties(cudaNvSci PROPERTIES
            ADDITIONAL_CLEAN_FILES "teapot1024_out.ppm"
        )
    else()
        message(STATUS "NvSCI not found - will not build sample 'cudaNvSci'")
    endif()
else()
    message(STATUS "Will not build sample cudaNvSci - requires Linux OS")
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

