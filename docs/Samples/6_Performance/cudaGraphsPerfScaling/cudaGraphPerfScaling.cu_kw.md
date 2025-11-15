# Keywords: Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu
---

**Total Keywords**: 19

---

## L

### LatchType {#latchtype}

- **Type**: identifier
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `ypedef volatile int LatchType;

std::chrono::time`


## R

### RANGE {#range}

- **Type**: macro
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `#define RANGE(name)
#endif

std::vector<cudaStream_t> stream;
cudaEvent_t               event[1];
cu`

### RANGE_POP {#rangepop}

- **Type**: macro
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `#define RANGE_POP()      nvtxRangePop();
#else
#define RANGE(name)
#endif

std::vector<cudaStream_t>`

### RANGE_PUSH {#rangepush}

- **Type**: macro
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `#define RANGE_PUSH(name) nvtxRangePushA(name)
#define RANGE_POP()      nvtxRangePop();
#else
#define`


## T

### Tracer {#tracer}

- **Type**: type
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `class Tracer`


## U

### USE_NVTX {#usenvtx}

- **Type**: macro
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `#define USE_NVTX

#include <chrono>
#include <cstdio>
#include <cuda_runtime.h>
#include <vector>

t`


## _

### __globaltimer {#globaltimer}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `long __globaltimer()
{`


## C

### createParallelChain {#createparallelchain}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `cudaGraph_t createParallelChain(int length, int width, bool singleEntry = false)
{`


## D

### delay {#delay}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `__global__ void delay(`


## E

### empty {#empty}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `__global__ void empty(`


## G

### getAsyncMicroSecondDuration {#getasyncmicrosecondduration}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `float getAsyncMicroSecondDuration(cudaEvent_t start, cudaEvent_t end)
{`

### getMicroSecondDuration {#getmicrosecondduration}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `float getMicroSecondDuration(T start, T end)
{`


## H

### hostData {#hostdata}

- **Type**: type
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `struct hostData`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## P

### postUploadAnnotation {#postuploadannotation}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `__global__ void postUploadAnnotation(`

### preUploadAnnotation {#preuploadannotation}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `__global__ void preUploadAnnotation(`


## R

### runDemo {#rundemo}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `void runDemo(cudaGraph_t graph, int length, int width)
{`


## U

### usage {#usage}

- **Type**: function
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `void usage()
{`


## W

### waitWithTimeout {#waitwithtimeout}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/cudaGraphsPerfScaling/cudaGraphPerfScaling.cu](./cudaGraphPerfScaling.cu_docs.md)
- **Context**: `__global__ void waitWithTimeout(`

