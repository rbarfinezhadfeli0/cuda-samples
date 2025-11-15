# Keywords: Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp
---

**Total Keywords**: 25

---

## B

### BENCH_MATRIX_K {#benchmatrixk}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define BENCH_MATRIX_K                (128)
#define BENCH_MATRIX_N                (128)

#define CLE`

### BENCH_MATRIX_M {#benchmatrixm}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define BENCH_MATRIX_M                (128)
#define BENCH_MATRIX_K                (128)
#define BENC`

### BENCH_MATRIX_N {#benchmatrixn}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define BENCH_MATRIX_N                (128)

#define CLEANUP()                          \
    do {  `


## C

### CLEANUP {#cleanup}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define CLEANUP()                          \
    do {                                   \
        if`

### CUBLAS_DGEMM_MAX_RELATIVE_ERR {#cublasdgemmmaxrelativeerr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define CUBLAS_DGEMM_MAX_RELATIVE_ERR (0.0)
#define CUBLAS_GEMM_TEST_COUNT        (30)
#define BENCH`

### CUBLAS_DGEMM_MAX_ULP_ERR {#cublasdgemmmaxulperr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define CUBLAS_DGEMM_MAX_ULP_ERR      (1.e-3)
#define CUBLAS_SGEMM_MAX_RELATIVE_ERR (6.e-6)
#define `

### CUBLAS_GEMM_TEST_COUNT {#cublasgemmtestcount}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define CUBLAS_GEMM_TEST_COUNT        (30)
#define BENCH_MATRIX_M                (128)
#define BENCH`

### CUBLAS_SGEMM_MAX_RELATIVE_ERR {#cublassgemmmaxrelativeerr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define CUBLAS_SGEMM_MAX_RELATIVE_ERR (6.e-6)
#define CUBLAS_DGEMM_MAX_RELATIVE_ERR (0.0)
#define CU`

### CUBLAS_SGEMM_MAX_ULP_ERR {#cublassgemmmaxulperr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define CUBLAS_SGEMM_MAX_ULP_ERR      (.3)
#define CUBLAS_DGEMM_MAX_ULP_ERR      (1.e-3)
#define CUB`


## N

### NBR_ALPHAS {#nbralphas}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define NBR_ALPHAS (sizeof(alpha) / sizeof(alpha[0]))
#define NBR_BETAS  (sizeof(beta) / sizeof(beta`

### NBR_BETAS {#nbrbetas}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `#define NBR_BETAS  (sizeof(beta) / sizeof(beta[0]))
    static T_ELEM theAlpha;
    static T_ELEM th`


## C

### cublasXgemm {#cublasxgemm}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `cublasStatus_t cublasXgemm(cublasHandle_t    handle,
                                         cublas`

### cublasXgemmBatched {#cublasxgemmbatched}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `cublasStatus_t cublasXgemmBatched(cublasHandle_t    handle,
                                        `

### cudaDeviceProp {#cudadeviceprop}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `struct cudaDeviceProp`


## F

### fillupMatrix {#fillupmatrix}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `void fillupMatrix(T_ELEM *A, int lda, int rows, int cols, int seed)
{`

### fillupMatrixDebug {#fillupmatrixdebug}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `void fillupMatrixDebug(T_ELEM *A, int lda, int rows, int cols)
{`


## G

### gemmOpts {#gemmopts}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `struct gemmOpts`

### gemmTestParams {#gemmtestparams}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `struct gemmTestParams`

### getDeviceMemory {#getdevicememory}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `size_t getDeviceMemory(void)
    {`

### getDeviceVersion {#getdeviceversion}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `int getDeviceVersion(void)
    {`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`


## P

### printCuType {#printcutype}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `void printCuType(const char *str, double A) {`

### processArgs {#processargs}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `int processArgs(int argc, char *argv[], struct gemmOpts *opts)
{`


## S

### switch {#switch}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `endif

        switch (ii) {`


## T

### test_gemm_loop {#testgemmloop}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp](./batchCUBLAS.cpp_docs.md)
- **Context**: `int test_gemm_loop(struct gemmOpts &opts, float err, double max_relative_error, cublasHandle_t handl`

