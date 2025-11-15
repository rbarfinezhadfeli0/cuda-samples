# Documentation: cmake/Modules/FindNVSCI.cmake
---
## File Metadata
- **Path**: `cmake/Modules/FindNVSCI.cmake`
- **Filename**: `FindNVSCI.cmake`
- **Language**: text
- **Size**: 1427 bytes
- **Lines**: 62
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
# Find the NVSCI libraries and headers
#
# This module defines the following variables:
#  NVSCI_FOUND        - True if NVSCI was found
#  NVSCI_INCLUDE_DIRS - NVSCI include directories
#  NVSCI_LIBRARIES    - NVSCI libraries
#  NVSCIBUF_LIBRARY   - NVSCI buffer library
#  NVSCISYNC_LIBRARY  - NVSCI sync library

# Find the libraries
find_library(NVSCIBUF_LIBRARY
    NAMES nvscibuf libnvscibuf
    PATHS 
        /usr/lib
        /usr/lib/${CMAKE_SYSTEM_PROCESSOR}-linux-gnu
    PATH_SUFFIXES nvidia
)

find_library(NVSCISYNC_LIBRARY
    NAMES nvscisync libnvscisync
    PATHS 
        /usr/lib
        /usr/lib/${CMAKE_SYSTEM_PROCESSOR}-linux-gnu
    PATH_SUFFIXES nvidia
)

# Find the header files
find_path(NVSCIBUF_INCLUDE_DIR
    NAMES nvscibuf.h
    PATHS 
        /usr/include
        /usr/local/include
)

find_path(NVSCISYNC_INCLUDE_DIR
    NAMES nvscisync.h
    PATHS 
        /usr/include
        /usr/local/include
)

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(NVSCI
    REQUIRED_VARS 
        NVSCIBUF_LIBRARY
        NVSCISYNC_LIBRARY
        NVSCIBUF_INCLUDE_DIR
        NVSCISYNC_INCLUDE_DIR
)

if(NVSCI_FOUND)
    set(NVSCI_LIBRARIES ${NVSCIBUF_LIBRARY} ${NVSCISYNC_LIBRARY})
    set(NVSCI_INCLUDE_DIRS ${NVSCIBUF_INCLUDE_DIR} ${NVSCISYNC_INCLUDE_DIR})
endif()

mark_as_advanced(
    NVSCIBUF_LIBRARY
    NVSCISYNC_LIBRARY
    NVSCIBUF_INCLUDE_DIR
    NVSCISYNC_INCLUDE_DIR
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

