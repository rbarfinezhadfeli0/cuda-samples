# Keywords: Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu
---

**Total Keywords**: 18

---

## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `                    StopWatchInterface *timer)
{
    // va`


## _

### _BOXFILTER_KERNEL_CH_ {#boxfilterkernelch}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `#define _BOXFILTER_KERNEL_CH_

#include <helper_functions.h>
#include <helper_math.h>

cudaTextureOb`

### __checkCudaErrors {#checkcudaerrors}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `void __checkCudaErrors(cudaError err, const char *file, const int line)
{`


## B

### boxFilter {#boxfilter}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `double boxFilter(float              *d_temp,
                            float              *d_dest,`

### boxFilterRGBA {#boxfilterrgba}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `double boxFilterRGBA(unsigned int       *d_temp,
                                unsigned int       `


## C

### checkCudaErrors {#checkcudaerrors}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `#define checkCudaErrors(err) __checkCudaErrors(err, __FILE__, __LINE__)

inline void __checkCudaErro`


## D

### d_boxfilter_rgba_x {#dboxfilterrgbax}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_boxfilter_rgba_x(`

### d_boxfilter_rgba_y {#dboxfilterrgbay}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_boxfilter_rgba_y(`

### d_boxfilter_x {#dboxfilterx}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `void d_boxfilter_x(float *id, float *od, int w, int h, int r)
{`

### d_boxfilter_x_global {#dboxfilterxglobal}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_boxfilter_x_global(`

### d_boxfilter_x_tex {#dboxfilterxtex}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_boxfilter_x_tex(`

### d_boxfilter_y {#dboxfiltery}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `void d_boxfilter_y(float *id, float *od, int w, int h, int r)
{`

### d_boxfilter_y_global {#dboxfilteryglobal}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_boxfilter_y_global(`

### d_boxfilter_y_tex {#dboxfilterytex}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `__global__ void d_boxfilter_y_tex(`


## F

### freeTextures {#freetextures}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `void freeTextures()
{`


## I

### initTexture {#inittexture}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `void initTexture(int width, int height, void *pImage, bool useRGBA)
{`


## R

### rgbaFloatToInt {#rgbafloattoint}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `int rgbaFloatToInt(float4 rgba)
{`

### rgbaIntToFloat {#rgbainttofloat}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/boxFilter/boxFilter_kernel.cu](./boxFilter_kernel.cu_docs.md)
- **Context**: `float4 rgbaIntToFloat(unsigned int c)
{`

