# Keywords: Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu
---

**Total Keywords**: 7

---

## L

### LOOP_NUM {#loopnum}

- **Type**: macro
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `#define LOOP_NUM 50

__global__ void atomicKernel(int *atom_arr)
{
    unsigned int tid = blockDim.x`


## A

### atomicKernel {#atomickernel}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `__global__ void atomicKernel(`

### atomicKernel_CPU {#atomickernelcpu}

- **Type**: function
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `void atomicKernel_CPU(int *atom_arr, int no_of_threads)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### max {#max}

- **Type**: macro
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `#define max(a, b) (a) > (b) ? (a) : (b)

#define LOOP_NUM 50

__global__ void atomicKernel(int *atom`

### min {#min}

- **Type**: macro
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `#define min(a, b) (a) < (b) ? (a) : (b)
#define max(a, b) (a) > (b) ? (a) : (b)

#define LOOP_NUM 50`


## V

### verify {#verify}

- **Type**: function
- **File**: [Samples/0_Introduction/systemWideAtomics/systemWideAtomics.cu](./systemWideAtomics.cu_docs.md)
- **Context**: `int verify(int *testData, const int len)
{`

