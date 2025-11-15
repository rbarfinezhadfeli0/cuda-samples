# Documentation: Samples/6_Performance/cudaGraphsPerfScaling/dataCollection.bash
---
## File Metadata
- **Path**: `Samples/6_Performance/cudaGraphsPerfScaling/dataCollection.bash`
- **Filename**: `dataCollection.bash`
- **Language**: text
- **Size**: 465 bytes
- **Lines**: 18
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
GPU=$1
DRIVER_VERSION=$2
BINARY=./cudaGraphsPerfScaling
datadir=PERF_DATA

suffix=$DRIVER_VERSION
prefix=$GPU
mkdir -p $datadir

trials=600

width=1
nvidia-smi > $datadir/${prefix}_info_${suffix}.txt
$BINARY 5 $trials 1 $width 0 1 256 > $datadir/${prefix}_${width}_small_${suffix}.csv
$BINARY 5 $trials 1 $width 0 32 2048 > $datadir/${prefix}_${width}_large_${suffix}.csv
width=4
$BINARY 5 $trials 1 $width 0 1 256 > $datadir/${prefix}_${width}_small_${suffix}.csv

```

---
## High-Level Overview
This file is a text source file in the CUDA Samples repository.


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

