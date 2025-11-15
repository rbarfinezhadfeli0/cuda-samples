# Keywords: Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu
---

**Total Keywords**: 20

---

## B

### BlockOperation {#blockoperation}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `un are
// shown.
// BlockOperation is an integer kerne`

### BlockWidth {#blockwidth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `#define BlockWidth  80
#define SharedPitch 384
#endif

// A function pointer can be declared explici`


## C

### ComputeBox {#computebox}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `char ComputeBox(unsigned char ul, // upper left
                                    unsigned char um`

### ComputeSobel {#computesobel}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `ice__ unsigned char ComputeSobel(unsigned char ul, /`


## F

### FunctionPointers_kernels {#functionpointerskernels}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `_cuda.h>

#include "FunctionPointers_kernels.h"

// Texture obje`


## L

### LocalBlock {#localblock}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `red__ unsigned char LocalBlock[];
static cudaArray`


## R

### RADIUS {#radius}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `#define RADIUS 1

// pixel value used for thresholding function,
// works well with sample image 'te`


## S

### SharedIdx {#sharedidx}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `       ib;

    int SharedIdx = threadIdx.y * Sha`

### SharedPitch {#sharedpitch}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `#define SharedPitch 384
#endif

// A function pointer can be declared explicitly like this line:
//_`

### SobelCopyImage {#sobelcopyimage}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `__global__ void
SobelCopyImage(`

### SobelDisplayMode {#sobeldisplaymode}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `               enum SobelDisplayMode mode,
             `

### SobelPitch {#sobelpitch}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `     unsigned short SobelPitch,
#ifndef FIXED_BLOC`

### SobelShared {#sobelshared}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `__global__ void SobelShared(`

### SobelTex {#sobeltex}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `__global__ void SobelTex(`


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `#define THRESHOLD 150.0f

#ifdef FIXED_BLOCKWIDTH
#define BlockWidth  80
#define SharedPitch 384
#en`

### Threshold {#threshold}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `char Threshold(unsigned char in, float thresh)
{`


## D

### deleteTexture {#deletetexture}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `void deleteTexture(void)
{`


## S

### setupFunctionTables {#setupfunctiontables}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `void setupFunctionTables()
{`

### setupTexture {#setuptexture}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `void setupTexture(int iw, int ih, Pixel *data, int Bpp)
{`

### sobelFilter {#sobelfilter}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/FunctionPointers/FunctionPointers_kernels.cu](./FunctionPointers_kernels.cu_docs.md)
- **Context**: `void sobelFilter(Pixel                *odata,
                            int                   iw,
`

