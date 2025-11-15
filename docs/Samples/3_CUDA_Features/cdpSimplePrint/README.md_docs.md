# Documentation: Samples/3_CUDA_Features/cdpSimplePrint/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/cdpSimplePrint/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 897 bytes
- **Lines**: 35
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# cdpSimplePrint - Simple Print (CUDA Dynamic Parallelism)

## Description

This sample demonstrates simple printf implemented using CUDA Dynamic Parallelism.  This sample requires devices with compute capability 3.5 or higher.

## Key Concepts

CUDA Dynamic Parallelism

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaDeviceSynchronize, cudaGetLastError, cudaGetDeviceProperties, cudaDeviceSetLimit

## Dependencies needed to build/run
[CDP](../../../README.md#cdp)

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

