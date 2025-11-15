# Keywords: Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu
---

**Total Keywords**: 18

---

## A

### ALLOWABLE_VARIANCE {#allowablevariance}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `#define ALLOWABLE_VARIANCE 1.e-6f
#define NUM_ELEMENTS       8000000

// Stores the square of each i`


## N

### NUM_ELEMENTS {#numelements}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `#define NUM_ELEMENTS       8000000

// Stores the square of each input element in output array
__glo`


## T

### THREADS_PER_BLOCK {#threadsperblock}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `#define THREADS_PER_BLOCK  512
#define ALLOWABLE_VARIANCE 1.e-6f
#define NUM_ELEMENTS       8000000
`


## C

### checkValidationFailure {#checkvalidationfailure}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `int checkValidationFailure(bool *foundValidationFailure)
{`

### createFreeGraph {#createfreegraph}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void createFreeGraph(cudaGraphExec_t *graphExec, float *dPtr)
{`

### createNegateSquaresGraphExplicitly {#createnegatesquaresgraphexplicitly}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void createNegateSquaresGraphExplicitly(cudaGraphExec_t *graphExec,
                                `

### createNegateSquaresGraphWithStreamCapture {#createnegatesquaresgraphwithstreamcapture}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void createNegateSquaresGraphWithStreamCapture(cudaGraphExec_t *graphExec,
                         `


## D

### doNegateSquaresInStream {#donegatesquaresinstream}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void doNegateSquaresInStream(cudaStream_t stream1, negSquareArrays *hostArrays, float **d_negSquare_`


## F

### fillRandomly {#fillrandomly}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void fillRandomly(float *array, int numElements)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## N

### negSquareArrays {#negsquarearrays}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `struct negSquareArrays`

### negateArray {#negatearray}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `__global__ void negateArray(`


## P

### prepareHostArrays {#preparehostarrays}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void prepareHostArrays(negSquareArrays *hostArrays)
{`

### prepareRefArrays {#preparerefarrays}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void prepareRefArrays(negSquareArrays *hostArrays, negSquareArrays *deviceRefArrays, bool **foundVal`


## R

### resetOutputArrays {#resetoutputarrays}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void resetOutputArrays(negSquareArrays *hostArrays)
{`


## S

### squareArray {#squarearray}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `__global__ void squareArray(`


## V

### validateGPU {#validategpu}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `__global__ void validateGPU(`

### validateHost {#validatehost}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryNodes/graphMemoryNodes.cu](./graphMemoryNodes.cu_docs.md)
- **Context**: `void validateHost(negSquareArrays *hostArrays, bool *foundValidationFailure)
{`

