# Keywords: Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu
---

**Total Keywords**: 20

---

## A

### AsyncCopyLargeChunk {#asynccopylargechunk}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `argeChunk  = 0,
    AsyncCopyLargeChunk            = 1,
   `

### AsyncCopyLargeChunkAWBarrier {#asynccopylargechunkawbarrier}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `           = 1,
    AsyncCopyLargeChunkAWBarrier   = 2,
    AsyncCop`

### AsyncCopyMultiStage {#asynccopymultistage}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `haredState = 3,
    AsyncCopyMultiStage            = 4,
   `

### AsyncCopyMultiStageLargeChunk {#asynccopymultistagelargechunk}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `
enum kernels {
    AsyncCopyMultiStageLargeChunk  = 0,
    AsyncCopy`

### AsyncCopyMultiStageSharedState {#asynccopymultistagesharedstate}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `WBarrier   = 2,
    AsyncCopyMultiStageSharedState = 3,
    AsyncCopyM`

### AsyncCopySingleStage {#asynccopysinglestage}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `           = 4,
    AsyncCopySingleStage           = 5,
    `


## C

### ConstantInit {#constantinit}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `void ConstantInit(float *data, int size, float val)
{`


## M

### MatrixMulAsyncCopyLargeChunk {#matrixmulasynccopylargechunk}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulAsyncCopyLargeChunk(`

### MatrixMulAsyncCopyLargeChunkAWBarrier {#matrixmulasynccopylargechunkawbarrier}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulAsyncCopyLargeChunkAWBarrier(`

### MatrixMulAsyncCopyMultiStage {#matrixmulasynccopymultistage}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulAsyncCopyMultiStage(`

### MatrixMulAsyncCopyMultiStageLargeChunk {#matrixmulasynccopymultistagelargechunk}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulAsyncCopyMultiStageLargeChunk(`

### MatrixMulAsyncCopyMultiStageSharedState {#matrixmulasynccopymultistagesharedstate}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulAsyncCopyMultiStageSharedState(`

### MatrixMulAsyncCopySingleStage {#matrixmulasynccopysinglestage}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulAsyncCopySingleStage(`

### MatrixMulNaive {#matrixmulnaive}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulNaive(`

### MatrixMulNaiveLargeChunk {#matrixmulnaivelargechunk}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `__global__ void MatrixMulNaiveLargeChunk(`

### MatrixMultiply {#matrixmultiply}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `int MatrixMultiply(int argc, char **argv, const dim3 &dimsA, const dim3 &dimsB, kernels kernel_numbe`


## N

### NaiveLargeChunk {#naivelargechunk}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `           = 6,
    NaiveLargeChunk                = 7
`


## W

### WorkgroupSize {#workgroupsize}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: ` Ops,"
           " WorkgroupSize= %u threads/block\n`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## S

### switch {#switch}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/globalToShmemAsyncCopy/globalToShmemAsyncCopy.cu](./globalToShmemAsyncCopy.cu_docs.md)
- **Context**: `kernel
    switch (kernel_number) {`

