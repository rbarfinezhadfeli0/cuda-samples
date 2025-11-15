# Documentation: Samples/8_Platform_Specific/Tegra/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 453 bytes
- **Lines**: 13
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
add_subdirectory(cudaNvSciNvMedia)
add_subdirectory(cudaNvSciBufMultiplanar)
add_subdirectory(cuDLAErrorReporting)
add_subdirectory(cuDLAHybridMode)
add_subdirectory(cuDLALayerwiseStatsHybrid)
add_subdirectory(cuDLALayerwiseStatsStandalone)
add_subdirectory(cuDLAStandaloneMode)
add_subdirectory(EGLSync_CUDAEvent_Interop)
add_subdirectory(fluidsGLES)
add_subdirectory(nbody_opengles)
add_subdirectory(simpleGLES)
add_subdirectory(simpleGLES_EGLOutput)

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

