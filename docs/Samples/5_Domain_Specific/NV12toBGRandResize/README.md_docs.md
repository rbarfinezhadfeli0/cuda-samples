# Documentation: Samples/5_Domain_Specific/NV12toBGRandResize/README.md
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/NV12toBGRandResize/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 2110 bytes
- **Lines**: 33
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```markdown
# NV12toBGRandResize - NV12toBGRandResize

## Description

This code shows two ways to convert and resize NV12 frames to BGR 3 planars frames using CUDA in batch. Way-1, Convert NV12 Input to BGR @ Input Resolution-1, then Resize to Resolution#2. Way-2, resize NV12 Input to Resolution#2 then convert it to BGR Output. NVIDIA HW Decoder, both dGPU and Tegra, normally outputs NV12 pitch format frames. For the inference using TensorRT, the input frame needs to be BGR planar format with possibly different size. So, conversion and resizing from NV12 to BGR planar is usually required for the inference following decoding. This CUDA code provides a reference implementation for conversion and resizing.

## Key Concepts

Graphics Interop, Image Processing, Video Processing

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaStreamDestroy, cudaMalloc, cudaFree, cudaMallocManaged, cudaStreamAttachMemAsync, cudaDestroyTextureObject, cudaEventSynchronize, cudaDeviceSynchronize, cudaCreateTextureObject, cudaEventRecord, cudaEventDestroy, cudaEventElapsedTime, cudaStreamCreate, cudaEventCreate

## Prerequisites

Download and install the [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) for your corresponding platform.

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

