# Keywords: Samples/5_Domain_Specific/volumeFiltering/volumeRender.h
---

**Total Keywords**: 7

---

## V

### VolumeRender_copyInvViewMatrix {#volumerendercopyinvviewmatrix}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: `ct_t tex);
    void VolumeRender_copyInvViewMatrix(float *invViewMatri`

### VolumeRender_deinit {#volumerenderdeinit}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: `er_init();
    void VolumeRender_deinit();

    void Volume`

### VolumeRender_init {#volumerenderinit}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: `tern "C"
{
    void VolumeRender_init();
    void VolumeR`

### VolumeRender_render {#volumerenderrender}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: ` *volume);
    void VolumeRender_render(dim3               `

### VolumeRender_setPreIntegrated {#volumerendersetpreintegrated}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: `deinit();

    void VolumeRender_setPreIntegrated(int state);
    voi`

### VolumeRender_setTextureFilterMode {#volumerendersettexturefiltermode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: `nt state);
    void VolumeRender_setTextureFilterMode(bool bLinearFilter,`


## _

### _VOLUMERENDER__H_ {#volumerenderh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender.h](./volumeRender.h_docs.md)
- **Context**: `#define _VOLUMERENDER__H_

#include <cuda_runtime.h>

#include "volume.h"

extern "C"
{
    void Vol`

