# Documentation: Samples/5_Domain_Specific/CMakeLists.txt
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/CMakeLists.txt`
- **Filename**: `CMakeLists.txt`
- **Language**: text
- **Size**: 1183 bytes
- **Lines**: 37
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```text
add_subdirectory(BlackScholes)
add_subdirectory(BlackScholes_nvrtc)
add_subdirectory(FDTD3d)
add_subdirectory(HSOpticalFlow)
add_subdirectory(Mandelbrot)
add_subdirectory(MonteCarloMultiGPU)
add_subdirectory(NV12toBGRandResize)
add_subdirectory(SobelFilter)
add_subdirectory(SobolQRNG)
add_subdirectory(bicubicTexture)
add_subdirectory(bilateralFilter)
add_subdirectory(binomialOptions)
add_subdirectory(binomialOptions_nvrtc)
add_subdirectory(convolutionFFT2D)
add_subdirectory(dwtHaar1D)
add_subdirectory(dxtc)
add_subdirectory(fastWalshTransform)
add_subdirectory(fluidsGL)
add_subdirectory(marchingCubes)
add_subdirectory(nbody)
add_subdirectory(p2pBandwidthLatencyTest)
add_subdirectory(postProcessGL)
add_subdirectory(quasirandomGenerator)
add_subdirectory(quasirandomGenerator_nvrtc)
add_subdirectory(recursiveGaussian)
add_subdirectory(simpleD3D11)
add_subdirectory(simpleD3D11Texture)
add_subdirectory(simpleD3D12)
add_subdirectory(simpleGL)
add_subdirectory(simpleVulkan)
add_subdirectory(simpleVulkanMMAP)
add_subdirectory(smokeParticles)
add_subdirectory(stereoDisparity)
add_subdirectory(volumeFiltering)
add_subdirectory(volumeRender)
add_subdirectory(vulkanImageCUDA)

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

