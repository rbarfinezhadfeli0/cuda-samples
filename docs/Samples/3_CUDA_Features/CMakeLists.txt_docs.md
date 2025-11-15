# Documentation: Samples/3_CUDA_Features/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 861 bytes
- **Lines**: 25
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
add_subdirectory(StreamPriorities)
add_subdirectory(bf16TensorCoreGemm)
add_subdirectory(binaryPartitionCG)
add_subdirectory(bindlessTexture)
add_subdirectory(cdpAdvancedQuicksort)
add_subdirectory(cdpBezierTessellation)
add_subdirectory(cdpQuadtree)
add_subdirectory(cdpSimplePrint)
add_subdirectory(cdpSimpleQuicksort)
add_subdirectory(cudaCompressibleMemory)
add_subdirectory(cudaTensorCoreGemm)
add_subdirectory(dmmaTensorCoreGemm)
add_subdirectory(globalToShmemAsyncCopy)
add_subdirectory(graphConditionalNodes)
add_subdirectory(graphMemoryFootprint)
add_subdirectory(graphMemoryNodes)
add_subdirectory(immaTensorCoreGemm)
add_subdirectory(jacobiCudaGraphs)
add_subdirectory(memMapIPCDrv)
add_subdirectory(newdelete)
add_subdirectory(ptxjit)
add_subdirectory(simpleCudaGraphs)
add_subdirectory(tf32TensorCoreGemm)
add_subdirectory(warpAggregatedAtomicsCG)

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

