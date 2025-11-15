# Keywords: Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu
---

**Total Keywords**: 16

---

## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `#define MAX(a, b) ((a > b) ? a : b)
#endif

////////////////////////////////////////////////////////`

### MipMap {#mipmap}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `////////////////
// MipMap Generation

//  A k`


## S

### SHOW_MIPMAPS {#showmipmaps}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `#define SHOW_MIPMAPS

// local references to resources

Image              atlasImage;
std::vector<I`


## _

### _BINDLESSTEXTURE_KERNEL_CU_ {#bindlesstexturekernelcu}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `#define _BINDLESSTEXTURE_KERNEL_CU_

#include <helper_cuda.h>
#include <helper_math.h>
#include <mat`


## D

### d_mipmap {#dmipmap}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `__global__ void d_mipmap(`

### d_render {#drender}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `__global__ void d_render(`

### decodeTextureObject {#decodetextureobject}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `cudaTextureObject_t decodeTextureObject(uint2 obj)
{`

### deinitAtlasAndImages {#deinitatlasandimages}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `void deinitAtlasAndImages()
{`


## E

### encodeTextureObject {#encodetextureobject}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `uint2 encodeTextureObject(cudaTextureObject_t obj)
{`


## G

### generateMipMaps {#generatemipmaps}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `void generateMipMaps(cudaMipmappedArray_t mipmapArray, cudaExtent size)
{`

### getMipMapLevels {#getmipmaplevels}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `uint getMipMapLevels(cudaExtent size)
{`


## I

### initAtlasAndImages {#initatlasandimages}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `void initAtlasAndImages(const Image *images, size_t numImages, cudaExtent atlasSize)
{`


## R

### randomizeAtlas {#randomizeatlas}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `void randomizeAtlas()
{`

### renderAtlasImage {#renderatlasimage}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `void renderAtlasImage(dim3 gridSize, dim3 blockSize, uchar4 *d_output, uint imageW, uint imageH, flo`


## T

### to_float4 {#tofloat4}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `float4 to_float4(uchar4 vec) {`

### to_uchar4 {#touchar4}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture_kernel.cu](./bindlessTexture_kernel.cu_docs.md)
- **Context**: `uchar4 to_uchar4(float4 vec)
{`

