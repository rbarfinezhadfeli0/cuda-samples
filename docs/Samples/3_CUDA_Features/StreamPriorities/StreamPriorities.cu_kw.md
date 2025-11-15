# Keywords: Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu
---

**Total Keywords**: 9

---

## E

### EACH_SIZE {#eachsize}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `#define EACH_SIZE  128 * 1024 * 1024

// # threadblocks
#define TBLOCKS 1024
#define THREADS 512

//`

### ERR_EQ {#erreq}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `#define ERR_EQ(X, Y)                                                                 \
    do {     `

### ERR_NE {#errne}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `#define ERR_NE(X, Y)                                                                 \
    do {     `


## T

### TBLOCKS {#tblocks}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `#define TBLOCKS 1024
#define THREADS 512

// throw error on equality
#define ERR_EQ(X, Y)           `

### THREADS {#threads}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `#define THREADS 512

// throw error on equality
#define ERR_EQ(X, Y)                                `

### TOTAL_SIZE {#totalsize}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `#define TOTAL_SIZE 256 * 1024 * 1024
#define EACH_SIZE  128 * 1024 * 1024

// # threadblocks
#define`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### mem_init {#meminit}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `void mem_init(int *buf, size_t n)
{`

### memcpy_kernel {#memcpykernel}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/StreamPriorities/StreamPriorities.cu](./StreamPriorities.cu_docs.md)
- **Context**: `__global__ void memcpy_kernel(`

