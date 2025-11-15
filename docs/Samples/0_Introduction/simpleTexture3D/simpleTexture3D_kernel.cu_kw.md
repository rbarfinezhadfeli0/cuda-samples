# Keywords: Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu
---

**Total Keywords**: 6

---

## _

### _SIMPLETEXTURE3D_KERNEL_CU_ {#simpletexture3dkernelcu}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu](./simpleTexture3D_kernel.cu_docs.md)
- **Context**: `#define _SIMPLETEXTURE3D_KERNEL_CU_

#include <helper_cuda.h>
#include <helper_math.h>
#include <mat`


## C

### cleanupCuda {#cleanupcuda}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu](./simpleTexture3D_kernel.cu_docs.md)
- **Context**: `void cleanupCuda()
{`


## D

### d_render {#drender}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu](./simpleTexture3D_kernel.cu_docs.md)
- **Context**: `__global__ void d_render(`


## I

### initCuda {#initcuda}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu](./simpleTexture3D_kernel.cu_docs.md)
- **Context**: `void initCuda(const uchar *h_volume, cudaExtent volumeSize)
{`


## R

### render_kernel {#renderkernel}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu](./simpleTexture3D_kernel.cu_docs.md)
- **Context**: `void render_kernel(dim3 gridSize, dim3 blockSize, uint *d_output, uint imageW, uint imageH, float w)`


## S

### setTextureFilterMode {#settexturefiltermode}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTexture3D/simpleTexture3D_kernel.cu](./simpleTexture3D_kernel.cu_docs.md)
- **Context**: `void setTextureFilterMode(bool bLinearFilter)
{`

