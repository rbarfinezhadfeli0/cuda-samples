# Keywords: Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu
---

**Total Keywords**: 17

---

## B

### BlockWidth {#blockwidth}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `#define BlockWidth  80
#define SharedPitch 384
#endif

// This will output the proper CUDA error str`


## C

### ComputeSobel {#computesobel}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `ice__ unsigned char ComputeSobel(unsigned char ul, /`


## L

### LocalBlock {#localblock}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `red__ unsigned char LocalBlock[];
static cudaArray`


## R

### RADIUS {#radius}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `#define RADIUS 1

#ifdef FIXED_BLOCKWIDTH
#define BlockWidth  80
#define SharedPitch 384
#endif

// `


## S

### SharedIdx {#sharedidx}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `       ib;

    int SharedIdx = threadIdx.y * Sha`

### SharedPitch {#sharedpitch}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `#define SharedPitch 384
#endif

// This will output the proper CUDA error strings in the event that `

### SobelCopyImage {#sobelcopyimage}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `__global__ void
SobelCopyImage(`

### SobelDisplayMode {#sobeldisplaymode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `nt iw, int ih, enum SobelDisplayMode mode, float fScale)`

### SobelFilter_kernels {#sobelfilterkernels}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `tring.h>

#include "SobelFilter_kernels.h"

// Texture obje`

### SobelPitch {#sobelpitch}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `     unsigned short SobelPitch,
#ifndef FIXED_BLOC`

### SobelShared {#sobelshared}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `__global__ void SobelShared(`

### SobelTex {#sobeltex}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `__global__ void SobelTex(`


## _

### __checkCudaErrors {#checkcudaerrors}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `void __checkCudaErrors(cudaError err, const char *file, const int line)
{`


## C

### checkCudaErrors {#checkcudaerrors}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `#define checkCudaErrors(err) __checkCudaErrors(err, __FILE__, __LINE__)

inline void __checkCudaErro`


## D

### deleteTexture {#deletetexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `void deleteTexture(void)
{`


## S

### setupTexture {#setuptexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `void setupTexture(int iw, int ih, Pixel *data, int Bpp)
{`

### sobelFilter {#sobelfilter}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/SobelFilter/SobelFilter_kernels.cu](./SobelFilter_kernels.cu_docs.md)
- **Context**: `void sobelFilter(Pixel *odata, int iw, int ih, enum SobelDisplayMode mode, float fScale)
{`

