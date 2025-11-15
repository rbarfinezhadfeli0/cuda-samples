# Keywords: Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp
---

**Total Keywords**: 15

---

## B

### BATCH_SIZE {#batchsize}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `#define BATCH_SIZE 10000

// use double precision data type
#define DOUBLE_PRECISION /* comment this`


## D

### DATA_TYPE {#datatype}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `#define DATA_TYPE float
#define MAX_ERROR 1e-6
#endif /* DOUBLE_PRCISION */

// use pivot vector whi`

### DOUBLE_PRECISION {#doubleprecision}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `#define DOUBLE_PRECISION /* comment this to use single precision */
#ifdef DOUBLE_PRECISION
#define `


## M

### MAX_ERROR {#maxerror}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `#define MAX_ERROR 1e-6
#endif /* DOUBLE_PRCISION */

// use pivot vector while decomposing
#define P`


## P

### PIVOT {#pivot}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `#define PIVOT /* comment this to disable pivot use */

// helper functions

// wrapper around cublas`


## C

### checkRelativeError {#checkrelativeerror}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `bool checkRelativeError(DATA_TYPE *mat1, DATA_TYPE *mat2, DATA_TYPE maxError)
{`

### cublasXgetrfBatched {#cublasxgetrfbatched}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `cublasStatus_t
cublasXgetrfBatched(cublasHandle_t handle, int n, DATA_TYPE *const A[], int lda, int `


## G

### getLUdecoded {#getludecoded}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void getLUdecoded(DATA_TYPE *mat, DATA_TYPE *L, DATA_TYPE *U)
{`

### getPmatFromPivot {#getpmatfrompivot}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void getPmatFromPivot(DATA_TYPE *Pmat, int *P)
{`


## I

### initIdentityMatrix {#initidentitymatrix}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void initIdentityMatrix(DATA_TYPE *mat)
{`

### initRandomMatrix {#initrandommatrix}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void initRandomMatrix(DATA_TYPE *mat)
{`

### initZeroMatrix {#initzeromatrix}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void initZeroMatrix(DATA_TYPE *mat) {`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### matrixMultiply {#matrixmultiply}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void matrixMultiply(DATA_TYPE *res, DATA_TYPE *mat1, DATA_TYPE *mat2)
{`


## P

### printMatrix {#printmatrix}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUBLAS_LU/simpleCUBLAS_LU.cpp](./simpleCUBLAS_LU.cpp_docs.md)
- **Context**: `void printMatrix(DATA_TYPE *mat)
{`

