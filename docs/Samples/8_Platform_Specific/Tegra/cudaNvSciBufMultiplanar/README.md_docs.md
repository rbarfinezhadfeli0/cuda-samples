# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/README.md
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 2103 bytes
- **Lines**: 37
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# cudaNvSciBufMultiplanar - CUDA NvSciBufMultiplanar Image Samples

## Description

This sample demonstrates CUDA-NvSciBuf Interop for Multiplanar images. A YUV 420 multiplanar image is flipped and allocated using NvSciBuf APIs and imported into CUDA with CUDA External Resource Interoperability. A CUDA surface is created from the corresponding mapped CUDA array and again bit flipping is performed on the surface. The result is copied back to a YUV image which is compared against the input.

## Key Concepts

CUDA NvSci Interop, Data Parallel Algorithms, Image Processing

## Supported SM Architectures

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 10.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 10.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 12.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux

## Supported CPU Architecture

aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaDeviceGetAttribute, cudaNvSciBufMultiplanar, cudaDestroyExternalMemory, cuDriverGetVersion, cuDeviceGetUuid, cudaSetDevice, cudaGetMipmappedArrayLevel, cudaFreeMipmappedArray, cudaImportExternalMemory, cudaCreateChannelDesc, cudaExternalMemoryGetMappedMipmappedArray, cuCtxSynchronize, cudaMemcpy2DToArray, cudaMemcpy2DFromArray

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

