# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/README.md
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 2216 bytes
- **Lines**: 40
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# cudaNvSciNvMedia - NvMedia CUDA Interop

## Description

This sample demonstrates CUDA-NvMedia interop via NvSciBuf/NvSciSync APIs. Note that this sample only supports cross build from x86_64 to aarch64, aarch64 native build is not supported. For detailed workflow of the sample please check cudaNvSciNvMedia_Readme.pdf in the sample directory.

## Key Concepts

CUDA NvSci Interop, Data Parallel Algorithms, Image Processing

## Supported SM Architectures

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, QNX

## Supported CPU Architecture

aarch64

## CUDA APIs involved

### [CUDA Driver API](http://docs.nvidia.com/cuda/cuda-driver-api/index.html)
cuDeviceGetUuid

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaImportExternalSemaphore, cudaGetMipmappedArrayLevel, cudaSetDevice, cudaDestroySurfaceObject, cudaCreateSurfaceObject, cudaImportNvSciImage, cudaCreateChannelDesc, cudaMallocHost, cudaSignalExternalSemaphoresAsync, cudaFreeHost, cudaMemcpyAsync, cudaStreamCreateWithFlags, cudaExternalMemoryGetMappedMipmappedArray, cudaMallocArray, cudaFreeArray, cudaStreamDestroy, cudaDeviceGetNvSciSyncAttributes, cudaDestroyExternalMemory, cudaImportExternalMemory, cudaDestroyExternalSemaphore, cudaFreeMipmappedArray, cudaImportNvSciSync, cudaFree, cudaStreamSynchronize, cudaMalloc, cudaWaitExternalSemaphoresAsync

## Dependencies needed to build/run
[NVSCI](../../../README.md#nvsci), [NvMedia](../../../README.md#nvmedia)

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

