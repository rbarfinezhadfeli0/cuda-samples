# Documentation: Samples/2_Concepts_and_Techniques/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1101 bytes
- **Lines**: 33
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
add_subdirectory(EGLStream_CUDA_CrossGPU)
add_subdirectory(EGLStream_CUDA_Interop)
add_subdirectory(FunctionPointers)
add_subdirectory(MC_EstimatePiInlineP)
add_subdirectory(MC_EstimatePiInlineQ)
add_subdirectory(MC_EstimatePiP)
add_subdirectory(MC_EstimatePiQ)
add_subdirectory(MC_SingleAsianOptionP)
add_subdirectory(boxFilter)
add_subdirectory(convolutionSeparable)
add_subdirectory(convolutionTexture)
add_subdirectory(dct8x8)
add_subdirectory(eigenvalues)
add_subdirectory(histogram)
add_subdirectory(imageDenoising)
add_subdirectory(inlinePTX)
add_subdirectory(inlinePTX_nvrtc)
add_subdirectory(interval)
add_subdirectory(particles)
add_subdirectory(radixSortThrust)
add_subdirectory(reduction)
add_subdirectory(reductionMultiBlockCG)
add_subdirectory(scalarProd)
add_subdirectory(scan)
add_subdirectory(segmentationTreeThrust)
add_subdirectory(shfl_scan)
add_subdirectory(sortingNetworks)
add_subdirectory(streamOrderedAllocation)
add_subdirectory(streamOrderedAllocationIPC)
add_subdirectory(streamOrderedAllocationP2P)
add_subdirectory(threadFenceReduction)
add_subdirectory(threadMigration)

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

