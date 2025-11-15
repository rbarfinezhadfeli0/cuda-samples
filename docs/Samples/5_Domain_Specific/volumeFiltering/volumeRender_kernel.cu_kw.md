# Keywords: Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu
---

**Total Keywords**: 26

---

## H

### HyperGraph {#hypergraph}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `education/materials/HyperGraph/raytrace/rtinter3.h`


## R

### Ray {#ray}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `struct Ray`


## V

### VOLUMERENDER_TFS {#volumerendertfs}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `#define VOLUMERENDER_TFS            2
#define VOLUMERENDER_TF_PREINTSIZE  1024
#define VOLUMERENDER_`

### VOLUMERENDER_TF_PREINTRAY {#volumerendertfpreintray}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `#define VOLUMERENDER_TF_PREINTRAY   4

enum TFMode {
    TF_SINGLE_1D         = 0, // single 1D TF f`

### VOLUMERENDER_TF_PREINTSIZE {#volumerendertfpreintsize}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `#define VOLUMERENDER_TF_PREINTSIZE  1024
#define VOLUMERENDER_TF_PREINTSTEPS 1024
#define VOLUMEREND`

### VOLUMERENDER_TF_PREINTSTEPS {#volumerendertfpreintsteps}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `#define VOLUMERENDER_TF_PREINTSTEPS 1024
#define VOLUMERENDER_TF_PREINTRAY   4

enum TFMode {
    TF`

### VolumeRender_copyInvViewMatrix {#volumerendercopyinvviewmatrix}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_copyInvViewMatrix(float *invViewMatrix, size_t sizeofMatrix)
{`

### VolumeRender_deinit {#volumerenderdeinit}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_deinit()
{`

### VolumeRender_init {#volumerenderinit}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_init()
{`

### VolumeRender_render {#volumerenderrender}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_render(dim3                gridSize,
                         dim3                `

### VolumeRender_setPreIntegrated {#volumerendersetpreintegrated}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_setPreIntegrated(int state) {`

### VolumeRender_setTextureFilterMode {#volumerendersettexturefiltermode}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_setTextureFilterMode(bool bLinearFilter, Volume *vol)
{`

### VolumeRender_updateTF {#volumerenderupdatetf}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void VolumeRender_updateTF(int tfIdx, int numColors, float4 *colors)
{`

### VolumeType {#volumetype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `de = VolumeTypeInfo<VolumeType>::readMode;

    ch`

### VolumeTypeInfo {#volumetypeinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `texDescr.readMode = VolumeTypeInfo<VolumeType>::readMo`


## _

### _VOLUMERENDER_KERNEL_CU_ {#volumerenderkernelcu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `#define _VOLUMERENDER_KERNEL_CU_

#include <helper_cuda.h>
#include <helper_math.h>

#include "volum`


## D

### d_integrate_trapezoidal {#dintegratetrapezoidal}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `__global__ void
d_integrate_trapezoidal(`

### d_preintegrate {#dpreintegrate}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `__global__ void d_preintegrate(`

### d_render {#drender}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void d_render(uint               *d_output,
                         uint                imageW,
   `

### d_render_preint {#drenderpreint}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `__global__ void d_render_preint(`

### d_render_preint_off {#drenderpreintoff}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `__global__ void d_render_preint_off(`

### d_render_regular {#drenderregular}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `__global__ void d_render_regular(`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `int iDivUp(size_t a, size_t b)
{`

### intersectBox {#intersectbox}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `int intersectBox(Ray r, float3 boxmin, float3 boxmax, float *tnear, float *tfar)
{`


## M

### mul {#mul}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `float4 mul(const float3x4 &M, const float4 &v)
{`


## R

### rgbaFloatToInt {#rgbafloattoint}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `uint rgbaFloatToInt(float4 rgba)
{`

