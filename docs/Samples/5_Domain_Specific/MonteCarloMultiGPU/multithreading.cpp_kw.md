# Keywords: Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp
---

**Total Keywords**: 7

---

## C

### CloseHandle {#closehandle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `ead, INFINITE);
    CloseHandle(thread);
}

// Wait`

### CreateThread {#createthread}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `*data)
{
    return CreateThread(NULL, 0, (LPTHREAD_`


## W

### WaitForMultipleObjects {#waitformultipleobjects}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `ads, int num)
{
    WaitForMultipleObjects(num, threads, true,`

### WaitForSingleObject {#waitforsingleobject}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `hread thread)
{
    WaitForSingleObject(thread, INFINITE);
`


## C

### cutEndThread {#cutendthread}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutEndThread(CUTThread thread) {`

### cutStartThread {#cutstartthread}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `CUTThread cutStartThread(CUT_THREADROUTINE func, void *data)
{`

### cutWaitForThreads {#cutwaitforthreads}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutWaitForThreads(const CUTThread *threads, int num)
{`

