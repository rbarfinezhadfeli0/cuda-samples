# Documentation: Samples/4_CUDA_Libraries/cudaNvSci/README.md
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/cudaNvSci/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 2322 bytes
- **Lines**: 40
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# cudaNvSci - CUDA NvSciBuf/NvSciSync Interop

## Description

This sample demonstrates CUDA-NvSciBuf/NvSciSync Interop. Two CPU threads import the NvSciBuf and NvSciSync into CUDA to perform two image processing algorithms on a ppm image - image rotation in 1st thread &amp; rgba to grayscale conversion of rotated image in 2nd thread. Currently only supported on Ubuntu 18.04

## Key Concepts

CUDA NvSci Interop, Data Parallel Algorithms, Image Processing

## Supported SM Architectures

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetUuid

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaExternalMemoryGetMappedBuffer, cudaImportExternalSemaphore, cudaDeviceGetAttribute, cudaNvSciSignal, cudaGetMipmappedArrayLevel, cudaImportNvSciRawBuf, cudaSetDevice, cudaImportNvSciImage, cudaNvSciApp, cudaDeviceId, cudaMallocHost, cudaSignalExternalSemaphoresAsync, cudaCreateTextureObject, cudaFreeHost, cudaNvSci, cudaNvSciWait, cudaGetDeviceCount, cudaMemcpyAsync, cudaStreamCreateWithFlags, cudaExternalMemoryGetMappedMipmappedArray, cudaStreamDestroy, cudaDeviceGetNvSciSyncAttributes, cudaDestroyTextureObject, cudaDestroyExternalMemory, cudaImportExternalMemory, cudaDestroyExternalSemaphore, cudaFreeMipmappedArray, cudaFree, cudaStreamSynchronize, cudaWaitExternalSemaphoresAsync, cudaImportNvSciSemaphore

## Dependencies needed to build/run
[NVSCI](../../../README.md#nvsci)

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

