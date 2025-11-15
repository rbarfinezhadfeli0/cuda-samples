# Keywords: Samples/0_Introduction/simpleZeroCopy/simpleZeroCopy.cu
---

**Total Keywords**: 5

---

## A

### ALIGN_UP {#alignup}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleZeroCopy/simpleZeroCopy.cu](./simpleZeroCopy.cu_docs.md)
- **Context**: `#define ALIGN_UP(x, size) (((size_t)x + (size - 1)) & (~(size - 1)))

int main(int argc, char **argv`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleZeroCopy/simpleZeroCopy.cu](./simpleZeroCopy.cu_docs.md)
- **Context**: `#define MAX(a, b) (a > b ? a : b)
#endif

/* Add two vectors on the GPU */
__global__ void vectorAdd`

### MEMORY_ALIGNMENT {#memoryalignment}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleZeroCopy/simpleZeroCopy.cu](./simpleZeroCopy.cu_docs.md)
- **Context**: `#define MEMORY_ALIGNMENT  4096
#define ALIGN_UP(x, size) (((size_t)x + (size - 1)) & (~(size - 1)))
`

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleZeroCopy/simpleZeroCopy.cu](./simpleZeroCopy.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## V

### vectorAddGPU {#vectoraddgpu}

- **Type**: cuda_kernel
- **File**: [Samples/0_Introduction/simpleZeroCopy/simpleZeroCopy.cu](./simpleZeroCopy.cu_docs.md)
- **Context**: `__global__ void vectorAddGPU(`

