# Documentation: Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/README.md
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1634 bytes
- **Lines**: 37
- **Generated**: 2025-11-15 12:53:54 UTC

---
## Original Source
```markdown
# reductionMultiBlockCG - Reduction using MultiBlock Cooperative Groups

## Description

This sample demonstrates single pass reduction using Multi Block Cooperative Groups.  This sample requires devices with compute capability 6.0 or higher having compute preemption.

## Key Concepts

Cooperative Groups, MultiBlock Cooperative Groups

## Supported SM Architectures

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaSetDevice, cudaDeviceSynchronize, cudaLaunchCooperativeKernel, cudaMalloc, cudaOccupancyMaxActiveBlocksPerMultiprocessor, cudaGetDeviceProperties, cudaOccupancyMaxPotentialBlockSize

## Dependencies needed to build/run
[MBCG](../../../README.md#mbcg), [CPP11](../../../README.md#cpp11)

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

