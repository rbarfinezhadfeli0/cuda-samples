# Documentation: Samples/3_CUDA_Features/graphConditionalNodes/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/graphConditionalNodes/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1436 bytes
- **Lines**: 33
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# graphConditionalNodes - Graph Conditional Nodes

## Description

A demonstration of CUDA graphs conditional nodes.

## Key Concepts

CUDA Graphs

## Supported SM Architectures

[SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaDeviceSynchronize, cudaDriverGetVersion, cudaFree, cudaGraphAddNode, cudaGraphConditionalHandleCreate, cudaGraphCreate, cudaGraphDestroy, cudaGraphExecDestroy, cudaGraphInstantiate, cudaGraphLaunch, cudaGraphSetConditional, cudaMalloc, cudaMemset, cudaStreamBeginCapture, cudaStreamBeginCaptureToGraph, cudaStreamCreate, cudaStreamDestroy, cudaStreamEndCapture, cudaStreamGetCaptureInfo

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

