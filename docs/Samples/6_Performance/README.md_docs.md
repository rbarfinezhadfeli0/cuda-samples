# Documentation: Samples/6_Performance/README.md
---
## File Metadata
- **Path**: `Samples/6_Performance/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 874 bytes
- **Lines**: 15
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```markdown
# 6. Performance


### [alignedTypes](./alignedTypes)
A simple test, showing huge access speed gap between aligned and misaligned structures. It measures per-element copy throughput for aligned and misaligned structures on big chunks of data.

### [transpose](./transpose)
This sample demonstrates Matrix Transpose.  Different performance are shown to achieve high performance.

### [UnifiedMemoryPerf](./UnifiedMemoryPerf)
This sample demonstrates the performance comparision using matrix multiplication kernel of Unified Memory with/without hints and other types of memory like zero copy buffers, pageable, pagelocked memory performing synchronous and Asynchronous transfers on a single GPU.

### [cudaGraphsPerfScaling](./cudaGraphsPerfScaling)
This sample demonstrates the performance characteristics of cuda graphs. It is focused on how the apis scale with graph size.

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

