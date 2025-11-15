# Keywords: Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu
---

**Total Keywords**: 10

---

## G

### GRAPH_LAUNCH_ITERATIONS {#graphlaunchiterations}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `#define GRAPH_LAUNCH_ITERATIONS 3

typedef struct callBackData
{
    const char *fn_name;
    double`


## T

### THREADS_PER_BLOCK {#threadsperblock}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `#define THREADS_PER_BLOCK       512
#define GRAPH_LAUNCH_ITERATIONS 3

typedef struct callBackData
{`


## C

### callBackData {#callbackdata}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `struct callBackData`

### cudaGraphsManual {#cudagraphsmanual}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `void cudaGraphsManual(float  *inputVec_h,
                      float  *inputVec_d,
                `

### cudaGraphsUsingStreamCapture {#cudagraphsusingstreamcapture}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `void cudaGraphsUsingStreamCapture(float  *inputVec_h,
                                  float  *inpu`


## I

### init_input {#initinput}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `void init_input(float *a, size_t size)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### myHostNodeCallback {#myhostnodecallback}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `CUDART_CB myHostNodeCallback(void *data)
{`


## R

### reduce {#reduce}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `__global__ void reduce(`

### reduceFinal {#reducefinal}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/simpleCudaGraphs/simpleCudaGraphs.cu](./simpleCudaGraphs.cu_docs.md)
- **Context**: `__global__ void reduceFinal(`

