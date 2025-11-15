# Keywords: Samples/0_Introduction/simpleCallback/multithreading.h
---

**Total Keywords**: 4

---

## C

### CUTBarrier {#cutbarrier}

- **Type**: type
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.h](./multithreading.h_docs.md)
- **Context**: `struct CUTBarrier`

### CUT_THREADEND {#cutthreadend}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.h](./multithreading.h_docs.md)
- **Context**: `#define CUT_THREADEND  return 0

struct CUTBarrier
{
    pthread_mutex_t mutex;
    pthread_cond_t  `

### CUT_THREADPROC {#cutthreadproc}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.h](./multithreading.h_docs.md)
- **Context**: `#define CUT_THREADPROC void *
#define CUT_THREADEND  return 0

struct CUTBarrier
{
    pthread_mutex`


## M

### MULTITHREADING_H {#multithreadingh}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCallback/multithreading.h](./multithreading.h_docs.md)
- **Context**: `#define MULTITHREADING_H

// Simple portable thread library.

#if defined(WIN32) || defined(_WIN32) `

