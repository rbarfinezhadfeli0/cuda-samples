# Documentation: Samples/0_Introduction/simpleStreams/README.md
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleStreams/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1782 bytes
- **Lines**: 33
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```markdown
# simpleStreams - simpleStreams

## Description

This sample uses CUDA streams to overlap kernel executions with memory copies between the host and a GPU device.  This sample uses a new CUDA 4.0 feature that supports pinning of generic host memory.  Requires Compute Capability 2.0 or higher.

## Key Concepts

Asynchronous Data Transfers, CUDA Streams and Events

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaSetDeviceFlags, cudaSetDevice, cudaEventDestroy, cudaStreamCreate, cudaMallocHost, cudaEventCreateWithFlags, cudaFreeHost, cudaMemcpyAsync, cudaGetDeviceCount, cudaStreamDestroy, cudaMemset, cudaEventElapsedTime, cudaHostAlloc, cudaFree, cudaHostRegister, cudaEventSynchronize, cudaEventRecord, cudaMalloc, cudaGetDeviceProperties, cudaHostUnregister

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

