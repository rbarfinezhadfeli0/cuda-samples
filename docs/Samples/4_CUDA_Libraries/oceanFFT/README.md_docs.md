# Documentation: Samples/4_CUDA_Libraries/oceanFFT/README.md
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/oceanFFT/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1786 bytes
- **Lines**: 37
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```markdown
# oceanFFT - CUDA FFT Ocean Simulation

## Description

This sample simulates an Ocean height field using CUFFT Library and renders the result using OpenGL.

## Key Concepts

Graphics Interop, Image Processing, CUFFT Library

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaMemcpy, cudaMalloc, cudaFree, cudaGraphicsResourceGetMappedPointer, cudaCalculateSlopeKernel, cudaGraphicsMapResources, cudaUpdateHeightmapKernel, cudaGraphicsUnregisterResource, cudaGenerateSpectrumKernel, cudaGraphicsGLRegisterBuffer, cudaGetDeviceProperties

## Dependencies needed to build/run
[X11](../../../README.md#x11), [GL](../../../README.md#gl), [CUFFT](../../../README.md#cufft)

## Prerequisites

Download and install the [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) for your corresponding platform.
Make sure the dependencies mentioned in [Dependencies]() section above are installed.

## References (for more details)

```

---
## High-Level Overview
This file is a markdown source file in the CUDA Samples repository.


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

