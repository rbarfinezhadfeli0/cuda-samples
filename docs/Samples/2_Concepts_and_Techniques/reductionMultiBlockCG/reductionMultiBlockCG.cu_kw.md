# Keywords: Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu
---

**Total Keywords**: 11

---

## S

### SinglePass {#singlepass}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `hing %s kernel\n", "SinglePass Multi Block Coopera`

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `                    StopWatchInterface *timer,
           `


## B

### benchmarkReduce {#benchmarkreduce}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `float benchmarkReduce(int                 n,
                      int                 numThreads,
 `


## C

### call_reduceSinglePassMultiBlockCG {#callreducesinglepassmultiblockcg}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `void call_reduceSinglePassMultiBlockCG(int size, int threads, int numBlocks, float *d_idata, float *`


## G

### getNumBlocksAndThreads {#getnumblocksandthreads}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `void getNumBlocksAndThreads(int n, int maxBlocks, int maxThreads, int &blocks, int &threads)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## N

### nextPow2 {#nextpow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `int nextPow2(unsigned int x)
{`


## R

### reduceBlock {#reduceblock}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `void reduceBlock(double *sdata, const cg::thread_block &cta)
{`

### reduceCPU {#reducecpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `T reduceCPU(T *data, int size)
{`

### reduceSinglePassMultiBlockCG {#reducesinglepassmultiblockcg}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `__global__ void reduceSinglePassMultiBlockCG(`

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reductionMultiBlockCG/reductionMultiBlockCG.cu](./reductionMultiBlockCG.cu_docs.md)
- **Context**: `bool runTest(int argc, char **argv, int device)
{`

