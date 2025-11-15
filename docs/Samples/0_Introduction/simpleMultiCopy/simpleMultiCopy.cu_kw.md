# Keywords: Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu
---

**Total Keywords**: 7

---

## S

### SIMULATE_IO {#simulateio}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `#define SIMULATE_IO

int *h_data_source;
int *h_data_sink;

int *h_data_in[STREAM_COUNT];
int *d_dat`

### STREAM_COUNT {#streamcount}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `#define STREAM_COUNT 4

// Uncomment to simulate data source/sink IO times
// #define SIMULATE_IO

i`


## I

### incKernel {#inckernel}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `__global__ void incKernel(`

### init {#init}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `void init()
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`


## P

### processWithStreams {#processwithstreams}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `float processWithStreams(int streams_used)
{`


## T

### test {#test}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleMultiCopy/simpleMultiCopy.cu](./simpleMultiCopy.cu_docs.md)
- **Context**: `bool test()
{`

