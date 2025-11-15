# Documentation for Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/batchCUBLAS
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

/*
 * This example demonstrates how to get better performance by
 * batching CUBLAS calls with the use of using streams
 */

#include <ctype.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
#include <float.h>
#endif

/* Using updated (v2) interfaces to cublas and cusparse */
#include <cublas_v2.h>
#include <cuda_runtime.h>

// Utilities and system includes
#include <helper_cuda.h>

#include "batchCUBLAS.h"

const char *sSDKname = "batchCUBLAS";

//==============================================================================
// Device information utilities
//==============================================================================

#if defined(__cplusplus)
extern "C"
{
#endif /* __cplusplus */

    int getDeviceVersion(void)
    {
        int                   device;
        struct cudaDeviceProp properties;

        if (cudaGetDevice(&device) != cudaSuccess) {
            printf("failed to get device\n");
            return 0;
        }

        if (cudaGetDeviceProperties(&properties, device) != cudaSuccess) {
            printf("failed to get properties\n");
            return 0;
        }

        return properties.major * 100 + properties.minor * 10;
    }

    size_t getDeviceMemory(void)
    {
        struct cudaDeviceProp properties;
        int                   device;

        if (cudaGetDevice(&device) != cudaSuccess) {
            return 0;
        }

        if (cudaGetDeviceProperties(&properties, device) != cudaSuccess) {
            return 0;
        }

        return properties.totalGlobalMem;
    }
#if defined(__cplusplus)
}
#endif /* __cplusplus */

//==============================================================================
// random utilities
//==============================================================================

template <typename T_ELEM> void fillupMatrix(T_ELEM *A, int lda, int rows, int cols, int seed = 0);

template <typename T_ELEM> void fillupMatrix(T_ELEM *A, int lda, int rows, int cols, int seed)
{
    for (int j = 0; j < cols; j++) {
        for (int i = 0; i < rows; i++) {
            A[i + lda * j] = cuGet<T_ELEM>(((double)(((lda * i + j + seed) % 253) + 1)) / 256.0,
                                           ((double)((((cols * i + j) + 123 + seed) % 253) + 1)) / 256.0);
        }
    }
}
/* Explicit instantiation */
template void fillupMatrix<float>(float *A, int lda, int rows, int cols, int seed);
template void fillupMatrix<double>(double *A, int lda, int rows, int cols, int seed);

/* For debugging */
void printCuType(const char *str, float A) { fprintf(stdout, "%s (0x%08x, %g)", str, floatAsUInt(A), A); }

void printCuType(const char *str, double A) { fprintf(stdout, "%s (0x%016llx, %g)", str, doubleAsULL(A), A); }

//==============================================================================
// defines and structures
//==============================================================================

#define CUBLAS_SGEMM_MAX_ULP_ERR      (.3)
#define CUBLAS_DGEMM_MAX_ULP_ERR      (1.e-3)
#define CUBLAS_SGEMM_MAX_RELATIVE_ERR (6.e-6)
#define CUBLAS_DGEMM_MAX_RELATIVE_ERR (0.0)
#define CUBLAS_GEMM_TEST_COUNT        (30)
#define BENCH_MATRIX_M                (128)
#define BENCH_MATRIX_K                (128)
#define BENCH_MATRIX_N                (128)

#define CLEANUP()                          \
    do {                                   \
        if (A)                             \
            free(A);                       \
        if (B)                             \
            free(B);                       \
        if (C)                             \
            free(C);                       \
        for (int i = 0; i < opts.N; ++i) { \
            if (devPtrA[i])                \
                cudaFree(devPtrA[i]);      \
            if (devPtrB[i])                \
                cudaFree(devPtrB[i]);      \
            if (devPtrC[i])                \
                cudaFree(devPtrC[i]);      \
        }                                  \
        if (devPtrA)                       \
            free(devPtrA);                 \
        if (devPtrB)                       \
            free(devPtrB);                 \
        if (devPtrC)                       \
            free(devPtrC);                 \
        if (devPtrA_dev)                   \
            cudaFree(devPtrA_dev);         \
        if (devPtrB_dev)                   \
            cudaFree(devPtrB_dev);         \
        if (devPtrC_dev)                   \
            cudaFree(devPtrC_dev);         \
        fflush(stdout);                    \
    } while (0)

enum testMethod { tmRegular, tmStream, tmBatched };

