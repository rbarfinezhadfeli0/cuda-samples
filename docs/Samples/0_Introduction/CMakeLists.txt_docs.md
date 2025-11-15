# Documentation: Samples/0_Introduction/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/0_Introduction/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1543 bytes
- **Lines**: 47
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
add_subdirectory(UnifiedMemoryStreams)
add_subdirectory(asyncAPI)
add_subdirectory(clock)
add_subdirectory(clock_nvrtc)
add_subdirectory(cudaOpenMP)
add_subdirectory(fp16ScalarProduct)
add_subdirectory(matrixMul)
add_subdirectory(matrixMulDrv)
add_subdirectory(matrixMulDynlinkJIT)
add_subdirectory(matrixMul_nvrtc)
add_subdirectory(mergeSort)
add_subdirectory(simpleAWBarrier)
add_subdirectory(simpleAssert)
add_subdirectory(simpleAssert_nvrtc)
add_subdirectory(simpleAtomicIntrinsics)
add_subdirectory(simpleAtomicIntrinsics_nvrtc)
add_subdirectory(simpleAttributes)
add_subdirectory(simpleCUDA2GL)
add_subdirectory(simpleCallback)
add_subdirectory(simpleCooperativeGroups)
add_subdirectory(simpleCubemapTexture)
add_subdirectory(simpleDrvRuntime)
add_subdirectory(simpleHyperQ)
add_subdirectory(simpleIPC)
add_subdirectory(simpleLayeredTexture)
add_subdirectory(simpleMPI)
add_subdirectory(simpleMultiCopy)
add_subdirectory(simpleMultiGPU)
add_subdirectory(simpleOccupancy)
add_subdirectory(simpleP2P)
add_subdirectory(simplePitchLinearTexture)
add_subdirectory(simplePrintf)
add_subdirectory(simpleStreams)
add_subdirectory(simpleSurfaceWrite)
add_subdirectory(simpleTemplates)
add_subdirectory(simpleTexture)
add_subdirectory(simpleTexture3D)
add_subdirectory(simpleTextureDrv)
add_subdirectory(simpleVoteIntrinsics)
add_subdirectory(simpleZeroCopy)
add_subdirectory(template)
add_subdirectory(systemWideAtomics)
add_subdirectory(vectorAdd)
add_subdirectory(vectorAddDrv)
add_subdirectory(vectorAddMMAP)
add_subdirectory(vectorAdd_nvrtc)

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

