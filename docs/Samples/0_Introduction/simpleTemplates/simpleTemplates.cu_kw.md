# Keywords: Samples/0_Introduction/simpleTemplates/simpleTemplates.cu
---

**Total Keywords**: 11

---

## A

### ArrayComparator {#arraycomparator}

- **Type**: type
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `class ArrayComparator`

### ArrayFileWriter {#arrayfilewriter}

- **Type**: type
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `class ArrayFileWriter`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `#define MAX(a, b) (a > b ? a : b)
#endif

// includes, kernels
#include "sharedmem.cuh"

int g_Total`


## S

### SharedMemory {#sharedmemory}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `app at run time
    SharedMemory<T> smem;
    T     `

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `and start timer
    StopWatchInterface *timer = NULL;
    `


## C

### compare {#compare}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `bool compare(const float *reference, float *data, unsigned int len)
    {`

### computeGold {#computegold}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `void computeGold(T *reference, T *idata, const unsigned int len)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `void runTest(int argc, char **argv, int len)
{`


## T

### testKernel {#testkernel}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `__global__ void testKernel(`


## W

### write {#write}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleTemplates/simpleTemplates.cu](./simpleTemplates.cu_docs.md)
- **Context**: `bool write(const char *filename, float *data, unsigned int len, float epsilon)
    {`

