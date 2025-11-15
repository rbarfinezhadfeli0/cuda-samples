# Keywords: Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu
---

**Total Keywords**: 12

---

## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `                    StopWatchInterface *timer,
           `


## V

### VERSION_MAJOR {#versionmajor}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `#define VERSION_MAJOR (CUDART_VERSION / 1000)
#define VERSION_MINOR (CUDART_VERSION % 100) / 10

con`

### VERSION_MINOR {#versionminor}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `#define VERSION_MINOR (CUDART_VERSION % 100) / 10

const char *sSDKsample = "threadFenceReduction";
`


## B

### benchmarkReduce {#benchmarkreduce}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `float benchmarkReduce(int                 n,
                      int                 numThreads,
 `


## G

### getNumBlocksAndThreads {#getnumblocksandthreads}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `void getNumBlocksAndThreads(int n, int maxBlocks, int maxThreads, int &blocks, int &threads)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## N

### nextPow2 {#nextpow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `int nextPow2(unsigned int x)
{`


## R

### reduce {#reduce}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `void reduce(int size, int threads, int blocks, float *d_idata, float *d_odata)
{`

### reduceCPU {#reducecpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `T reduceCPU(T *data, int size)
{`

### reduceSinglePass {#reducesinglepass}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `void reduceSinglePass(int size, int threads, int blocks, float *d_idata, float *d_odata)
{`

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `bool runTest(int argc, char **argv)
{`


## S

### shmoo {#shmoo}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/threadFenceReduction/threadFenceReduction.cu](./threadFenceReduction.cu_docs.md)
- **Context**: `void shmoo(int minN, int maxN, int maxThreads, int maxBlocks)
{`

