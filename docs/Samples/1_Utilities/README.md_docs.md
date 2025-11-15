# Documentation: Samples/1_Utilities/README.md
---
## File Metadata
- **Path**: `Samples/1_Utilities/README.md`
- **Filename**: `README.md`
- **Language**: markdown
- **Size**: 669 bytes
- **Lines**: 16
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```markdown
# 1. Utilities

### [deviceQuery](./deviceQuery)
This sample enumerates the properties of the CUDA devices present in the system.

### [deviceQueryDrv](./deviceQueryDrv)
This sample enumerates the properties of the CUDA devices present using CUDA Driver API calls

### [topologyQuery](./topologyQuery)
A simple example on how to query the topology of a system with multiple GPU

## Note

### bandwidthTest
The bandwidthTest sample was out-of-date and has been removed as of the CUDA Samples 12.9 release (see the [change log](../../CHANGELOG.md)). For up-to-date bandwidth measurements, refer instead to the [NVBandwith](https://github.com/nvidia/nvbandwidth) utility.

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

