# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp
---

**Total Keywords**: 13

---

## B

### BOOST_BIND_ENABLE_PASCAL {#boostbindenablepascal}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_BIND_ENABLE_PASCAL

#endif

#endif
`

### BOOST_HAS_GETTIMEOFDAY {#boosthasgettimeofday}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_GETTIMEOFDAY
#endif

// The MP task implementation of Boost Threads aims to replac`

### BOOST_HAS_MPTASKS {#boosthasmptasks}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_MPTASKS
#elif (__dest_os == __mac_os_x)
// We are doing a Carbon/Mach-O/MSL build `

### BOOST_HAS_NANOSLEEP {#boosthasnanosleep}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NANOSLEEP

#endif

#else

// Using the MSL C library.

// We will eventually suppo`

### BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE {#boosthaspthreadmutexattrsettype}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE
#define BOOST_HAS_NANOSLEEP

#endif

#else

// Using the`

### BOOST_HAS_SCHED_YIELD {#boosthasschedyield}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_SCHED_YIELD
#define BOOST_HAS_GETTIMEOFDAY
#define BOOST_HAS_SIGACTION

#if (__GNU`

### BOOST_HAS_SIGACTION {#boosthassigaction}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_SIGACTION

#if (__GNUC__ < 3) && !defined(__APPLE_CC__)

// GCC strange "ignore st`

### BOOST_HAS_STDINT_H {#boosthasstdinth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_STDINT_H
#endif

//
// BSD runtime has pthreads, sigaction, sched_yield and gettim`

### BOOST_HAS_THREADS {#boosthasthreads}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_THREADS

// The remote call manager depends on this.
#define BOOST_BIND_ENABLE_PAS`

### BOOST_HAS_UNISTD_H {#boosthasunistdh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_HAS_UNISTD_H
#endif
//
// Begin by including our boilerplate code for POSIX
// feature`

### BOOST_NO_STDC_NAMESPACE {#boostnostdcnamespace}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_NO_STDC_NAMESPACE
#endif

#if (__GNUC__ == 4)

// Both gcc and intel require these.
#d`

### BOOST_PLATFORM {#boostplatform}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: `#define BOOST_PLATFORM "Mac OS"

#if __MACH__ && !defined(_MSL_USING_MSL_C)

// Using the Mac OS X s`


## M

### MaxOS {#maxos}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/macos.hpp](./macos.hpp_docs.md)
- **Context**: ` able to do this on MaxOS X.
//
#include <boo`

