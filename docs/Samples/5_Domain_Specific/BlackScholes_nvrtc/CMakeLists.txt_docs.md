# Documentation: Samples/5_Domain_Specific/BlackScholes_nvrtc/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/BlackScholes_nvrtc/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1464 bytes
- **Lines**: 42
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(BlackScholes_nvrtc LANGUAGES C CXX CUDA)

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

# Source file
# Add target for BlackScholes_nvrtc
add_executable(BlackScholes_nvrtc BlackScholes.cpp BlackScholes_gold.cpp)

target_compile_options(BlackScholes_nvrtc PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

target_compile_features(BlackScholes_nvrtc PRIVATE cxx_std_17 cuda_std_17)

set_target_properties(BlackScholes_nvrtc PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

target_link_libraries(BlackScholes_nvrtc PRIVATE
    CUDA::nvrtc
    CUDA::cuda_driver
)

# Copy kernel to the output directory
add_custom_command(TARGET BlackScholes_nvrtc POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
    ${CMAKE_CURRENT_SOURCE_DIR}/BlackScholes_kernel.cuh ${CMAKE_CURRENT_BINARY_DIR}
)

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

