# Keywords: Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp
---

**Total Keywords**: 9

---

## _

### _matrixSize {#matrixsize}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `struct _matrixSize`


## I

### initializeCUDA {#initializecuda}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `void initializeCUDA(int argc, char **argv, int &devID, int &iSizeMultiple, sMatrixSize &matrix_size)`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### matrixMulCPU {#matrixmulcpu}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `void matrixMulCPU(float *C, const float *A, const float *B, unsigned int hA, unsigned int wA, unsign`

### matrixMultiply {#matrixmultiply}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `int matrixMultiply(int argc, char **argv, int devID, sMatrixSize &matrix_size)
{`

### max {#max}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `#define max(a, b) ((a > b) ? a : b)
#endif

// Optional Command-line multiplier for matrix sizes
typ`

### min {#min}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `#define min(a, b) ((a < b) ? a : b)
#endif
#ifndef max
#define max(a, b) ((a > b) ? a : b)
#endif

/`


## P

### printDiff {#printdiff}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `void printDiff(float *data1, float *data2, int width, int height, int iListLength, float fListTol)
{`


## R

### randomInit {#randominit}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/matrixMulCUBLAS/matrixMulCUBLAS.cpp](./matrixMulCUBLAS.cpp_docs.md)
- **Context**: `void randomInit(float *data, int size)
{`

