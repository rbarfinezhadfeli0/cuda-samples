# Documentation: Samples/3_CUDA_Features/simpleCudaGraphs/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/simpleCudaGraphs/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1176 bytes
- **Lines**: 31
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# simpleCudaGraphs - Simple CUDA Graphs

## Description

A demonstration of CUDA Graphs creation, instantiation and launch using Graphs APIs and Stream Capture APIs.

## Key Concepts

CUDA Graphs, Stream Capture

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphClone, cudaExtent, cudaGraphLaunch, cudaStreamCreate, cudaLaunchHostFunc, cudaGraphAddMemcpyNode, cudaMallocHost, cudaPitchedPtr, cudaStreamEndCapture, cudaGraphCreate, cudaFreeHost, cudaGraphGetNodes, cudaMemsetAsync, cudaMemcpyAsync, cudaGraphAddHostNode, cudaGraphInstantiate, cudaStreamDestroy, cudaStreamBeginCapture, cudaStreamWaitEvent, cudaEventCreate, cudaMalloc, cudaFree, cudaPos, cudaGraphAddKernelNode, cudaGraphDestroy, cudaEventRecord, cudaGraphsManual, cudaStreamSynchronize, cudaGraphAddMemsetNode, cudaGraphsUsingStreamCapture, cudaGraphExecDestroy

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

