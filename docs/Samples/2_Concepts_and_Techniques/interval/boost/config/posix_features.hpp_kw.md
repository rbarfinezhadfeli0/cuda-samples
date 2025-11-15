# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp
---

**Total Keywords**: 12

---

## B

### BOOST_HAS_CLOCK_GETTIME {#boosthasclockgettime}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_CLOCK_GETTIME
#endif

// BOOST_HAS_SCHED_YIELD:
// This is predicated on _POSIX_PR`

### BOOST_HAS_DIRENT_H {#boosthasdirenth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_DIRENT_H
#endif

// POSIX version 3 requires <signal.h> to have sigaction:
#if def`

### BOOST_HAS_EXPM1 {#boosthasexpm1}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_EXPM1
#endif
#endif

#endif
`

### BOOST_HAS_GETTIMEOFDAY {#boosthasgettimeofday}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_GETTIMEOFDAY
#if defined(_XOPEN_SOURCE) && (_XOPEN_SOURCE + 0 >= 500)
#define BOOS`

### BOOST_HAS_LOG1P {#boosthaslog1p}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_LOG1P
#endif
#ifndef BOOST_HAS_EXPM1
#define BOOST_HAS_EXPM1
#endif
#endif

#endif`

### BOOST_HAS_NANOSLEEP {#boosthasnanosleep}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NANOSLEEP
#endif

// BOOST_HAS_CLOCK_GETTIME:
// This is predicated on _POSIX_TIME`

### BOOST_HAS_NL_TYPES_H {#boosthasnltypesh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NL_TYPES_H
#endif

// POSIX version 6 requires <stdint.h>
#if defined(_POSIX_VERSI`

### BOOST_HAS_PTHREADS {#boosthaspthreads}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_PTHREADS
#endif

// BOOST_HAS_NANOSLEEP:
// This is predicated on _POSIX_TIMERS or`

### BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE {#boosthaspthreadmutexattrsettype}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE
#endif
#ifndef BOOST_HAS_LOG1P
#define BOOST_HAS_LOG1P
#`

### BOOST_HAS_SCHED_YIELD {#boosthasschedyield}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_SCHED_YIELD
#endif

// BOOST_HAS_GETTIMEOFDAY:
// BOOST_HAS_PTHREAD_MUTEXATTR_SETT`

### BOOST_HAS_SIGACTION {#boosthassigaction}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_SIGACTION
#endif
// POSIX defines _POSIX_THREADS > 0 for pthread support,
// howev`

### BOOST_HAS_STDINT_H {#boosthasstdinth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp](./posix_features.hpp_docs.md)
- **Context**: `#define BOOST_HAS_STDINT_H
#endif

// POSIX version 2 requires <dirent.h>
#if defined(_POSIX_VERSION`

