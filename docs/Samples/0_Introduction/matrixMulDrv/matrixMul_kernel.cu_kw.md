# Keywords: Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu
---

**Total Keywords**: 5

---

## _

### _MATRIXMUL_KERNEL_H_ {#matrixmulkernelh}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu](./matrixMul_kernel.cu_docs.md)
- **Context**: `#define _MATRIXMUL_KERNEL_H_

#include <stdio.h>

#define AS(i, j) As[i][j]
#define BS(i, j) Bs[i][j`


## M

### matrixMul {#matrixmul}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu](./matrixMul_kernel.cu_docs.md)
- **Context**: `void matrixMul(float *C, float *A, float *B, size_type wA, size_type wB)
{`

### matrixMul_bs16_64bit {#matrixmulbs1664bit}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu](./matrixMul_kernel.cu_docs.md)
- **Context**: `__global__ void matrixMul_bs16_64bit(`

### matrixMul_bs32_64bit {#matrixmulbs3264bit}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu](./matrixMul_kernel.cu_docs.md)
- **Context**: `__global__ void matrixMul_bs32_64bit(`

### matrixMul_bs8_64bit {#matrixmulbs864bit}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/matrixMulDrv/matrixMul_kernel.cu](./matrixMul_kernel.cu_docs.md)
- **Context**: `__global__ void matrixMul_bs8_64bit(`

