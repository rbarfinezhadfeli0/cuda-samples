# Keywords: Samples/0_Introduction/simpleCallback/multithreading.cpp
---

**Total Keywords**: 17

---

## B

### BarrierEvent {#barrierevent}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: ` TRUE, FALSE, TEXT("BarrierEvent"));
    barrier.cou`


## C

### CloseHandle {#closehandle}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `ead, INFINITE);
    CloseHandle(thread);
}

// Wait`

### CreateEvent {#createevent}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `rier.barrierEvent = CreateEvent(NULL, TRUE, FALSE, `

### CreateThread {#createthread}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `*data)
{
    return CreateThread(NULL, 0, (LPTHREAD_`


## E

### EnterCriticalSection {#entercriticalsection}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `myBarrierCount;
    EnterCriticalSection(&barrier->criticalS`


## I

### InitializeCriticalSection {#initializecriticalsection}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `rrier barrier;

    InitializeCriticalSection(&barrier.criticalSe`


## L

### LeaveCriticalSection {#leavecriticalsection}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `barrier->count;
    LeaveCriticalSection(&barrier->criticalS`


## S

### SetEvent {#setevent}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `aseCount) {
        SetEvent(barrier->barrierEve`


## W

### WaitForMultipleObjects {#waitformultipleobjects}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `ads, int num)
{
    WaitForMultipleObjects(num, threads, true,`

### WaitForSingleObject {#waitforsingleobject}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `hread thread)
{
    WaitForSingleObject(thread, INFINITE);
`


## C

### cutCreateBarrier {#cutcreatebarrier}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `CUTBarrier cutCreateBarrier(int releaseCount)
{`

### cutDestroyBarrier {#cutdestroybarrier}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutDestroyBarrier(CUTBarrier *barrier)
{`

### cutEndThread {#cutendthread}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutEndThread(CUTThread thread) {`

### cutIncrementBarrier {#cutincrementbarrier}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutIncrementBarrier(CUTBarrier *barrier)
{`

### cutStartThread {#cutstartthread}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `CUTThread cutStartThread(CUT_THREADROUTINE func, void *data)
{`

### cutWaitForBarrier {#cutwaitforbarrier}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutWaitForBarrier(CUTBarrier *barrier)
{`

### cutWaitForThreads {#cutwaitforthreads}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.cpp](./multithreading.cpp_docs.md)
- **Context**: `void cutWaitForThreads(const CUTThread *threads, int num)
{`

