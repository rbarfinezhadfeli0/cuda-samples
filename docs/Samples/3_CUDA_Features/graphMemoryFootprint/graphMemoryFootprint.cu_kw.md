# Keywords: Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu
---

**Total Keywords**: 14

---

## N

### NUM_GRAPHS {#numgraphs}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `#define NUM_GRAPHS        8
#define THREADS_PER_BLOCK 512

void printMemoryFootprint(int device)
{
 `


## T

### THREADS_PER_BLOCK {#threadsperblock}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `#define THREADS_PER_BLOCK 512

void printMemoryFootprint(int device)
{
    size_t footprint;
    che`


## C

### cleanupMemory {#cleanupmemory}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void cleanupMemory(int device)
{`

### clockBlock {#clockblock}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `__global__ void clockBlock(`

### createSimpleAllocFreeGraph {#createsimpleallocfreegraph}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void createSimpleAllocFreeGraph(cudaGraphExec_t *graphExec, float **dPtr, size_t bytes, int device)
`

### createSimpleAllocNoFreeGraph {#createsimpleallocnofreegraph}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void createSimpleAllocNoFreeGraph(cudaGraphExec_t *graphExec, float **dPtr, size_t bytes, int device`

### createVirtAddrReuseGraph {#createvirtaddrreusegraph}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void createVirtAddrReuseGraph(cudaGraphExec_t *graphExec, size_t bytes, int device)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## P

### physicalMemoryReuseSingleStream {#physicalmemoryreusesinglestream}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void physicalMemoryReuseSingleStream(size_t bytes, int device)
{`

### prepareAllocParams {#prepareallocparams}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void prepareAllocParams(cudaMemAllocNodeParams *allocParams, size_t bytes, int device)
{`

### printMemoryFootprint {#printmemoryfootprint}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void printMemoryFootprint(int device)
{`


## S

### simultaneousStreams {#simultaneousstreams}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void simultaneousStreams(size_t bytes, int device)
{`


## U

### unfreedAllocations {#unfreedallocations}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void unfreedAllocations(size_t bytes, int device)
{`


## V

### virtualAddressReuseSingleGraph {#virtualaddressreusesinglegraph}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/graphMemoryFootprint/graphMemoryFootprint.cu](./graphMemoryFootprint.cu_docs.md)
- **Context**: `void virtualAddressReuseSingleGraph(size_t bytes, int device)
{`

