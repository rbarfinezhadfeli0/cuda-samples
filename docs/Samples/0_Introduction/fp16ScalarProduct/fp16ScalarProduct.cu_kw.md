# Keywords: Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu
---

**Total Keywords**: 8

---

## N

### NUM_OF_BLOCKS {#numofblocks}

- **Type**: macro
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `#define NUM_OF_BLOCKS  128
#define NUM_OF_THREADS 128

__forceinline__ __device__ void reduceInShare`

### NUM_OF_THREADS {#numofthreads}

- **Type**: macro
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `#define NUM_OF_THREADS 128

__forceinline__ __device__ void reduceInShared_intrinsics(half2 *const v`


## G

### generateInput {#generateinput}

- **Type**: function
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `void generateInput(half2 *a, size_t size)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`


## R

### reduceInShared_intrinsics {#reduceinsharedintrinsics}

- **Type**: function
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `void reduceInShared_intrinsics(half2 *const v)
{`

### reduceInShared_native {#reduceinsharednative}

- **Type**: function
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `void reduceInShared_native(half2 *const v)
{`


## S

### scalarProductKernel_intrinsics {#scalarproductkernelintrinsics}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `__global__ void
scalarProductKernel_intrinsics(`

### scalarProductKernel_native {#scalarproductkernelnative}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/fp16ScalarProduct/fp16ScalarProduct.cu](./fp16ScalarProduct.cu_docs.md)
- **Context**: `__global__ void
scalarProductKernel_native(`

