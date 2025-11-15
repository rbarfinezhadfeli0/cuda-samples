# Keywords: Samples/6_Performance/transpose/transpose.cu
---

**Total Keywords**: 19

---

## B

### BLOCK_ROWS {#blockrows}

- **Type**: macro
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `#define BLOCK_ROWS 16

// This sample assumes that MATRIX_SIZE_X = MATRIX_SIZE_Y
int MATRIX_SIZE_X =`


## F

### FLOOR {#floor}

- **Type**: macro
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `#define FLOOR(a, b) (a - (a % b))

// Compute the tile size necessary to illustrate performance case`


## M

### MatrixSize {#matrixsize}

- **Type**: identifier
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `          printf("> MatrixSize X = %d is greater t`


## N

### NUM_REPS {#numreps}

- **Type**: macro
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `#define NUM_REPS 100

// -------------------------------------------------------
// Copies
// width `

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `         "elements, NumDevsUsed = %u, Workgroup = %`


## T

### TILE_DIM {#tiledim}

- **Type**: macro
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `#define TILE_DIM   32
#define BLOCK_ROWS 16

// This sample assumes that MATRIX_SIZE_X = MATRIX_SIZE`


## C

### computeTransposeGold {#computetransposegold}

- **Type**: function
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `void computeTransposeGold(float *gold, float *idata, const int size_x, const int size_y)
{`

### copy {#copy}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void copy(`

### copySharedMem {#copysharedmem}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void copySharedMem(`


## G

### getParams {#getparams}

- **Type**: function
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `void getParams(int argc, char **argv, cudaDeviceProp &deviceProp, int &size_x, int &size_y, int max_`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## S

### showHelp {#showhelp}

- **Type**: function
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `void showHelp()
{`

### switch {#switch}

- **Type**: function
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `pointer
        switch (k) {`


## T

### transposeCoalesced {#transposecoalesced}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void transposeCoalesced(`

### transposeCoarseGrained {#transposecoarsegrained}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void transposeCoarseGrained(`

### transposeDiagonal {#transposediagonal}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void transposeDiagonal(`

### transposeFineGrained {#transposefinegrained}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void transposeFineGrained(`

### transposeNaive {#transposenaive}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void transposeNaive(`

### transposeNoBankConflicts {#transposenobankconflicts}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/transpose/transpose.cu](./transpose.cu_docs.md)
- **Context**: `__global__ void transposeNoBankConflicts(`

