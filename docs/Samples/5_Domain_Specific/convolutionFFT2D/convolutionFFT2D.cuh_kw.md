# Keywords: Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh
---

**Total Keywords**: 23

---

## L

### LOAD_FCOMPLEX {#loadfcomplex}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define LOAD_FCOMPLEX(i)   d_Src[i]
#define LOAD_FCOMPLEX_A(i) d_SrcA[i]
#define LOAD_FCOMPLEX_B(i) `

### LOAD_FCOMPLEX_A {#loadfcomplexa}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define LOAD_FCOMPLEX_A(i) d_SrcA[i]
#define LOAD_FCOMPLEX_B(i) d_SrcB[i]

#define SET_FCOMPLEX_BASE`

### LOAD_FCOMPLEX_B {#loadfcomplexb}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define LOAD_FCOMPLEX_B(i) d_SrcB[i]

#define SET_FCOMPLEX_BASE
#define SET_FCOMPLEX_BASE_A
#define `

### LOAD_FLOAT {#loadfloat}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define LOAD_FLOAT(i) d_Src[i]
#define SET_FLOAT_BASE
#endif

#include "convolutionFFT2D_common.h"

`


## P

### POWER_OF_TWO {#poweroftwo}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define POWER_OF_TWO 1

#if (USE_TEXTURE)
#define LOAD_FLOAT(i) tex1Dfetch<float>(texFloat, i)
#defi`


## S

### SET_FCOMPLEX_BASE {#setfcomplexbase}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define SET_FCOMPLEX_BASE
#define SET_FCOMPLEX_BASE_A
#define SET_FCOMPLEX_BASE_B
#endif

inline __d`

### SET_FCOMPLEX_BASE_A {#setfcomplexbasea}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define SET_FCOMPLEX_BASE_A
#define SET_FCOMPLEX_BASE_B
#endif

inline __device__ void spPostprocess`

### SET_FCOMPLEX_BASE_B {#setfcomplexbaseb}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define SET_FCOMPLEX_BASE_B
#endif

inline __device__ void spPostprocessC2C(fComplex &D1, fComplex &`

### SET_FLOAT_BASE {#setfloatbase}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define SET_FLOAT_BASE
#endif

#include "convolutionFFT2D_common.h"

///////////////////////////////`


## U

### USE_TEXTURE {#usetexture}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `#define USE_TEXTURE  1
#define POWER_OF_TWO 1

#if (USE_TEXTURE)
#define LOAD_FLOAT(i) tex1Dfetch<fl`


## F

### factorRadix2 {#factorradix2}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `uint factorRadix2(uint &log2N, uint n)
{`


## G

### getTwiddle {#gettwiddle}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `void getTwiddle(fComplex &twiddle, float phase) {`


## M

### mod {#mod}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `uint mod(uint a, uint DA)
{`

### modulateAndNormalize_kernel {#modulateandnormalizekernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `__global__ void modulateAndNormalize_kernel(`

### mulAndScale {#mulandscale}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `void mulAndScale(fComplex &a, const fComplex &b, const float &c)
{`


## P

### padDataClampToBorder_kernel {#paddataclamptoborderkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `__global__ void padDataClampToBorder_kernel(`

### padKernel_kernel {#padkernelkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `__global__ void padKernel_kernel(`


## S

### spPostprocess2D_kernel {#sppostprocess2dkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `__global__ void spPostprocess2D_kernel(`

### spPostprocessC2C {#sppostprocessc2c}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `void spPostprocessC2C(fComplex &D1, fComplex &D2, const fComplex &twiddle)
{`

### spPreprocess2D_kernel {#sppreprocess2dkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `__global__ void spPreprocess2D_kernel(`

### spPreprocessC2C {#sppreprocessc2c}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `void spPreprocessC2C(fComplex &D1, fComplex &D2, const fComplex &twiddle)
{`

### spProcess2D_kernel {#spprocess2dkernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `__global__ void spProcess2D_kernel(`


## U

### udivmod {#udivmod}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cuh](./convolutionFFT2D.cuh_docs.md)
- **Context**: `void udivmod(uint &dividend, uint divisor, uint &rem)
{`

