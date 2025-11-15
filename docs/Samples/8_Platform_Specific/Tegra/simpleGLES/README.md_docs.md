# Documentation: Samples/8_Platform_Specific/Tegra/simpleGLES/README.md
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/simpleGLES/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1711 bytes
- **Lines**: 37
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# simpleGLES - Simple OpenGLES

## Description

Demonstrates data exchange between CUDA and OpenGL ES (aka Graphics interop). The program modifies vertex positions with CUDA and uses OpenGL ES to render the geometry.

## Key Concepts

Graphics Interop, Vertex Buffers, 3D Graphics

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux

## Supported CPU Architecture

armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaMemcpy, cudaFree, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaDeviceSynchronize, cudaGraphicsUnregisterResource, cudaMalloc, cudaGraphicsGLRegisterBuffer

## Dependencies needed to build/run
[X11](../../../README.md#x11), [GLES](../../../README.md#gles)

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

