# Documentation: Samples/3_CUDA_Features/jacobiCudaGraphs/README.md
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/jacobiCudaGraphs/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1116 bytes
- **Lines**: 31
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```markdown
# jacobiCudaGraphs - Jacobi CUDA Graphs

## Description

Demonstrates Instantiated CUDA Graph Update with Jacobi Iterative Method using cudaGraphExecKernelNodeSetParams() and cudaGraphExecUpdate() approach.

## Key Concepts

CUDA Graphs, Stream Capture, Instantiated CUDA Graph Update, Cooperative Groups

## Supported SM Architectures

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaExtent, cudaGraphLaunch, cudaGraphAddMemcpyNode, cudaMallocHost, cudaPitchedPtr, cudaStreamEndCapture, cudaGraphCreate, cudaFreeHost, cudaMemsetAsync, cudaMemcpyAsync, cudaGraphExecKernelNodeSetParams, cudaStreamCreateWithFlags, cudaGraphInstantiate, cudaStreamBeginCapture, cudaFree, cudaGraphExecUpdate, cudaGraphAddKernelNode, cudaPos, cudaStreamSynchronize, cudaGraphAddMemsetNode, cudaMalloc, cudaGraphExecDestroy

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

