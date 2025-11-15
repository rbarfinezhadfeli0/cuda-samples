# Keywords: Samples/0_Introduction/simpleStreams/simpleStreams.cu
---

**Total Keywords**: 12

---

## A

### ALIGN_UP {#alignup}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `#define ALIGN_UP(x, size) (((size_t)x + (size - 1)) & (~(size - 1)))

__global__ void init_array(int`

### AllocateHostMemory {#allocatehostmemory}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `void AllocateHostMemory(bool bPinGenericMemory, int **pp_a, int **ppAligned_a, int nbytes)
{`


## B

### BlockingSync {#blockingsync}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `/ by default we use BlockingSync

    int niteration`


## D

### DEFAULT_PINNED_GENERIC_MEMORY {#defaultpinnedgenericmemory}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `#define DEFAULT_PINNED_GENERIC_MEMORY true
#endif

int main(int argc, char **argv)
{
    int   cuda_`


## F

### FreeHostMemory {#freehostmemory}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `void FreeHostMemory(bool bPinGenericMemory, int **pp_a, int **ppAligned_a, int nbytes)
{`


## M

### MEMORY_ALIGNMENT {#memoryalignment}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `#define MEMORY_ALIGNMENT  4096
#define ALIGN_UP(x, size) (((size_t)x + (size - 1)) & (~(size - 1)))
`


## V

### VirtualAlloc {#virtualalloc}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `2
        printf("> VirtualAlloc() allocating %4.2f `

### VirtualFree {#virtualfree}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `ifdef WIN32
        VirtualFree(*pp_a, 0, MEM_RELEA`


## C

### correct_data {#correctdata}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `bool correct_data(int *a, const int n, const int c)
{`


## I

### init_array {#initarray}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `__global__ void init_array(`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## P

### printHelp {#printhelp}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleStreams/simpleStreams.cu](./simpleStreams.cu_docs.md)
- **Context**: `void printHelp()
{`

