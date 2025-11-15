# Keywords: Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu
---

**Total Keywords**: 21

---

## B

### BLOCK_SIZE {#blocksize}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `#define BLOCK_SIZE 32
__global__ void matrixMultiplyKernel(float *C, float *A, float *B, unsigned in`


## C

### CpAsync {#cpasync}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: ` DEVICE_MEMORY
    "CpAsync", // USE HOST PAGEA`

### CpHpglk {#cphpglk}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `E_MEMORY ASYNC
    "CpHpglk", // USE HOST PAGEL`

### CpPglAs {#cppglas}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: ` DEVICE MEMORY
    "CpPglAs"  // USE HOST PAGEL`


## M

### MemCopy {#memcopy}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `  // Zero Copy
    "MemCopy", // USE HOST PAGEA`

### MemcpyAsync_HostCudaHostAlloc_DeviceCudaMalloc {#memcpyasynchostcudahostallocdevicecudamalloc}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `                   "MemcpyAsync_HostCudaHostAlloc_DeviceCudaMalloc"};

const char *mem`

### MemcpyAsync_HostMalloc_DeviceCudaMalloc {#memcpyasynchostmallocdevicecudamalloc}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `                   "MemcpyAsync_HostMalloc_DeviceCudaMalloc",
                 `


## R

### RandFloat {#randfloat}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `float RandFloat(float low, float high)
{`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `m / threads.y);
    StopWatchInterface *gpuLaunchCallsTime`


## V

### VERIFY_GPU_CORRECTNESS {#verifygpucorrectness}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `#define VERIFY_GPU_CORRECTNESS 0

size_t maxSampleSizeInMb = 64;
int    numKernelRuns     = 20;
int `


## C

### copyMatrix {#copymatrix}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void copyMatrix(float *dstMatrix, float *srcMatrix, unsigned int matrixDim)
{`


## F

### fillMatrixWithRandomValues {#fillmatrixwithrandomvalues}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void fillMatrixWithRandomValues(float *matrix, unsigned int matrixDim)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### matrixMultiplyKernel {#matrixmultiplykernel}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `__global__ void matrixMultiplyKernel(`

### matrixMultiplyPerfRunner {#matrixmultiplyperfrunner}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void matrixMultiplyPerfRunner(bool reportAsBandwidth,
                              bool print_launc`


## R

### resultsData {#resultsdata}

- **Type**: type
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `struct resultsData`

### runMatrixMultiplyKernel {#runmatrixmultiplykernel}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void runMatrixMultiplyKernel(unsigned int matrixDim,
                             int          alloc`


## T

### testResults {#testresults}

- **Type**: type
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `struct testResults`


## U

### usage {#usage}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void usage()
{`


## V

### verifyMatrixData {#verifymatrixdata}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void verifyMatrixData(float *expectedData, float *observedData, unsigned int matrixDim)
{`

### verifyMatrixMultiplyCorrectness {#verifymatrixmultiplycorrectness}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/matrixMultiplyPerf.cu](./matrixMultiplyPerf.cu_docs.md)
- **Context**: `void verifyMatrixMultiplyCorrectness(float *C, float *A, float *B, unsigned int matrixDim)
{`

