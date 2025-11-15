# Documentation: Samples/2_Concepts_and_Techniques/shfl_scan/README.md
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/shfl_scan/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1031 bytes
- **Lines**: 35
- **Generated**: 2025-11-15 12:53:54 UTC

---
## Original Source
```markdown
# shfl_scan - CUDA Parallel Prefix Sum with Shuffle Intrinsics (SHFL_Scan)

## Description

This example demonstrates how to use the shuffle intrinsic __shfl_up_sync to perform a scan operation across a thread block.

## Key Concepts

Data-Parallel Algorithms, Performance Strategies

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaMallocHost, cudaEventSynchronize, cudaEventRecord, cudaFreeHost, cudaGetDevice, cudaMemset, cudaMalloc, cudaEventElapsedTime, cudaGetDeviceProperties, cudaEventCreate

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

