# Documentation: Samples/5_Domain_Specific/p2pBandwidthLatencyTest/README.md
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/p2pBandwidthLatencyTest/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 1957 bytes
- **Lines**: 33
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```markdown
# p2pBandwidthLatencyTest - Peer-to-Peer Bandwidth Latency Test with Multi-GPUs

## Description

This application demonstrates the CUDA Peer-To-Peer (P2P) data transfers between pairs of GPUs and computes latency and bandwidth.  Tests on GPU pairs using P2P and without P2P are tested.

## Key Concepts

Performance Strategies, Asynchronous Data Transfers, Unified Virtual Address Space, Peer to Peer Data Transfers, Multi-GPU

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaSetDevice, cudaEventDestroy, cudaOccupancyMaxPotentialBlockSize, cudaCheckError, cudaFreeHost, cudaGetDeviceCount, cudaDeviceCanAccessPeer, cudaStreamCreateWithFlags, cudaStreamDestroy, cudaGetLastError, cudaMemset, cudaStreamWaitEvent, cudaEventElapsedTime, cudaEventCreate, cudaHostAlloc, cudaFree, cudaGetErrorString, cudaMemcpyPeerAsync, cudaDeviceDisablePeerAccess, cudaEventRecord, cudaStreamSynchronize, cudaDeviceEnablePeerAccess, cudaMalloc, cudaGetDeviceProperties

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

