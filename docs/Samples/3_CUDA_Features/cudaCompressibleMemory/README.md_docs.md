# Documentation: Samples/3_CUDA_Features/cudaCompressibleMemory/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/cudaCompressibleMemory/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1683 bytes
- **Lines**: 36
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# cudaCompressibleMemory - CUDA Compressible Memory

## Description

This sample demonstrates the compressible memory allocation using cuMemMap API.

## Key Concepts

CUDA Driver API, Compressible Memory, MMAP

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows, QNX

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemGetAllocationPropertiesFromHandle, cuMemCreate, cuDeviceGetAttribute, cuCtxGetDevice, cuMemGetAllocationGranularity, cuMemAddressFree, cuMemUnmap, cuMemMap, cuMemRelease, cuMemAddressReserve, cuMemSetAccess

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaEventSynchronize, cudaEventRecord, cudaEventElapsedTime, cudaOccupancyMaxPotentialBlockSize, cudaEventCreate

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

