# Documentation: Samples/6_Performance/UnifiedMemoryPerf/README.md
---
## File Metadata
- **Path**: `Samples/6_Performance/UnifiedMemoryPerf/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1273 bytes
- **Lines**: 35
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```markdown
# UnifiedMemoryPerf - Unified and other CUDA Memories Performance

## Description

This sample demonstrates the performance comparision using matrix multiplication kernel of Unified Memory with/without hints and other types of memory like zero copy buffers, pageable, pagelocked memory performing synchronous and Asynchronous transfers on a single GPU.

## Key Concepts

CUDA Systems Integration, Unified Memory, CUDA Streams and Events, Pinned System Paged Memory

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l, aarch64

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaMemcpy, cudaStreamDestroy, cudaMemPrefetchAsync, cudaFree, cudaMallocHost, cudaMallocManaged, cudaStreamAttachMemAsync, cudaHostGetDevicePointer, cudaFreeHost, cudaStreamSynchronize, cudaMalloc, cudaMemcpyAsync, cudaStreamCreate, cudaGetDeviceProperties

## Dependencies needed to build/run
[UVM](../../../README.md#uvm)

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

