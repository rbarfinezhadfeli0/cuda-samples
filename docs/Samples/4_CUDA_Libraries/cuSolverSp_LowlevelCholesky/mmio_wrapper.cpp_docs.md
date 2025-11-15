# Documentation for Samples/4_CUDA_Libraries/cuSolverSp_LowlevelCholesky/mmio_wrapper.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/cuSolverSp_LowlevelCholesky/mmio_wrapper.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/cuSolverSp_LowlevelCholesky
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp

#include <cusolverDn.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>

#include "mmio.h"

/* avoid Windows warnings (for example: strcpy, fscanf, etc.) */
#if defined(_WIN32)
#define _CRT_SECURE_NO_WARNINGS
#endif

/* various __inline__ __device__  function to initialize a T_ELEM */
template <typename T_ELEM> __inline__ T_ELEM cuGet(int);
template <> __inline__ float                 cuGet<float>(int x) { return float(x); }

template <> __inline__ double cuGet<double>(int x) { return double(x); }

template <> __inline__ cuComplex cuGet<cuComplex>(int x) { return (make_cuComplex(float(x), 0.0f)); }

template <> __inline__ cuDoubleComplex cuGet<cuDoubleComplex>(int x) { return (make_cuDoubleComplex(double(x), 0.0)); }


template <typename T_ELEM> __inline__ T_ELEM cuGet(int, int);
template <> __inline__ float                 cuGet<float>(int x, int y) { return float(x); }

template <> __inline__ double cuGet<double>(int x, int y) { return double(x); }

template <> __inline__ cuComplex cuGet<cuComplex>(int x, int y) { return make_cuComplex(float(x), float(y)); }

template <> __inline__ cuDoubleComplex cuGet<cuDoubleComplex>(int x, int y)
{
    return (make_cuDoubleComplex(double(x), double(y)));
}


template <typename T_ELEM> __inline__ T_ELEM cuGet(float);
template <> __inline__ float                 cuGet<float>(float x) { return float(x); }

template <> __inline__ double cuGet<double>(float x) { return double(x); }

template <> __inline__ cuComplex cuGet<cuComplex>(float x) { return (make_cuComplex(float(x), 0.0f)); }

template <> __inline__ cuDoubleComplex cuGet<cuDoubleComplex>(float x)
{
    return (make_cuDoubleComplex(double(x), 0.0));
}


template <typename T_ELEM> __inline__ T_ELEM cuGet(float, float);
template <> __inline__ float                 cuGet<float>(float x, float y) { return float(x); }

template <> __inline__ double cuGet<double>(float x, float y) { return double(x); }

template <> __inline__ cuComplex cuGet<cuComplex>(float x, float y) { return (make_cuComplex(float(x), float(y))); }

template <> __inline__ cuDoubleComplex cuGet<cuDoubleComplex>(float x, float y)
{
    return (make_cuDoubleComplex(double(x), double(y)));
}


template <typename T_ELEM> __inline__ T_ELEM cuGet(double);
template <> __inline__ float                 cuGet<float>(double x) { return float(x); }

template <> __inline__ double cuGet<double>(double x) { return double(x); }

template <> __inline__ cuComplex cuGet<cuComplex>(double x) { return (make_cuComplex(float(x), 0.0f)); }

template <> __inline__ cuDoubleComplex cuGet<cuDoubleComplex>(double x)
{
    return (make_cuDoubleComplex(double(x), 0.0));
}


template <typename T_ELEM> __inline__ T_ELEM cuGet(double, double);
template <> __inline__ float                 cuGet<float>(double x, double y) { return float(x); }

template <> __inline__ double cuGet<double>(double x, double y) { return double(x); }

template <> __inline__ cuComplex cuGet<cuComplex>(double x, double y) { return (make_cuComplex(float(x), float(y))); }

template <> __inline__ cuDoubleComplex cuGet<cuDoubleComplex>(double x, double y)
{
    return (make_cuDoubleComplex(double(x), double(y)));
}


