# Keywords: Samples/5_Domain_Specific/FDTD3d/inc/FDTD3dGPU.h
---

**Total Keywords**: 5

---

## _

### _FDTD3DGPU_H_ {#fdtd3dgpuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/FDTD3d/inc/FDTD3dGPU.h](./FDTD3dGPU.h_docs.md)
- **Context**: `#define _FDTD3DGPU_H_

#include <cstddef>
#if defined(WIN32) || defined(_WIN32) || defined(WIN64) ||`


## K

### k_blockDimMaxY {#kblockdimmaxy}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/FDTD3d/inc/FDTD3dGPU.h](./FDTD3dGPU.h_docs.md)
- **Context**: `#define k_blockDimMaxY 16
#define k_blockSizeMin 128
#define k_blockSizeMax (k_blockDimX * k_blockDi`

### k_blockDimX {#kblockdimx}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/FDTD3d/inc/FDTD3dGPU.h](./FDTD3dGPU.h_docs.md)
- **Context**: `#define k_blockDimX    32
#define k_blockDimMaxY 16
#define k_blockSizeMin 128
#define k_blockSizeMa`

### k_blockSizeMax {#kblocksizemax}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/FDTD3d/inc/FDTD3dGPU.h](./FDTD3dGPU.h_docs.md)
- **Context**: `#define k_blockSizeMax (k_blockDimX * k_blockDimMaxY)

bool getTargetDeviceGlobalMemSize(memsize_t *`

### k_blockSizeMin {#kblocksizemin}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/FDTD3d/inc/FDTD3dGPU.h](./FDTD3dGPU.h_docs.md)
- **Context**: `#define k_blockSizeMin 128
#define k_blockSizeMax (k_blockDimX * k_blockDimMaxY)

bool getTargetDevi`