struct gemmOpts
{
    int        m;
    int        n;
    int        k;
    testMethod test_method;
    char      *elem_type;
    int        N; // number of multiplications
};

template <typename T_ELEM> struct gemmTestParams
{
    cublasOperation_t transa;
    cublasOperation_t transb;
    int               m;
    int               n;
    int               k;
    T_ELEM            alpha;
    T_ELEM            beta;
};

//==============================================================================
// template wrappers for cuda functions
//==============================================================================

static inline cublasStatus_t cublasXgemm(cublasHandle_t    handle,
                                         cublasOperation_t transa,
                                         cublasOperation_t transb,
                                         int               m,
                                         int               n,
                                         int               k,
                                         float            *alpha,
                                         const float      *A,
                                         int               lda,
                                         float            *B,
                                         int               ldb,
                                         float            *beta,
                                         float            *C,
                                         int               ldc)
{
    return cublasSgemm(handle, transa, transb, m, n, k, alpha, A, lda, B, ldb, beta, C, ldc);
}

static inline cublasStatus_t cublasXgemm(cublasHandle_t    handle,
                                         cublasOperation_t transa,
                                         cublasOperation_t transb,
                                         int               m,
                                         int               n,
                                         int               k,
                                         double           *alpha,
                                         const double     *A,
                                         int               lda,
                                         double           *B,
                                         int               ldb,
                                         double           *beta,
                                         double           *C,
                                         int               ldc)
{
    return cublasDgemm(handle, transa, transb, m, n, k, alpha, A, lda, B, ldb, beta, C, ldc);
}

static inline cublasStatus_t cublasXgemmBatched(cublasHandle_t    handle,
                                                cublasOperation_t transa,
                                                cublasOperation_t transb,
                                                int               m,
                                                int               n,
                                                int               k,
                                                float            *alpha,
                                                const float      *Aarray[],
                                                int               lda,
                                                const float      *Barray[],
                                                int               ldb,
                                                float            *beta,
                                                float            *Carray[],
                                                int               ldc,
                                                int               batchCount)
{
#if CUDART_VERSION >= 4010
    return cublasSgemmBatched(
        handle, transa, transb, m, n, k, alpha, Aarray, lda, Barray, ldb, beta, Carray, ldc, batchCount);
#else
    return CUBLAS_STATUS_SUCCESS;
#endif
}

static inline cublasStatus_t cublasXgemmBatched(cublasHandle_t    handle,
                                                cublasOperation_t transa,
                                                cublasOperation_t transb,
                                                int               m,
                                                int               n,
                                                int               k,
                                                double           *alpha,
                                                const double     *Aarray[],
                                                int               lda,
                                                const double     *Barray[],
                                                int               ldb,
                                                double           *beta,
                                                double           *Carray[],
                                                int               ldc,
                                                int               batchCount)
{
#if CUDART_VERSION >= 4010
    return cublasDgemmBatched(
        handle, transa, transb, m, n, k, alpha, Aarray, lda, Barray, ldb, beta, Carray, ldc, batchCount);
#else
    return CUBLAS_STATUS_SUCCESS;
#endif
}

//==============================================================================
// Primary Application code
//==============================================================================

static int processArgs(int argc, char *argv[], struct gemmOpts *opts)
{
    int error = 0;
    int oldError;
    memset(opts, 0, sizeof(*opts));
    static char default_type[] = "d"; // default double
    opts->elem_type            = default_type;
    opts->N                    = 10;

    while (argc) {
        oldError = error;

        if (*argv[0] == SWITCH_CHAR) {
            switch (*(argv[0] + 1)) {
            case 'm':
                opts->m = (int)atol(argv[0] + 2);
                break;

            case 'n':
                opts->n = (int)atol(argv[0] + 2);
                break;

            case 'k':
                opts->k = (int)atol(argv[0] + 2);
                break;

            case 'N':
                opts->N = (int)atol(argv[0] + 2);
                break;

            default:
                break;
            }
        }

        if (error > oldError) {
            fprintf(stderr, "Invalid switch '%c%s'\n", SWITCH_CHAR, argv[0] + 1);
        }

        argc -= 1;
        argv++;
    }

    return error;
}

