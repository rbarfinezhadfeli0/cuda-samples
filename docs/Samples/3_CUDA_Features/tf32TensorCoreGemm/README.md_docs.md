# Documentation: Samples/3_CUDA_Features/tf32TensorCoreGemm/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/tf32TensorCoreGemm/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1494 bytes
- **Lines**: 37
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# tf32TensorCoreGemm - tf32 Tensor Core GEMM

## Description

A CUDA sample demonstrating tf32 (e8m10) GEMM computation using the Warp Matrix Multiply and Accumulate (WMMA) API introduced with CUDA 11 in Ampere chip family tensor cores for faster matrix operations. This sample also uses async copy provided by cuda pipeline interface for gmem to shmem async loads which improves kernel performance and reduces register presssure.

## Key Concepts

Matrix Multiply, WMMA, Tensor Cores

## Supported SM Architectures

[SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaGetErrorString, cudaGetLastError, cudaEventSynchronize, cudaFuncSetAttribute, cudaEventRecord, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

## Dependencies needed to build/run
[CPP11](../../../README.md#cpp11)

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

