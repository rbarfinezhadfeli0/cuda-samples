# Documentation: Samples/5_Domain_Specific/nbody/README.md
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/nbody/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 2879 bytes
- **Lines**: 39
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```markdown
# nbody - CUDA N-Body Simulation

## Description

This sample demonstrates efficient all-pairs simulation of a gravitational n-body simulation in CUDA.  This sample accompanies the GPU Gems 3 chapter "Fast N-Body Simulation with CUDA".  With CUDA 5.5, performance on Tesla K20c has increased to over 1.8TFLOP/s single precision.  Double Performance has also improved on all Kepler and Fermi GPU architectures as well.  Starting in CUDA 4.0, the nBody sample has been updated to take advantage of new features to easily scale the n-body simulation across multiple GPUs in a single PC.  Adding "-numbodies=<bodies>" to the command line will allow users to set # of bodies for simulation.  Adding “-numdevices=<N>” to the command line option will cause the sample to use N devices (if available) for simulation.  In this mode, the position and velocity data for all bodies are read from system memory using “zero copy” rather than from device memory.  For a small number of devices (4 or fewer) and a large enough number of bodies, bandwidth is not a bottleneck so we can achieve strong scaling across these devices.

## Key Concepts

Graphics Interop, Data Parallel Algorithms, Physically-Based Simulation

## Supported SM Architectures

[SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.9 ](https://developer.nvidia.com/cuda-gpus)  [SM 9.0 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaGraphicsUnmapResources, cudaSetDeviceFlags, cudaGraphicsResourceSetMapFlags, cudaGraphicsResourceGetMappedPointer, cudaGraphicsMapResources, cudaSetDevice, cudaEventSynchronize, cudaGetDeviceCount, cudaGetDeviceProperties, cudaDeviceSynchronize, cudaEventRecord, cudaGetDevice, cudaMemcpyToSymbol, cudaStreamQuery, cudaEventDestroy, cudaEventElapsedTime, cudaDeviceCanAccessPeer, cudaEventCreate

## Dependencies needed to build/run
[X11](../../../README.md#x11), [GL](../../../README.md#gl)

## Prerequisites

Download and install the [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) for your corresponding platform.
Make sure the dependencies mentioned in [Dependencies]() section above are installed.

## References (for more details)

[whitepaper](./doc/nbody_gems3_ch31.pdf)

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