template <typename T_ELEM>
static int TESTGEN(gemm)(const struct gemmOpts         *opts,
                         int                            matrixM,
                         int                            matrixN,
                         int                            matrixK,
                         int                           &numTests,
                         struct gemmTestParams<T_ELEM> *params)
{
    static T_ELEM alpha[] = {
        cuGet<T_ELEM>(0, 0), cuGet<T_ELEM>(-1, -1), cuGet<T_ELEM>(1, -2), cuGet<T_ELEM>(2, -1), cuGet<T_ELEM>(0, -3)};
    static T_ELEM beta[] = {
        cuGet<T_ELEM>(0, 0), cuGet<T_ELEM>(-1, -1), cuGet<T_ELEM>(1, -2), cuGet<T_ELEM>(2, -1), cuGet<T_ELEM>(0, -3)};

#define NBR_ALPHAS (sizeof(alpha) / sizeof(alpha[0]))
#define NBR_BETAS  (sizeof(beta) / sizeof(beta[0]))
    static T_ELEM theAlpha;
    static T_ELEM theBeta;
    static int    state;
    static int    m;
    static int    n;
    static int    k;

    if (numTests-- <= 0) {
        return -1;
    }

    theAlpha       = alpha[cuRand() % NBR_ALPHAS];
    theBeta        = beta[cuRand() % NBR_BETAS];
    params->transa = CUBLAS_OP_N;
    params->transb = CUBLAS_OP_N;
    m              = matrixM;
    n              = matrixN;
    k              = matrixK;
    params->m      = m;
    params->n      = n;
    params->k      = k;
    params->alpha  = theAlpha;
    params->beta   = theBeta;

    printf("#### args: ta=%d tb=%d m=%d n=%d k=%d ",
           (unsigned int)params->transa,
           (unsigned int)params->transb,
           params->m,
           params->n,
           params->k);
    printCuType(" alpha =", params->alpha);
    printCuType(" beta=", params->beta);
    printf("\n");

    m = cuRand() % matrixM;
    n = cuRand() % matrixN;
    k = cuRand() % matrixK;

    state = cuRand() % 9;
    return 0;
}

template <typename T_ELEM> void fillupMatrixDebug(T_ELEM *A, int lda, int rows, int cols)
{
    for (int j = 0; j < cols; j++) {
        for (int i = 0; i < rows; i++) {
            A[i + lda * j] = cuGet<T_ELEM>(i + j);
        }
    }
}

