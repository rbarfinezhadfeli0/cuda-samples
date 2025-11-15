# Keywords: Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu
---

**Total Keywords**: 13

---

## H

### HyperGraph {#hypergraph}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `education/materials/HyperGraph/raytrace/rtinter3.h`


## R

### Ray {#ray}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `struct Ray`


## V

### VolumeType {#volumetype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `pedef unsigned char VolumeType;
// typedef unsigne`


## _

### _VOLUMERENDER_KERNEL_CU_ {#volumerenderkernelcu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `#define _VOLUMERENDER_KERNEL_CU_

#include <helper_cuda.h>
#include <helper_math.h>

typedef unsigne`


## C

### copyInvViewMatrix {#copyinvviewmatrix}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void copyInvViewMatrix(float *invViewMatrix, size_t sizeofMatrix)
{`


## D

### d_render {#drender}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `__global__ void d_render(`


## F

### freeCudaBuffers {#freecudabuffers}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void freeCudaBuffers()
{`


## I

### initCuda {#initcuda}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void initCuda(void *h_volume, cudaExtent volumeSize)
{`

### intersectBox {#intersectbox}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `int intersectBox(Ray r, float3 boxmin, float3 boxmax, float *tnear, float *tfar)
{`


## M

### mul {#mul}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `float4 mul(const float3x4 &M, const float4 &v)
{`


## R

### render_kernel {#renderkernel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void render_kernel(dim3  gridSize,
                              dim3  blockSize,
                  `

### rgbaFloatToInt {#rgbafloattoint}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `uint rgbaFloatToInt(float4 rgba)
{`


## S

### setTextureFilterMode {#settexturefiltermode}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeRender/volumeRender_kernel.cu](./volumeRender_kernel.cu_docs.md)
- **Context**: `void setTextureFilterMode(bool bLinearFilter)
{`