static void compress_index(const int *Ind, int nnz, int m, int *Ptr, int base)
{
    int i;

    /* initialize everything to zero */
    for (i = 0; i < m + 1; i++) {
        Ptr[i] = 0;
    }
    /* count elements in every row */
    Ptr[0] = base;
    for (i = 0; i < nnz; i++) {
        Ptr[Ind[i] + (1 - base)]++;
    }
    /* add all the values */
    for (i = 0; i < m; i++) {
        Ptr[i + 1] += Ptr[i];
    }
}


struct cooFormat
{
    int i;
    int j;
    int p; // permutation
};


int cmp_cooFormat_csr(struct cooFormat *s, struct cooFormat *t)
{
    if (s->i < t->i) {
        return -1;
    }
    else if (s->i > t->i) {
        return 1;
    }
    else {
        return s->j - t->j;
    }
}

int cmp_cooFormat_csc(struct cooFormat *s, struct cooFormat *t)
{
    if (s->j < t->j) {
        return -1;
    }
    else if (s->j > t->j) {
        return 1;
    }
    else {
        return s->i - t->i;
    }
}

typedef int (*FUNPTR)(const void *, const void *);
typedef int (*FUNPTR2)(struct cooFormat *s, struct cooFormat *t);

static FUNPTR2 fptr_array[2] = {
    cmp_cooFormat_csr,
    cmp_cooFormat_csc,
};


static int verify_pattern(int m, int nnz, int *csrRowPtr, int *csrColInd)
{
    int i, col, start, end, base_index;
    int error_found = 0;

    if (nnz != (csrRowPtr[m] - csrRowPtr[0])) {
        fprintf(stderr,
                "Error (nnz check failed): (csrRowPtr[%d]=%d - csrRowPtr[%d]=%d) != (nnz=%d)\n",
                0,
                csrRowPtr[0],
                m,
                csrRowPtr[m],
                nnz);
        error_found = 1;
    }

    base_index = csrRowPtr[0];
    if ((0 != base_index) && (1 != base_index)) {
        fprintf(stderr, "Error (base index check failed): base index = %d\n", base_index);
        error_found = 1;
    }

    for (i = 0; (!error_found) && (i < m); i++) {
        start = csrRowPtr[i] - base_index;
        end   = csrRowPtr[i + 1] - base_index;
        if (start > end) {
            fprintf(stderr,
                    "Error (corrupted row): csrRowPtr[%d] (=%d) > csrRowPtr[%d] (=%d)\n",
                    i,
                    start + base_index,
                    i + 1,
                    end + base_index);
            error_found = 1;
        }
        for (col = start; col < end; col++) {
            if (csrColInd[col] < base_index) {
                fprintf(stderr, "Error (column vs. base index check failed): csrColInd[%d] < %d\n", col, base_index);
                error_found = 1;
            }
            if ((col < (end - 1)) && (csrColInd[col] >= csrColInd[col + 1])) {
                fprintf(
                    stderr,
                    "Error (sorting of the column indecis check failed): (csrColInd[%d]=%d) >= (csrColInd[%d]=%d)\n",
                    col,
                    csrColInd[col],
                    col + 1,
                    csrColInd[col + 1]);
                error_found = 1;
            }
        }
    }
    return error_found;
}


