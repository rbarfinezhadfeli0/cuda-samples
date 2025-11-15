# Keywords: Samples/2_Concepts_and_Techniques/histogram/histogram64.cu
---

**Total Keywords**: 11

---

## M

### MERGE_THREADBLOCK_SIZE {#mergethreadblocksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `#define MERGE_THREADBLOCK_SIZE 256

__global__ void mergeHistogram64Kernel(uint *d_Histogram, uint *`


## S

### SHARED_MEMORY_BANKS {#sharedmemorybanks}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `#define SHARED_MEMORY_BANKS 16

////////////////////////////////////////////////////////////////////`


## A

### addByte {#addbyte}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `void addByte(uchar *s_ThreadBase, uint data)
{`

### addWord {#addword}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `void addWord(uchar *s_ThreadBase, uint data)
{`


## C

### closeHistogram64 {#closehistogram64}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `void closeHistogram64(void) {`


## H

### histogram64 {#histogram64}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `void histogram64(uint *d_Histogram, void *d_Data, uint byteCount)
{`

### histogram64Kernel {#histogram64kernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `__global__ void histogram64Kernel(`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `uint iDivUp(uint a, uint b) {`

### iSnapDown {#isnapdown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `uint iSnapDown(uint a, uint b) {`

### initHistogram64 {#inithistogram64}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `void initHistogram64(void)
{`


## M

### mergeHistogram64Kernel {#mergehistogram64kernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram64.cu](./histogram64.cu_docs.md)
- **Context**: `__global__ void mergeHistogram64Kernel(`