template <typename T_ELEM>
int test_gemm_loop(struct gemmOpts &opts, float err, double max_relative_error, cublasHandle_t handle)
{
    struct gemmTestParams<T_ELEM> params;
    cudaStream_t                 *streamArray = 0;
    cublasStatus_t                status1, status2, status3;
    T_ELEM                       *A           = NULL;
    T_ELEM                       *B           = NULL;
    T_ELEM                       *C           = NULL;
    T_ELEM                      **devPtrA     = 0;
    T_ELEM                      **devPtrB     = 0;
    T_ELEM                      **devPtrC     = 0;
    T_ELEM                      **devPtrA_dev = NULL;
    T_ELEM                      **devPtrB_dev = NULL;
    T_ELEM                      **devPtrC_dev = NULL;
    int                           matrixM, matrixN, matrixK;
    int                           rowsA, rowsB, rowsC;
    int                           colsA, colsB, colsC;
    int                           matrixSizeA, matrixSizeB, matrixSizeC;
    int                           errors;
    double                        start, stop;

    printf("Testing %cgemm\n", *opts.elem_type);

    matrixM = (opts.m) ? opts.m : BENCH_MATRIX_M;
    matrixN = (opts.n) ? opts.n : BENCH_MATRIX_N;
    matrixK = (opts.k) ? opts.k : BENCH_MATRIX_K;

    rowsA = imax(1, matrixM);
    colsA = imax(1, matrixK);
    rowsB = imax(1, matrixK);
    colsB = imax(1, matrixN);
    rowsC = imax(1, matrixM);
    colsC = imax(1, matrixN);

    matrixSizeA = rowsA * colsA;
    matrixSizeB = rowsB * colsB;
    matrixSizeC = rowsC * colsC;

    devPtrA = (T_ELEM **)malloc(opts.N * sizeof(*devPtrA));
    devPtrB = (T_ELEM **)malloc(opts.N * sizeof(*devPtrB));
    devPtrC = (T_ELEM **)malloc(opts.N * sizeof(*devPtrC));

    for (int i = 0; i < opts.N; i++) {
        cudaError_t err1 = cudaMalloc((void **)&devPtrA[i], matrixSizeA * sizeof(devPtrA[0][0]));
        cudaError_t err2 = cudaMalloc((void **)&devPtrB[i], matrixSizeB * sizeof(devPtrB[0][0]));
        cudaError_t err3 = cudaMalloc((void **)&devPtrC[i], matrixSizeC * sizeof(devPtrC[0][0]));

        if ((err1 != cudaSuccess) || (err2 != cudaSuccess) || (err3 != cudaSuccess)) {
            CLEANUP();
            fprintf(stderr, "!!!! GPU memory allocation error\n");
            return CUBLASTEST_FAILED;
        }
    }

    // For batched processing we need those arrays on the device
    if (opts.test_method == tmBatched) {
        cudaError_t err1 = cudaMalloc((void **)&devPtrA_dev, opts.N * sizeof(*devPtrA));
        cudaError_t err2 = cudaMalloc((void **)&devPtrB_dev, opts.N * sizeof(*devPtrB));
        cudaError_t err3 = cudaMalloc((void **)&devPtrC_dev, opts.N * sizeof(*devPtrC));

        if ((err1 != cudaSuccess) || (err2 != cudaSuccess) || (err3 != cudaSuccess)) {
            CLEANUP();
            fprintf(stderr, "!!!! GPU memory allocation error\n");
            return CUBLASTEST_FAILED;
        }

        err1 = cudaMemcpy(devPtrA_dev, devPtrA, opts.N * sizeof(*devPtrA), cudaMemcpyHostToDevice);
        err2 = cudaMemcpy(devPtrB_dev, devPtrB, opts.N * sizeof(*devPtrB), cudaMemcpyHostToDevice);
        err3 = cudaMemcpy(devPtrC_dev, devPtrC, opts.N * sizeof(*devPtrC), cudaMemcpyHostToDevice);

        if ((err1 != cudaSuccess) || (err2 != cudaSuccess) || (err3 != cudaSuccess)) {
            CLEANUP();
            fprintf(stderr, "!!!! cannot copy pointer array to device\n");
            return CUBLASTEST_FAILED;
        }
    }

    A = (T_ELEM *)malloc(matrixSizeA * sizeof(A[0]));
    B = (T_ELEM *)malloc(matrixSizeB * sizeof(B[0]));
    C = (T_ELEM *)malloc(matrixSizeC * sizeof(C[0]));

    if ((!A) || (!B) || (!C)) {
        CLEANUP();
        fprintf(stderr, "!!!! system memory allocation error\n");
        return CUBLASTEST_FAILED;
    }

    streamArray = (cudaStream_t *)malloc(opts.N * sizeof(cudaStream_t *));

    for (int i = 0; i < opts.N; i++) {
        if (opts.test_method == tmStream) {
            cudaError_t cudaErr = cudaStreamCreate(&streamArray[i]);

            if (cudaErr != cudaSuccess) {
                CLEANUP();
                fprintf(stderr, "!!!! cannot create stream\n");
                return CUBLASTEST_FAILED;
            }
        }
        else {
            streamArray[i] = 0;
        }
    }

    errors       = 0;
    int numTests = 1;

    while (TESTGEN(gemm)(&opts, matrixM, matrixN, matrixK, numTests, &params) == 0) {
        printf("#### args: lda=%d ldb=%d ldc=%d\n", rowsA, rowsB, rowsC);

        // fillup with Nan first (so lda padding is full on Nan)
        memset(A, 0xFF, matrixSizeA * sizeof(A[0]));
        fillupMatrixDebug(A, rowsA, params.m, params.k);
        memset(B, 0xFF, matrixSizeB * sizeof(B[0]));
        fillupMatrix(B, rowsB, params.k, params.n, 121);

        if (!cuEqual(params.beta, cuGet<T_ELEM>(0))) {
            fillupMatrix(C, rowsC, params.m, params.n);
        }
        else {
            /* fill with SNaNs to make sure ZGEMM doesn't access C */
            memset(C, 0xFF, matrixSizeC * sizeof(C[0]));
        }

        double flopsCoef = 2.0;

        for (int i = 0; i < opts.N; i++) {
            status1 = cublasSetMatrix(rowsA, colsA, sizeof(A[0]), A, rowsA, devPtrA[i], rowsA);
            status2 = cublasSetMatrix(rowsB, colsB, sizeof(B[0]), B, rowsB, devPtrB[i], rowsB);
            status3 = cublasSetMatrix(rowsC, colsC, sizeof(C[0]), C, rowsC, devPtrC[i], rowsC);

            if ((status1 != CUBLAS_STATUS_SUCCESS) || (status2 != status1) || (status3 != status1)) {
                CLEANUP();
                fprintf(stderr, "!!!! GPU access error (write)\n");
                return CUBLASTEST_FAILED;
            }
        }

        start = second();

        if (opts.test_method == tmBatched) {
            cublasSetStream(handle, streamArray[0]);
            status1 = cublasXgemmBatched(handle,
                                         params.transa,
                                         params.transb,
                                         params.m,
                                         params.n,
                                         params.k,
                                         &params.alpha,
                                         (const T_ELEM **)devPtrA_dev,
                                         rowsA,
                                         (const T_ELEM **)devPtrB_dev,
                                         rowsB,
                                         &params.beta,
                                         devPtrC_dev,
                                         rowsC,
                                         opts.N);

            if (status1 != CUBLAS_STATUS_SUCCESS) {
                cudaError_t cudaStatus = cudaGetLastError();
                CLEANUP();
                fprintf(stderr,
                        "!!!! GPU program execution error : cublas Error=%d, cuda "
                        "Error=%d,(%s)\n",
                        status1,
                        cudaStatus,
                        cudaGetErrorString(cudaStatus));
                return CUBLASTEST_FAILED;
            }
        }
        else {
            for (int i = 0; i < opts.N; i++) {
                cublasSetStream(handle, streamArray[i]);
                status1 = cublasXgemm(handle,
                                      params.transa,
                                      params.transb,
                                      params.m,
                                      params.n,
                                      params.k,
                                      &params.alpha,
                                      devPtrA[i],
                                      rowsA,
                                      devPtrB[i],
                                      rowsB,
                                      &params.beta,
                                      devPtrC[i],
                                      rowsC);

                if (status1 != CUBLAS_STATUS_SUCCESS) {
                    cudaError_t cudaStatus = cudaGetLastError();
                    CLEANUP();
                    fprintf(stderr,
                            "!!!! GPU program execution error : cublas Error=%d, cuda "
                            "Error=%d,(%s)\n",
                            status1,
                            cudaStatus,
                            cudaGetErrorString(cudaStatus));
                    return CUBLASTEST_FAILED;
                }
            }
        }

        cudaError_t cudaStatus = cudaDeviceSynchronize();

        if (cudaStatus != cudaSuccess) {
            CLEANUP();
            fprintf(stderr,
                    "!!!! GPU program execution error on cudaDeviceSynchronize : "
                    "cudaError=%d,(%s)\n",
                    cudaStatus,
                    cudaGetErrorString(cudaStatus));
            return CUBLASTEST_FAILED;
        }

        stop = second();

        fprintf(stdout,
                "^^^^ elapsed = %10.8f sec  GFLOPS=%g\n",
                (stop - start),
                opts.N * (1e-9 * flopsCoef * params.m * params.n * params.k) / (stop - start));

    } // end while (TESTGEN..

    CLEANUP();
    fprintf(stdout, "@@@@ %cgemm test %s\n", *opts.elem_type, errors ? "FAIL" : "OK");
    return CUBLASTEST_PASSED;
}

