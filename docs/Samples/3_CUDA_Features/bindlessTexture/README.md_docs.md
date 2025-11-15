# Documentation: Samples/3_CUDA_Features/bindlessTexture/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/bindlessTexture/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1322 bytes
- **Lines**: 35
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# bindlessTexture - Bindless Texture

## Description

This example demonstrates use of cudaSurfaceObject, cudaTextureObject, and MipMap support in CUDA.  A GPU with Compute Capability SM 3.0 is required to run the sample.

## Key Concepts

Graphics Interop, Texture

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaGetMipmappedArrayLevel, cudaGraphicsMapResources, cudaDestroySurfaceObject, cudaExtent, cudaDeviceSynchronize, cudaCreateSurfaceObject, cudaMallocMipmappedArray, cudaPitchedPtr, cudaGraphicsResourceGetMappedPointer, cudaCreateTextureObject, cudaGraphicsUnmapResources, cudaMallocArray, cudaFreeArray, cudaArrayGetInfo, cudaGetLastError, cudaDestroyTextureObject, cudaGraphicsGLRegisterBuffer, cudaFreeMipmappedArray, cudaFree, cudaGraphicsUnregisterResource, cudaMalloc

## Dependencies needed to build/run
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

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

