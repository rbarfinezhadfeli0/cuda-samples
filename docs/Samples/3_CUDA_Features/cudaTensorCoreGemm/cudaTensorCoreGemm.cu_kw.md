# Keywords: Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu
---

**Total Keywords**: 37

---

## B

### BLOCK_COL_TILES {#blockcoltiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define BLOCK_COL_TILES (WARP_COL_TILES * BLOCK_COL_WARPS)

#define GLOBAL_MEM_STRIDE N_GLOBAL

#def`

### BLOCK_COL_WARPS {#blockcolwarps}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define BLOCK_COL_WARPS 4

#define WARP_ROW_TILES 4
#define WARP_COL_TILES 2

#define BLOCK_ROW_TILE`

### BLOCK_ROW_TILES {#blockrowtiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define BLOCK_ROW_TILES (WARP_ROW_TILES * BLOCK_ROW_WARPS)
#define BLOCK_COL_TILES (WARP_COL_TILES *`

### BLOCK_ROW_WARPS {#blockrowwarps}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define BLOCK_ROW_WARPS 2
#define BLOCK_COL_WARPS 4

#define WARP_ROW_TILES 4
#define WARP_COL_TILES`


## C

### CHUNK_COPY_LINES_PER_WARP {#chunkcopylinesperwarp}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define CHUNK_COPY_LINES_PER_WARP (WARP_COPY_BYTES / CHUNK_LINE_BYTES)
#define CHUNK_COPY_LINE_LANES`

### CHUNK_COPY_LINE_LANES {#chunkcopylinelanes}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define CHUNK_COPY_LINE_LANES     (WARP_SIZE / CHUNK_COPY_LINES_PER_WARP)

#define BLOCK_ROW_WARPS 2`

### CHUNK_K {#chunkk}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define CHUNK_K 8
#endif

#define CHUNK_LINE_BYTES          (CHUNK_K * K * sizeof(half))
#define WAR`

### CHUNK_LINE_BYTES {#chunklinebytes}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define CHUNK_LINE_BYTES          (CHUNK_K * K * sizeof(half))
#define WARP_COPY_BYTES           (WA`

### CPU_DEBUG {#cpudebug}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define CPU_DEBUG 0
#endif

#ifndef SHARED_MEMORY_LIMIT_64K
// Set this to 0 to use more than 64 Kb `

### C_LAYOUT {#clayout}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define C_LAYOUT wmma::mem_row_major

// Implementation constants.

#define WARPS_PER_BLOCK   8
#def`


## G

### GLOBAL_MEM_STRIDE {#globalmemstride}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define GLOBAL_MEM_STRIDE N_GLOBAL

#define SHMEM_STRIDE (N * BLOCK_ROW_TILES)
#define SHMEM_OFFSET `


## K

### K_GLOBAL {#kglobal}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define K_GLOBAL (K * K_TILES)

#define C_LAYOUT wmma::mem_row_major

// Implementation constants.

`

### K_TILES {#ktiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define K_TILES 256

#define M_GLOBAL (M * M_TILES)
#define N_GLOBAL (N * N_TILES)
#define K_GLOBAL `


## M

### M_GLOBAL {#mglobal}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define M_GLOBAL (M * M_TILES)
#define N_GLOBAL (N * N_TILES)
#define K_GLOBAL (K * K_TILES)

#defin`

### M_TILES {#mtiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define M_TILES 256
#define N_TILES 256
#define K_TILES 256

#define M_GLOBAL (M * M_TILES)
#define `

### MxNxK {#mxnxk}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `}
}

// Performs an MxNxK GEMM (C=alpha*A*B +`


## N

### N_GLOBAL {#nglobal}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define N_GLOBAL (N * N_TILES)
#define K_GLOBAL (K * K_TILES)

#define C_LAYOUT wmma::mem_row_major
`

### N_TILES {#ntiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define N_TILES 256
#define K_TILES 256

#define M_GLOBAL (M * M_TILES)
#define N_GLOBAL (N * N_TILE`


## S

### SHARED_MEMORY_LIMIT_64K {#sharedmemorylimit64k}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define SHARED_MEMORY_LIMIT_64K 1
#endif

// GPU configuration.

#define WARP_SIZE 32

// MMA matrix`

### SHMEM_OFFSET {#shmemoffset}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define SHMEM_OFFSET (N * WARP_ROW_TILES)

// The macro below is used to shift rows of the A matrix `

### SHMEM_STRIDE {#shmemstride}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define SHMEM_STRIDE (N * BLOCK_ROW_TILES)
#define SHMEM_OFFSET (N * WARP_ROW_TILES)

// The macro b`

### SKEW_HALF {#skewhalf}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define SKEW_HALF 16

#define checkKernelErrors(expr)                                               `


## T

### THREADS_PER_BLOCK {#threadsperblock}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define THREADS_PER_BLOCK (WARP_SIZE * WARPS_PER_BLOCK)

#if SHARED_MEMORY_LIMIT_64K
// With only 64`


## W

### WARPS_PER_BLOCK {#warpsperblock}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WARPS_PER_BLOCK   8
#define THREADS_PER_BLOCK (WARP_SIZE * WARPS_PER_BLOCK)

#if SHARED_MEMO`

### WARP_COL_TILES {#warpcoltiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WARP_COL_TILES 2

#define BLOCK_ROW_TILES (WARP_ROW_TILES * BLOCK_ROW_WARPS)
#define BLOCK_C`

### WARP_COPY_BYTES {#warpcopybytes}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WARP_COPY_BYTES           (WARP_SIZE * sizeof(int4))
#define CHUNK_COPY_LINES_PER_WARP (WARP`

### WARP_ROW_TILES {#warprowtiles}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WARP_ROW_TILES 4
#define WARP_COL_TILES 2

#define BLOCK_ROW_TILES (WARP_ROW_TILES * BLOCK_R`

### WARP_SIZE {#warpsize}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WARP_SIZE 32

// MMA matrix tile dimensions.

#define M 16
#define N 16
#define K 16

#defin`

### WMMA_K {#wmmak}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WMMA_K 16

// GEMM configuration.

#define M_TILES 256
#define N_TILES 256
#define K_TILES 2`

### WMMA_M {#wmmam}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WMMA_M 16
#define WMMA_N 16
#define WMMA_K 16

// GEMM configuration.

#define M_TILES 256
#`

### WMMA_N {#wmman}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define WMMA_N 16
#define WMMA_K 16

// GEMM configuration.

#define M_TILES 256
#define N_TILES 256`


## C

### checkKernelErrors {#checkkernelerrors}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `#define checkKernelErrors(expr)                                                               \
    `

### compute_gemm {#computegemm}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `__global__ void compute_gemm(`


## I

### init_host_matrices {#inithostmatrices}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `void init_host_matrices(half *a, half *b, float *c)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### matMultiplyOnHost {#matmultiplyonhost}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `void matMultiplyOnHost(half  *A,
                                half  *B,
                         `


## S

### simple_wmma_gemm {#simplewmmagemm}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cudaTensorCoreGemm/cudaTensorCoreGemm.cu](./cudaTensorCoreGemm.cu_docs.md)
- **Context**: `__global__ void
simple_wmma_gemm(`

