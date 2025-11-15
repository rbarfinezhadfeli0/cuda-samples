# Documentation: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/README.md
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 2093 bytes
- **Lines**: 40
- **Generated**: 2025-11-15 12:53:54 UTC

---
## Original Source
```markdown
# EGLStream_CUDA_Interop - EGLStream CUDA Interop

## Description

Demonstrates data exchange between CUDA and EGL Streams.

## Key Concepts

EGLStreams Interop

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuMemcpyDtoH, cuDeviceGetName, cuEGLStreamConsumerReleaseFrame, cuEGLStreamConsumerConnect, cuEGLStreamConsumerDisconnect, cuCtxPushCurrent, cuArrayDestroy, cuEGLStreamProducerReturnFrame, cuEGLStreamProducerPresentFrame, cuMemFree, cuGraphicsResourceGetMappedEglFrame, cuInit, cuEGLStreamConsumerAcquireFrame, cuEGLStreamProducerDisconnect, cuDeviceGetCount, cuEGLStreamProducerConnect, cuDeviceGetAttribute, cuCtxSynchronize, cuMemAlloc, cuCtxPopCurrent, cuCtxCreate, cuMemcpy

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaProducerReadYUVFrame, cudaProducerTest, cudaProducerDeinit, cudaDeviceCreateProducer, cudaProducerReadARGBFrame, cudaDeviceCreateConsumer, cudaConsumerTest, cudaProducerInit

## Dependencies needed to build/run
[EGL](../../../README.md#egl)

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