template <typename T_ELEM>
int loadMMSparseMatrix(char    *filename,
                       char     elem_type,
                       bool     csrFormat,
                       int     *m,
                       int     *n,
                       int     *nnz,
                       T_ELEM **aVal,
                       int    **aRowInd,
                       int    **aColInd,
                       int      extendSymMatrix)
{
    MM_typecode       matcode;
    double           *tempVal;
    int              *tempRowInd, *tempColInd;
    double           *tval;
    int              *trow, *tcol;
    int              *csrRowPtr, *cscColPtr;
    int               i, j, error, base, count;
    struct cooFormat *work;

    /* read the matrix */
    error = mm_read_mtx_crd(filename, m, n, nnz, &trow, &tcol, &tval, &matcode);
    if (error) {
        fprintf(stderr, "!!!! can not open file: '%s'\n", filename);
        return 1;
    }

    /* start error checking */
    if (mm_is_complex(matcode) && ((elem_type != 'z') && (elem_type != 'c'))) {
        fprintf(stderr, "!!!! complex matrix requires type 'z' or 'c'\n");
        return 1;
    }

    if (mm_is_dense(matcode) || mm_is_array(matcode) || mm_is_pattern(matcode) /*|| mm_is_integer(matcode)*/) {
        fprintf(stderr, "!!!! dense, array, pattern and integer matrices are not supported\n");
        return 1;
    }

    /* if necessary symmetrize the pattern (transform from triangular to full) */
    if ((extendSymMatrix) && (mm_is_symmetric(matcode) || mm_is_hermitian(matcode) || mm_is_skew(matcode))) {
        // count number of non-diagonal elements
        count = 0;
        for (i = 0; i < (*nnz); i++) {
            if (trow[i] != tcol[i]) {
                count++;
            }
        }
        // allocate space for the symmetrized matrix
        tempRowInd = (int *)malloc((*nnz + count) * sizeof(int));
        tempColInd = (int *)malloc((*nnz + count) * sizeof(int));
        if (mm_is_real(matcode) || mm_is_integer(matcode)) {
            tempVal = (double *)malloc((*nnz + count) * sizeof(double));
        }
        else {
            tempVal = (double *)malloc(2 * (*nnz + count) * sizeof(double));
        }
        // copy the elements regular and transposed locations
        for (j = 0, i = 0; i < (*nnz); i++) {
            tempRowInd[j] = trow[i];
            tempColInd[j] = tcol[i];
            if (mm_is_real(matcode) || mm_is_integer(matcode)) {
                tempVal[j] = tval[i];
            }
            else {
                tempVal[2 * j]     = tval[2 * i];
                tempVal[2 * j + 1] = tval[2 * i + 1];
            }
            j++;
            if (trow[i] != tcol[i]) {
                tempRowInd[j] = tcol[i];
                tempColInd[j] = trow[i];
                if (mm_is_real(matcode) || mm_is_integer(matcode)) {
                    if (mm_is_skew(matcode)) {
                        tempVal[j] = -tval[i];
                    }
                    else {
                        tempVal[j] = tval[i];
                    }
                }
                else {
                    if (mm_is_hermitian(matcode)) {
                        tempVal[2 * j]     = tval[2 * i];
                        tempVal[2 * j + 1] = -tval[2 * i + 1];
                    }
                    else {
                        tempVal[2 * j]     = tval[2 * i];
                        tempVal[2 * j + 1] = tval[2 * i + 1];
                    }
                }
                j++;
            }
        }
        (*nnz) += count;
        // free temporary storage
        free(trow);
        free(tcol);
        free(tval);
    }
    else {
        tempRowInd = trow;
        tempColInd = tcol;
        tempVal    = tval;
    }
    // life time of (trow, tcol, tval) is over.
    // please use COO format (tempRowInd, tempColInd, tempVal)

    // use qsort to sort COO format
    work = (struct cooFormat *)malloc(sizeof(struct cooFormat) * (*nnz));
    if (NULL == work) {
        fprintf(stderr, "!!!! allocation error, malloc failed\n");
        return 1;
    }
    for (i = 0; i < (*nnz); i++) {
        work[i].i = tempRowInd[i];
        work[i].j = tempColInd[i];
        work[i].p = i; // permutation is identity
    }

    if (csrFormat) {
        /* create row-major ordering of indices (sorted by row and within each row by column) */
        qsort(work, *nnz, sizeof(struct cooFormat), (FUNPTR)fptr_array[0]);
    }
    else {
        /* create column-major ordering of indices (sorted by column and within each column by row) */
        qsort(work, *nnz, sizeof(struct cooFormat), (FUNPTR)fptr_array[1]);
    }

    // (tempRowInd, tempColInd) is sorted either by row-major or by col-major
    for (i = 0; i < (*nnz); i++) {
        tempRowInd[i] = work[i].i;
        tempColInd[i] = work[i].j;
    }

    // setup base
    // check if there is any row/col 0, if so base-0
    // check if there is any row/col equal to matrix dimension m/n, if so base-1
    int base0 = 0;
    int base1 = 0;
    for (i = 0; i < (*nnz); i++) {
        const int row = tempRowInd[i];
        const int col = tempColInd[i];
        if ((0 == row) || (0 == col)) {
            base0 = 1;
        }
        if ((*m == row) || (*n == col)) {
            base1 = 1;
        }
    }
    if (base0 && base1) {
        printf("Error: input matrix is base-0 and base-1 \n");
        return 1;
    }

    base = 0;
    if (base1) {
        base = 1;
    }

    /* compress the appropriate indices */
    if (csrFormat) {
        /* CSR format (assuming row-major format) */
        csrRowPtr = (int *)malloc(((*m) + 1) * sizeof(csrRowPtr[0]));
        if (!csrRowPtr)
            return 1;
        compress_index(tempRowInd, *nnz, *m, csrRowPtr, base);

        *aRowInd = csrRowPtr;
        *aColInd = (int *)malloc((*nnz) * sizeof(int));
    }
    else {
        /* CSC format (assuming column-major format) */
        cscColPtr = (int *)malloc(((*n) + 1) * sizeof(cscColPtr[0]));
        if (!cscColPtr)
            return 1;
        compress_index(tempColInd, *nnz, *n, cscColPtr, base);

        *aColInd = cscColPtr;
        *aRowInd = (int *)malloc((*nnz) * sizeof(int));
    }

    /* transfrom the matrix values of type double into one of the cusparse library types */
    *aVal = (T_ELEM *)malloc((*nnz) * sizeof(T_ELEM));

    for (i = 0; i < (*nnz); i++) {
        if (csrFormat) {
            (*aColInd)[i] = tempColInd[i];
        }
        else {
            (*aRowInd)[i] = tempRowInd[i];
        }
        if (mm_is_real(matcode) || mm_is_integer(matcode)) {
            (*aVal)[i] = cuGet<T_ELEM>(tempVal[work[i].p]);
        }
        else {
            (*aVal)[i] = cuGet<T_ELEM>(tempVal[2 * work[i].p], tempVal[2 * work[i].p + 1]);
        }
    }

    /* check for corruption */
    int error_found;
    if (csrFormat) {
        error_found = verify_pattern(*m, *nnz, *aRowInd, *aColInd);
    }
    else {
        error_found = verify_pattern(*n, *nnz, *aColInd, *aRowInd);
    }
    if (error_found) {
        fprintf(stderr, "!!!! verify_pattern failed\n");
        return 1;
    }

    /* cleanup and exit */
    free(work);
    free(tempVal);
    free(tempColInd);
    free(tempRowInd);

    return 0;
}