int main(int argc, char *argv[])
{
    struct gemmOpts opts;
    int             errors, nTimes, nTotalErrors = 0;
    int             status = CUBLASTEST_PASSED;

    printf("%s Starting...\n\n", sSDKname);

    int dev = findCudaDevice(argc, (const char **)argv);

    if (dev == -1) {
        return CUBLASTEST_FAILED;
    }

    errors = processArgs(argc, argv, &opts);

    if (errors) {
        fprintf(stdout,
                "\n Usage: batchcublas [-mSIZE_M] [-nSIZE_N] [-kSIZE_N] "
                "[-NSIZE_NUM_ITERATIONS] [-qatest] [-noprompt]\n");
        return CUBLASTEST_FAILED;
    }

    cublasHandle_t handle;

    if (cublasCreate(&handle) != CUBLAS_STATUS_SUCCESS) {
        fprintf(stdout, "CUBLAS initialization failed!\n");
        exit(EXIT_FAILURE);
    }

    // Run single kernels
    fprintf(stdout, "\n ==== Running single kernels ==== \n\n");
    nTimes            = opts.N;
    opts.N            = 1;
    *(opts.elem_type) = 's';
    status =
        test_gemm_loop<float>(opts, (float)CUBLAS_SGEMM_MAX_ULP_ERR, (double)CUBLAS_SGEMM_MAX_RELATIVE_ERR, handle);

    // Run Double version
    *(opts.elem_type) = 'd';

    if (getDeviceVersion() < DEV_VER_DBL_SUPPORT) {
        fprintf(stdout, "@@@@ dgemm test WAIVED due to lack of DP support\n");
        exit(EXIT_WAIVED);
    }

    status =
        test_gemm_loop<double>(opts, (float)CUBLAS_DGEMM_MAX_ULP_ERR, (double)CUBLAS_DGEMM_MAX_RELATIVE_ERR, handle);
    nTotalErrors += (status == CUBLASTEST_PASSED ? 0 : 1);
    opts.N = nTimes;

    // Run with and without streams and then batched. The batched functions are a
    // feature new feature in 4.1
#if CUDART_VERSION >= 4010

    for (int ii = 0; ii < 3; ii++) {
#else

    for (int ii = 0; ii < 2; ii++) {
#endif

        switch (ii) {
        case 0:
            opts.test_method = tmRegular;
            fprintf(stdout, "\n ==== Running N=%d without streams ==== \n\n", opts.N);
            break;

        case 1:
            opts.test_method = tmStream;
            fprintf(stdout, "\n ==== Running N=%d with streams ==== \n\n", opts.N);
            break;

        case 2:
            opts.test_method = tmBatched;
            fprintf(stdout, "\n ==== Running N=%d batched ==== \n\n", opts.N);
            break;
        }

        // Run single version
        *(opts.elem_type) = 's';
        status =
            test_gemm_loop<float>(opts, (float)CUBLAS_SGEMM_MAX_ULP_ERR, (double)CUBLAS_SGEMM_MAX_RELATIVE_ERR, handle);
        nTotalErrors += (status == CUBLASTEST_PASSED ? 0 : 1);

        // Run Double version
        *(opts.elem_type) = 'd';

        // Test doesn't meet minSpec, will will wave the DP test
        if (getDeviceVersion() < DEV_VER_DBL_SUPPORT) {
            fprintf(stdout, "@@@@ dgemm test WAIVED due to lack of DP support\n");
            exit(EXIT_WAIVED);
        }
        else {
            status = test_gemm_loop<double>(
                opts, (float)CUBLAS_DGEMM_MAX_ULP_ERR, (double)CUBLAS_DGEMM_MAX_RELATIVE_ERR, handle);
            nTotalErrors += (status == CUBLASTEST_PASSED ? 0 : 1);
        }
    }

    cublasDestroy(handle);

    printf("\nTest Summary\n");
    printf("%d error(s)\n", nTotalErrors);
    exit(nTotalErrors == 0 ? EXIT_SUCCESS : EXIT_FAILURE);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/batchCUBLAS/batchCUBLAS.cpp`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 728
- **Approximate Size**: 27405 bytes

### Content Structure

#### Functions and Kernels

This file contains function definitions and potentially CUDA kernel launches.
Functions in this file handle:

- **Initialization**: Setting up CUDA context and allocating resources
- **Computation**: Core algorithmic implementations
- **Cleanup**: Freeing resources and error checking

#### Error Handling

The code implements error handling through:

- CUDA error checking macros
- Return code validation
- Exception handling where appropriate

#### Memory Management

Memory operations include:

- Device memory allocation (cudaMalloc)
- Host memory allocation
- Memory transfers (cudaMemcpy)
- Proper cleanup and deallocation

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

### Computational Complexity

The algorithms in this file are designed with performance in mind:

- **GPU Parallelism**: Leveraging thousands of CUDA cores
- **Memory Bandwidth**: Optimizing data transfer patterns
- **Occupancy**: Maximizing GPU utilization
- **Latency Hiding**: Using asynchronous operations where beneficial

### Optimization Opportunities

Potential areas for optimization:

1. Kernel launch configuration tuning
2. Shared memory usage
3. Coalesced memory access
4. Reduction of host-device transfers

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

To test this file:

1. Build the sample using CMake
2. Run the executable with appropriate parameters
3. Verify output against expected results
4. Check for memory leaks using cuda-memcheck
5. Profile performance using NVIDIA profiling tools

### Integration Tests

This file is tested as part of the overall sample application, ensuring:

- Correct functionality
- Expected performance characteristics
- Compatibility across different GPU architectures

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Running

```bash
./{executable_name} [options]
```

Refer to the sample's README for specific command-line options and usage patterns.

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
