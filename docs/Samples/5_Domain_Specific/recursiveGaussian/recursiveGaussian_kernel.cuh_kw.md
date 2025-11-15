# Keywords: Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh
---

**Total Keywords**: 8

---

## B

### BLOCK_DIM {#blockdim}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `#define BLOCK_DIM     16
#define CLAMP_TO_EDGE 1

// Transpose kernel (see transpose CUDA Sample for`


## C

### CLAMP_TO_EDGE {#clamptoedge}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `#define CLAMP_TO_EDGE 1

// Transpose kernel (see transpose CUDA Sample for details)
__global__ void`


## _

### _RECURSIVEGAUSSIAN_KERNEL_CU_ {#recursivegaussiankernelcu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `#define _RECURSIVEGAUSSIAN_KERNEL_CU_

#include <cooperative_groups.h>
#include <stdio.h>
#include <`


## D

### d_recursiveGaussian_rgba {#drecursivegaussianrgba}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `__global__ void d_recursiveGaussian_rgba(`

### d_simpleRecursive_rgba {#dsimplerecursivergba}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `__global__ void d_simpleRecursive_rgba(`

### d_transpose {#dtranspose}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `__global__ void d_transpose(`


## R

### rgbaFloatToInt {#rgbafloattoint}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `uint rgbaFloatToInt(float4 rgba)
{`

### rgbaIntToFloat {#rgbainttofloat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/recursiveGaussian/recursiveGaussian_kernel.cuh](./recursiveGaussian_kernel.cuh_docs.md)
- **Context**: `float4 rgbaIntToFloat(uint c)
{`

