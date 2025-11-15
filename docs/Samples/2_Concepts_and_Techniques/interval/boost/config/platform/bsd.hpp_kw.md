# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp
---

**Total Keywords**: 17

---

## B

### BOOST_HAS_GETTIMEOFDAY {#boosthasgettimeofday}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_GETTIMEOFDAY
#define BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE
#define BOOST_HAS_SIGACTI`

### BOOST_HAS_NANOSLEEP {#boosthasnanosleep}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NANOSLEEP
#define BOOST_HAS_GETTIMEOFDAY
#define BOOST_HAS_PTHREAD_MUTEXATTR_SETTY`

### BOOST_HAS_NL_TYPES_H {#boosthasnltypesh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NL_TYPES_H
#endif

//
// FreeBSD 3.x has pthreads support, but defines _POSIX_THRE`

### BOOST_HAS_PTHREADS {#boosthaspthreads}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_PTHREADS
#endif

//
// No wide character support in the BSD header files:
//
#if d`

### BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE {#boosthaspthreadmutexattrsettype}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE
#define BOOST_HAS_SIGACTION

// boilerplate code:
#defin`

### BOOST_HAS_SCHED_YIELD {#boosthasschedyield}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_SCHED_YIELD
#define BOOST_HAS_NANOSLEEP
#define BOOST_HAS_GETTIMEOFDAY
#define BOO`

### BOOST_HAS_SIGACTION {#boosthassigaction}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_SIGACTION

// boilerplate code:
#define BOOST_HAS_UNISTD_H
#include <boost/config/`

### BOOST_HAS_UNISTD_H {#boosthasunistdh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_HAS_UNISTD_H
#include <boost/config/posix_features.hpp>
`

### BOOST_NO_CTYPE_FUNCTIONS {#boostnoctypefunctions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_NO_CTYPE_FUNCTIONS
#endif

//
// thread API's not auto detected:
//
#define BOOST_HAS_`

### BOOST_NO_CWCHAR {#boostnocwchar}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_NO_CWCHAR
#endif
//
// The BSD <ctype.h> has macros only, no functions:
//
#if !define`

### BOOST_PLATFORM {#boostplatform}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define BOOST_PLATFORM "DragonFly " BOOST_STRINGIZE(__DragonFly__)
#endif

//
// is this the correct`


## D

### DragonFly {#dragonfly}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `ine BOOST_PLATFORM "DragonFly " BOOST_STRINGIZE(_`


## F

### FreeBSD {#freebsd}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `ine BOOST_PLATFORM "FreeBSD " BOOST_STRINGIZE(_`


## N

### NetBSD {#netbsd}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `ine BOOST_PLATFORM "NetBSD " BOOST_STRINGIZE(_`


## O

### OpenBSD {#openbsd}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `ine BOOST_PLATFORM "OpenBSD " BOOST_STRINGIZE(_`


## _

### _GLIBCXX_HAVE_SWPRINTF {#glibcxxhaveswprintf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define _GLIBCXX_HAVE_SWPRINTF 1
#endif

#if !((defined(__FreeBSD__) && (__FreeBSD__ >= 5)) || (__Ne`

### __NetBSD_GCC__ {#netbsdgcc}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/bsd.hpp](./bsd.hpp_docs.md)
- **Context**: `#define __NetBSD_GCC__ (__GNUC__ * 1000000 + __GNUC_MINOR__ * 1000 + __GNUC_PATCHLEVEL__)
// XXX - t`

