# Documentation: cmake/Modules/FindEGL.cmake
---
## File Metadata
- **Path**: `cmake/Modules/FindEGL.cmake`
- **Filename**: `FindEGL.cmake`
- **Language**: text
- **Size**: 379 bytes
- **Lines**: 18
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
find_path(EGL_INCLUDE_DIR
  NAMES EGL/egl.h
  PATHS /usr/include /usr/local/include
)

find_library(EGL_LIBRARY
  NAMES EGL
  PATHS /usr/lib /usr/local/lib
)

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(EGL DEFAULT_MSG EGL_LIBRARY EGL_INCLUDE_DIR)

if(EGL_FOUND)
  set(EGL_LIBRARIES ${EGL_LIBRARY})
  set(EGL_INCLUDE_DIRS ${EGL_INCLUDE_DIR})
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

