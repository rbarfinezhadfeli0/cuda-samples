# Keywords: Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c
---

**Total Keywords**: 15

---

## M

### MatrixMarket {#matrixmarket}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `ttp://math.nist.gov/MatrixMarket for details.
 *
 *
`

### MatrixMarketBanner {#matrixmarketbanner}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `if (strncmp(banner, MatrixMarketBanner, strlen(MatrixMarke`


## _

### _CRT_SECURE_NO_WARNINGS {#crtsecurenowarnings}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `#define _CRT_SECURE_NO_WARNINGS
#endif

// System includes
#include <ctype.h>
#include <stdio.h>
#in`


## M

### mm_is_valid {#mmisvalid}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_is_valid(MM_typecode matcode)
{`

### mm_read_banner {#mmreadbanner}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_banner(FILE *f, MM_typecode *matcode)
{`

### mm_read_mtx_array_size {#mmreadmtxarraysize}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_mtx_array_size(FILE *f, int *M, int *N)
{`

### mm_read_mtx_crd {#mmreadmtxcrd}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_mtx_crd(char *fname, int *M, int *N, int *nz, int **I, int **J, double **val, MM_typecod`

### mm_read_mtx_crd_data {#mmreadmtxcrddata}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_mtx_crd_data(FILE *f, int M, int N, int nz, int I[], int J[], double val[], MM_typecode `

### mm_read_mtx_crd_entry {#mmreadmtxcrdentry}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_mtx_crd_entry(FILE *f, int *I, int *J, double *real, double *imag, MM_typecode matcode)
`

### mm_read_mtx_crd_size {#mmreadmtxcrdsize}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_mtx_crd_size(FILE *f, int *M, int *N, int *nz)
{`

### mm_read_unsymmetric_sparse {#mmreadunsymmetricsparse}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_read_unsymmetric_sparse(const char *fname, int *M_, int *N_, int *nz_, double **val_, int **I`

### mm_write_banner {#mmwritebanner}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_write_banner(FILE *f, MM_typecode matcode)
{`

### mm_write_mtx_array_size {#mmwritemtxarraysize}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_write_mtx_array_size(FILE *f, int M, int N)
{`

### mm_write_mtx_crd {#mmwritemtxcrd}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_write_mtx_crd(char fname[], int M, int N, int nz, int I[], int J[], double val[], MM_typecode`

### mm_write_mtx_crd_size {#mmwritemtxcrdsize}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cuSolverDn_LinearSolver/mmio.c](./mmio.c_docs.md)
- **Context**: `int mm_write_mtx_crd_size(FILE *f, int M, int N, int nz)
{`

