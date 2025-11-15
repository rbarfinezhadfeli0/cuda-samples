# Documentation: Samples/5_Domain_Specific/bicubicTexture/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/bicubicTexture/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 2981 bytes
- **Lines**: 87
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```text
cmake_minimum_required(VERSION 3.20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake/Modules")

project(bicubicTexture LANGUAGES C CXX CUDA)

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

if(WIN32)
    set(PC_GLUT_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/../../../Common")
    set(PC_GLUT_LIBRARY_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/../../../Common/lib/x64")
endif()

find_package(OpenGL)
find_package(GLUT)

# Source file
if(${OpenGL_FOUND})
    if (${GLUT_FOUND})
        # Add target for bicubicTexture
        add_executable(bicubicTexture bicubicTexture.cpp bicubicTexture_cuda.cu)

        target_compile_options(bicubicTexture PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:--extended-lambda>)

        target_compile_features(bicubicTexture PRIVATE cxx_std_17 cuda_std_17)

        set_target_properties(bicubicTexture PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

        target_include_directories(bicubicTexture PUBLIC
            ${OPENGL_INCLUDE_DIR}
            ${CUDAToolkit_INCLUDE_DIRS}
            ${GLUT_INCLUDE_DIRS}
        )

        target_link_libraries(bicubicTexture
            ${OPENGL_LIBRARIES}
            ${GLUT_LIBRARIES}
        )

        # Copy data files to the output directory
        add_custom_command(TARGET bicubicTexture POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_directory
            ${CMAKE_CURRENT_SOURCE_DIR}/data
            ${CMAKE_CURRENT_BINARY_DIR}/data
        )

        if(WIN32)
            target_link_libraries(bicubicTexture
                ${PC_GLUT_LIBRARY_DIRS}/freeglut.lib
                ${PC_GLUT_LIBRARY_DIRS}/glew64.lib
            )

            add_custom_command(TARGET bicubicTexture
                POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/win64/$<CONFIGURATION>/freeglut.dll
                ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIGURATION>
            )

            add_custom_command(TARGET bicubicTexture
                POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_CURRENT_SOURCE_DIR}/../../../bin/win64/$<CONFIGURATION>/glew64.dll
                ${CMAKE_CURRENT_BINARY_DIR}/$<CONFIGURATION>
            )
        endif()

    else()
        message(STATUS "GLUT not found - will not build sample 'bicubicTexture'")
    endif()
else()
    message(STATUS "OpenGL not found - will not build sample 'bicubicTexture'")
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

