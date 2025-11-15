# Keywords: Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu
---

**Total Keywords**: 11

---

## N

### NUM_ELEMS {#numelems}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `#define NUM_ELEMS             10000000
#define NUM_THREADS_PER_BLOCK 512

// warp-aggregated atomic `

### NUM_THREADS_PER_BLOCK {#numthreadsperblock}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `#define NUM_THREADS_PER_BLOCK 512

// warp-aggregated atomic increment
__device__ int atomicAggInc(i`


## A

### atomicAggInc {#atomicagginc}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `int atomicAggInc(int *counter)
{`

### atomicAggIncMulti {#atomicaggincmulti}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `int atomicAggIncMulti(const int bucket, int *counter)
{`

### atomicAggMaxMulti {#atomicaggmaxmulti}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `void atomicAggMaxMulti(const int bucket, int *counter, const int valueForMax)
{`


## C

### calculateMaxInBuckets {#calculatemaxinbuckets}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `int calculateMaxInBuckets(int *h_srcArr, int *d_srcArr, int numOfBuckets)
{`

### calculateMaxInEachBuckets {#calculatemaxineachbuckets}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `__global__ void calculateMaxInEachBuckets(`


## F

### filter_arr {#filterarr}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `__global__ void filter_arr(`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### mapIndicesToBuckets {#mapindicestobuckets}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `int mapIndicesToBuckets(int *h_srcArr, int *d_srcArr, int numOfBuckets)
{`

### mapToBuckets {#maptobuckets}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/warpAggregatedAtomicsCG/warpAggregatedAtomicsCG.cu](./warpAggregatedAtomicsCG.cu_docs.md)
- **Context**: `__global__ void
mapToBuckets(`

