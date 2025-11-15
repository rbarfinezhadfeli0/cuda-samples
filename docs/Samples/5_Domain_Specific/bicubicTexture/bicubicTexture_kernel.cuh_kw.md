# Keywords: Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh
---

**Total Keywords**: 16

---

## _

### _BICUBICTEXTURE_KERNEL_CUH_ {#bicubictexturekernelcuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `#define _BICUBICTEXTURE_KERNEL_CUH_

enum Mode { MODE_NEAREST, MODE_BILINEAR, MODE_BICUBIC, MODE_FAS`


## C

### catRomFilter {#catromfilter}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `T catRomFilter(float x, T c0, T c1, T c2, T c3)
{`

### catrom_w0 {#catromw0}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `float catrom_w0(float a)
{`

### catrom_w1 {#catromw1}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `float catrom_w1(float a)
{`

### catrom_w2 {#catromw2}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `float catrom_w2(float a)
{`

### catrom_w3 {#catromw3}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `float catrom_w3(float a)
{`

### cubicFilter {#cubicfilter}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `T cubicFilter(float x, T c0, T c1, T c2, T c3)
{`


## D

### d_render {#drender}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `__global__ void d_render(`

### d_renderBicubic {#drenderbicubic}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `__global__ void d_renderBicubic(`

### d_renderCatRom {#drendercatrom}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `__global__ void d_renderCatRom(`

### d_renderFastBicubic {#drenderfastbicubic}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `__global__ void d_renderFastBicubic(`


## T

### tex2DBicubic {#tex2dbicubic}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `R tex2DBicubic(const cudaTextureObject_t tex, float x, float y)
{`

### tex2DBilinear {#tex2dbilinear}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `R tex2DBilinear(const cudaTextureObject_t tex, float x, float y)
{`

### tex2DBilinearGather {#tex2dbilineargather}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `float tex2DBilinearGather(const cudaTextureObject_t tex, float x, float y, int comp = 0)
{`

### tex2DCatRom {#tex2dcatrom}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `R tex2DCatRom(const cudaTextureObject_t tex, float x, float y)
{`

### tex2DFastBicubic {#tex2dfastbicubic}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bicubicTexture/bicubicTexture_kernel.cuh](./bicubicTexture_kernel.cuh_docs.md)
- **Context**: `R tex2DFastBicubic(const cudaTextureObject_t tex, float x, float y)
{`

