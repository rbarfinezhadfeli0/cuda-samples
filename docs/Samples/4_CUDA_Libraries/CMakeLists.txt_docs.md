# Documentation: Samples/4_CUDA_Libraries/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1230 bytes
- **Lines**: 35
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```text
add_subdirectory(FilterBorderControlNPP)
add_subdirectory(MersenneTwisterGP11213)
add_subdirectory(batchCUBLAS)
add_subdirectory(boxFilterNPP)
add_subdirectory(cannyEdgeDetectorNPP)
add_subdirectory(conjugateGradient)
add_subdirectory(conjugateGradientCudaGraphs)
add_subdirectory(conjugateGradientMultiBlockCG)
add_subdirectory(conjugateGradientMultiDeviceCG)
add_subdirectory(conjugateGradientPrecond)
add_subdirectory(conjugateGradientUM)
add_subdirectory(cudaNvSci)
add_subdirectory(cuSolverDn_LinearSolver)
add_subdirectory(cuSolverRf)
add_subdirectory(cuSolverSp_LinearSolver)
add_subdirectory(cuSolverSp_LowlevelCholesky)
add_subdirectory(cuSolverSp_LowlevelQR)
add_subdirectory(freeImageInteropNPP)
add_subdirectory(histEqualizationNPP)
add_subdirectory(jitLto)
add_subdirectory(lineOfSight)
add_subdirectory(matrixMulCUBLAS)
add_subdirectory(nvJPEG)
add_subdirectory(nvJPEG_encoder)
add_subdirectory(oceanFFT)
add_subdirectory(randomFog)
add_subdirectory(simpleCUBLAS)
add_subdirectory(simpleCUBLASXT)
add_subdirectory(simpleCUBLAS_LU)
add_subdirectory(simpleCUFFT)
add_subdirectory(simpleCUFFT_2d_MGPU)
add_subdirectory(simpleCUFFT_MGPU)
add_subdirectory(simpleCUFFT_callback)
add_subdirectory(watershedSegmentationNPP)

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

