# Keywords: Samples/2_Concepts_and_Techniques/histogram/histogram256.cu
---

**Total Keywords**: 9

---

## M

### MERGE_THREADBLOCK_SIZE {#mergethreadblocksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `#define MERGE_THREADBLOCK_SIZE 256

__global__ void mergeHistogram256Kernel(uint *d_Histogram, uint `


## T

### TAG_MASK {#tagmask}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `#define TAG_MASK 0xFFFFFFFFU
inline __device__ void addByte(uint *s_WarpHist, uint data, uint thread`


## A

### addByte {#addbyte}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `void addByte(uint *s_WarpHist, uint data, uint threadTag) {`

### addWord {#addword}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `void addWord(uint *s_WarpHist, uint data, uint tag)
{`


## C

### closeHistogram256 {#closehistogram256}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `void closeHistogram256(void) {`


## H

### histogram256 {#histogram256}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `void histogram256(uint *d_Histogram, void *d_Data, uint byteCount)
{`

### histogram256Kernel {#histogram256kernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `__global__ void histogram256Kernel(`


## I

### initHistogram256 {#inithistogram256}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `void initHistogram256(void)
{`


## M

### mergeHistogram256Kernel {#mergehistogram256kernel}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/histogram/histogram256.cu](./histogram256.cu_docs.md)
- **Context**: `__global__ void mergeHistogram256Kernel(`

