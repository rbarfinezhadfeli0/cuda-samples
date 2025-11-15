# Keywords: Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu
---

**Total Keywords**: 13

---

## I

### InputToThreads {#inputtothreads}

- **Type**: identifier
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `s];
    threadData *InputToThreads = new threadData[nt`


## T

### Task {#task}

- **Type**: type
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `struct Task`

### TaskList {#tasklist}

- **Type**: identifier
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `d::vector<Task<T>> &TaskList)
{
    for (unsigne`

### TaskListPtr {#tasklistptr}

- **Type**: identifier
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `    Task<double>   *TaskListPtr;
    cudaStream_t  `


## U

### UnifiedMemoryStreams {#unifiedmemorystreams}

- **Type**: identifier
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `t char *sSDKname = "UnifiedMemoryStreams";

// simple task
t`


## A

### allocate {#allocate}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `void allocate(const unsigned int s, const unsigned int unique_id)
    {`


## D

### drand48 {#drand48}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `double drand48() {`


## E

### execute {#execute}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `void execute(Task<T> &t, cublasHandle_t *handle, cudaStream_t *stream, int tid)
{`


## G

### gemv {#gemv}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `void gemv(int m, int n, T alpha, T *A, T *x, T beta, T *result)
{`


## I

### initialise_tasks {#initialisetasks}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `void initialise_tasks(std::vector<Task<T>> &TaskList)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## S

### srand48 {#srand48}

- **Type**: function
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `void   srand48(long seed) {`


## T

### threadData_t {#threaddatat}

- **Type**: type
- **File**: [Samples/0_Introduction/UnifiedMemoryStreams/UnifiedMemoryStreams.cu](./UnifiedMemoryStreams.cu_docs.md)
- **Context**: `struct threadData_t`

