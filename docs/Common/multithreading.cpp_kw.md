# Keywords: Common/multithreading.cpp
---

**Total Keywords**: 9

---

## C

### CloseHandle {#closehandle}

- **Type**: identifier
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `hread, INFINITE);
  CloseHandle(thread);
}

// Dest`

### CreateThread {#createthread}

- **Type**: identifier
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `d *data) {
  return CreateThread(NULL, 0, (LPTHREAD_`


## T

### TerminateThread {#terminatethread}

- **Type**: identifier
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `TThread thread) {
  TerminateThread(thread, 0);
  Close`


## W

### WaitForMultipleObjects {#waitformultipleobjects}

- **Type**: identifier
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `reads, int num) {
  WaitForMultipleObjects(num, threads, true,`

### WaitForSingleObject {#waitforsingleobject}

- **Type**: identifier
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `TThread thread) {
  WaitForSingleObject(thread, INFINITE);
`


## C

### cutDestroyThread {#cutdestroythread}

- **Type**: function
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutDestroyThread(CUTThread thread) {`

### cutEndThread {#cutendthread}

- **Type**: function
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutEndThread(CUTThread thread) {`

### cutStartThread {#cutstartthread}

- **Type**: function
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `CUTThread cutStartThread(CUT_THREADROUTINE func, void *data) {`

### cutWaitForThreads {#cutwaitforthreads}

- **Type**: function
- **File**: [Common/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutWaitForThreads(const CUTThread *threads, int num) {`

