# Keywords: Samples/2_Concepts_and_Techniques/reduction/reduction.cpp
---

**Total Keywords**: 14

---

## M

### MAX_BLOCK_DIM_SIZE {#maxblockdimsize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `#define MAX_BLOCK_DIM_SIZE 65535

#ifdef WIN32
#define strcasecmp strcmpi
#endif

extern "C" bool is`

### MIN {#min}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `#define MIN(x, y) ((x < y) ? x : y)
#endif

////////////////////////////////////////////////////////`


## N

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `, "
               "NumDevsUsed = %d, Workgroup = %`


## R

### ReduceType {#reducetype}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `"reduction.h"

enum ReduceType { REDUCE_INT, REDUC`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `,
                  StopWatchInterface *timer,
           `


## B

### benchmarkReduce {#benchmarkreduce}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `T benchmarkReduce(int                 n,
                  int                 numThreads,
         `


## G

### getNumBlocksAndThreads {#getnumblocksandthreads}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `void getNumBlocksAndThreads(int whichKernel, int n, int maxBlocks, int maxThreads, int &blocks, int `


## I

### isPow2 {#ispow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `bool isPow2(unsigned int x) {`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## N

### nextPow2 {#nextpow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `int nextPow2(unsigned int x)
{`


## R

### reduceCPU {#reducecpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `T reduceCPU(T *data, int size)
{`

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `bool runTest(int argc, char **argv, ReduceType datatype)
{`


## S

### shmoo {#shmoo}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `void shmoo(int minN, int maxN, int maxThreads, int maxBlocks, ReduceType datatype)
{`

### strcasecmp {#strcasecmp}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/reduction/reduction.cpp](./reduction.cpp_docs.md)
- **Context**: `#define strcasecmp strcmpi
#endif

extern "C" bool isPow2(unsigned int x) { return ((x & (x - 1)) ==`

