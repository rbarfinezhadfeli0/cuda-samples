# Keywords: Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu
---

**Total Keywords**: 8

---

## I

### INSERTION_SORT {#insertionsort}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `#define INSERTION_SORT 32

/////////////////////////////////////////////////////////////////////////`


## M

### MAX_DEPTH {#maxdepth}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `#define MAX_DEPTH      16
#define INSERTION_SORT 32

///////////////////////////////////////////////`


## C

### cdp_simple_quicksort {#cdpsimplequicksort}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `__global__ void cdp_simple_quicksort(`

### check_results {#checkresults}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `void check_results(int n, unsigned int *results_d)
{`


## I

### initialize_data {#initializedata}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `void initialize_data(unsigned int *dst, unsigned int nitems)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### run_qsort {#runqsort}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `void run_qsort(unsigned int *data, unsigned int nitems)
{`


## S

### selection_sort {#selectionsort}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpSimpleQuicksort/cdpSimpleQuicksort.cu](./cdpSimpleQuicksort.cu_docs.md)
- **Context**: `void selection_sort(unsigned int *data, int left, int right)
{`

