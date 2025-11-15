# Keywords: Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.h
---

**Total Keywords**: 3

---

## C

### CUT_THREADEND {#cutthreadend}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.h](./multithreading.h_docs.md)
- **Context**: `#define CUT_THREADEND
#endif

#ifdef __cplusplus
extern "C"
{
#endif

    // Create thread.
    CUTT`

### CUT_THREADPROC {#cutthreadproc}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.h](./multithreading.h_docs.md)
- **Context**: `#define CUT_THREADPROC void
#define CUT_THREADEND
#endif

#ifdef __cplusplus
extern "C"
{
#endif

  `


## M

### MULTITHREADING_H {#multithreadingh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.h](./multithreading.h_docs.md)
- **Context**: `#define MULTITHREADING_H

// Simple portable thread library.

#if defined(WIN32) || defined(_WIN32) `

