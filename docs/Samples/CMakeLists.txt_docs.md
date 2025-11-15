# Documentation: Samples/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 867 bytes
- **Lines**: 34
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
# This layer of CMakeLists.txt adds folders, for better organization in Visual Studio
# and other IDEs that support this feature.

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_FOLDER "0_Introduction")
add_subdirectory(0_Introduction)

set(CMAKE_FOLDER "1_Utilities")
add_subdirectory(1_Utilities)

set(CMAKE_FOLDER "2_Concepts_and_Techniques")
add_subdirectory(2_Concepts_and_Techniques)

set(CMAKE_FOLDER "3_CUDA_Features")
add_subdirectory(3_CUDA_Features)

set(CMAKE_FOLDER "4_CUDA_Libraries")
add_subdirectory(4_CUDA_Libraries)

set(CMAKE_FOLDER "5_Domain_Specific")
add_subdirectory(5_Domain_Specific)

set(CMAKE_FOLDER "6_Performance")
add_subdirectory(6_Performance)

set(CMAKE_FOLDER "7_libNVVM")
add_subdirectory(7_libNVVM)

if(BUILD_TEGRA)
    set(CMAKE_FOLDER "8_Platform_Specific/Tegra")
    add_subdirectory(8_Platform_Specific/Tegra)
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

