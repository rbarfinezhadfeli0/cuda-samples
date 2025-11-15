# Documentation: Samples/4_CUDA_Libraries/nvJPEG/README.md
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/nvJPEG/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 963 bytes
- **Lines**: 35
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# nvJPEG - NVJPEG simple

## Description

A CUDA Sample that demonstrates single and batched decoding of jpeg images using NVJPEG Library.

## Key Concepts

Image Decoding, NVJPEG Library

## Supported SM Architectures

## Supported OSes

Linux, Windows, QNX

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaHostAlloc, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaFree, cudaEventSynchronize, cudaEventRecord, cudaFreeHost, cudaStreamSynchronize, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## Dependencies needed to build/run
[NVJPEG](../../../README.md#nvjpeg)

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