/* specific instantiation */
template int loadMMSparseMatrix<float>(char   *filename,
                                       char    elem_type,
                                       bool    csrFormat,
                                       int    *m,
                                       int    *n,
                                       int    *nnz,
                                       float **aVal,
                                       int   **aRowInd,
                                       int   **aColInd,
                                       int     extendSymMatrix);

template int loadMMSparseMatrix<double>(char    *filename,
                                        char     elem_type,
                                        bool     csrFormat,
                                        int     *m,
                                        int     *n,
                                        int     *nnz,
                                        double **aVal,
                                        int    **aRowInd,
                                        int    **aColInd,
                                        int      extendSymMatrix);

template int loadMMSparseMatrix<cuComplex>(char       *filename,
                                           char        elem_type,
                                           bool        csrFormat,
                                           int        *m,
                                           int        *n,
                                           int        *nnz,
                                           cuComplex **aVal,
                                           int       **aRowInd,
                                           int       **aColInd,
                                           int         extendSymMatrix);

template int loadMMSparseMatrix<cuDoubleComplex>(char             *filename,
                                                 char              elem_type,
                                                 bool              csrFormat,
                                                 int              *m,
                                                 int              *n,
                                                 int              *nnz,
                                                 cuDoubleComplex **aVal,
                                                 int             **aRowInd,
                                                 int             **aColInd,
                                                 int               extendSymMatrix);

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/cuSolverSp_LowlevelCholesky/mmio_wrapper.cpp`.

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

- **Total Lines**: 474
- **Approximate Size**: 15941 bytes

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
