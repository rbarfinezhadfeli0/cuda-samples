# Documentation: Samples/1_Utilities/deviceQuery/README.md
---
## File Metadata
- **Path**: `Samples/1_Utilities/deviceQuery/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1503 bytes
- **Lines**: 36
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```markdown
# deviceQuery - Device Query

## Description

This sample enumerates the properties of the CUDA devices present in the system.

## Key Concepts

CUDA Runtime API, Device Query

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l, aarch64

## CUDA APIs involved

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetAttribute, cuSafeCallNoSync

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaRuntimeGetVersion, cudaGetErrorString, cudaDeviceCanAccessPeer, cudaSetDevice, cudaGetDeviceCount, cudaDriverGetVersion, cudaGetDeviceProperties

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

