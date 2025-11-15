# Keywords: Common/helper_cusolver.h
---

**Total Keywords**: 14

---

## G

### GetTickCount {#gettickcount}

- **Type**: identifier
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `
    return (double)GetTickCount() / 1000.0;
  }
}

`


## H

### HELPER_CUSOLVER {#helpercusolver}

- **Type**: macro
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `#define HELPER_CUSOLVER

#include <ctype.h>
#include <cuda_runtime.h>
#include <math.h>
#include <st`


## Q

### QuadPart {#quadpart}

- **Type**: identifier
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `q = 1.0 / (double)t.QuadPart;
    checkedForHigh`

### QueryPerformanceCounter {#queryperformancecounter}

- **Type**: identifier
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `HighResTimer) {
    QueryPerformanceCounter(&t);
    return (do`

### QueryPerformanceFrequency {#queryperformancefrequency}

- **Type**: identifier
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `  hasHighResTimer = QueryPerformanceFrequency(&t);
    oofreq = 1`


## S

### SWITCH_CHAR {#switchchar}

- **Type**: macro
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `#define SWITCH_CHAR '-'

struct testOpts {
  char *sparse_mat_filename;  // by switch -F<filename>
 `


## W

### WIN32_LEAN_AND_MEAN {#win32leanandmean}

- **Type**: macro
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `#define WIN32_LEAN_AND_MEAN
#endif
#include <windows.h>
double second(void) {
  LARGE_INTEGER t;
  s`


## C

### csr_mat_norminf {#csrmatnorminf}

- **Type**: function
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `double csr_mat_norminf(int m, int n, int nnzA, const cusparseMatDescr_t descrA,
                    `


## D

### display_matrix {#displaymatrix}

- **Type**: function
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `void display_matrix(int m, int n, int nnzA, const cusparseMatDescr_t descrA,
                    con`


## M

### mat_norminf {#matnorminf}

- **Type**: function
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `double mat_norminf(int m, int n, const double *A, int lda) {`


## S

### second {#second}

- **Type**: function
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `double second(void) {`


## T

### testOpts {#testopts}

- **Type**: type
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `struct testOpts`

### timeval {#timeval}

- **Type**: type
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `struct timeval`


## V

### vec_norminf {#vecnorminf}

- **Type**: function
- **File**: [Common/helper_cusolver.h](./helper_cusolver.h_docs.md)
- **Context**: `double vec_norminf(int n, const double *x) {`

