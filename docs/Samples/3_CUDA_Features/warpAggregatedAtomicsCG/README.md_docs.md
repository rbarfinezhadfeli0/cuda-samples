# Documentation: Samples/3_CUDA_Features/warpAggregatedAtomicsCG/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/warpAggregatedAtomicsCG/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 852 bytes
- **Lines**: 31
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# warpAggregatedAtomicsCG - Warp Aggregated Atomics using Cooperative Groups

## Description

This sample demonstrates how using Cooperative Groups (CG) to perform warp aggregated atomics to single and multiple counters, a useful technique to improve performance when many threads atomically add to a single or multiple counters.

## Key Concepts

Cooperative Groups, Atomic Intrinsics

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaFree, cudaDeviceGetAttribute, cudaMemset, cudaMalloc

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

