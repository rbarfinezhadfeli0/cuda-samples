# Keywords: Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu
---

**Total Keywords**: 6

---

## V

### VolumeFilter_runFilter {#volumefilterrunfilter}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu](./volumeFilter_kernel.cu_docs.md)
- **Context**: `
extern "C" Volume *VolumeFilter_runFilter(Volume *input,
    `

### VolumeType {#volumetype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu](./volumeFilter_kernel.cu_docs.md)
- **Context**: `filter_offset;

    VolumeType output = VolumeType`

### VolumeTypeInfo {#volumetypeinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu](./volumeFilter_kernel.cu_docs.md)
- **Context**: `VolumeType output = VolumeTypeInfo<VolumeType>::conver`


## _

### _VOLUMEFILTER_KERNEL_CU_ {#volumefilterkernelcu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu](./volumeFilter_kernel.cu_docs.md)
- **Context**: `#define _VOLUMEFILTER_KERNEL_CU_

#include <helper_cuda.h>
#include <helper_math.h>

#include "volum`


## D

### d_filter_surface3d {#dfiltersurface3d}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu](./volumeFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_filter_surface3d(`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFilter_kernel.cu](./volumeFilter_kernel.cu_docs.md)
- **Context**: `int iDivUp(size_t a, size_t b)
{`

