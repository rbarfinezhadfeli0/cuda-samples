# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/config/posix_features.hpp`
- **Filename**: `posix_features.hpp`
- **Language**: hpp
- **Size**: 3347 bytes
- **Lines**: 92
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
//  (C) Copyright John Maddock 2001 - 2003.
//  Use, modification and distribution are subject to the
//  Boost Software License, Version 1.0. (See accompanying file
//  LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)


//  See http://www.boost.org for most recent version.

// All POSIX feature tests go in this file,
// Note that we test _POSIX_C_SOURCE and _XOPEN_SOURCE as well
// _POSIX_VERSION and _XOPEN_VERSION: on some systems POSIX API's
// may be present but none-functional unless _POSIX_C_SOURCE and
// _XOPEN_SOURCE have been defined to the right value (it's up
// to the user to do this *before* including any header, although
// in most cases the compiler will do this for you).

#if defined(BOOST_HAS_UNISTD_H)
#include <unistd.h>

// XOpen has <nl_types.h>, but is this the correct version check?
#if defined(_XOPEN_VERSION) && (_XOPEN_VERSION >= 3)
#define BOOST_HAS_NL_TYPES_H
#endif

// POSIX version 6 requires <stdint.h>
#if defined(_POSIX_VERSION) && (_POSIX_VERSION >= 200100)
#define BOOST_HAS_STDINT_H
#endif

// POSIX version 2 requires <dirent.h>
#if defined(_POSIX_VERSION) && (_POSIX_VERSION >= 199009L)
#define BOOST_HAS_DIRENT_H
#endif

// POSIX version 3 requires <signal.h> to have sigaction:
#if defined(_POSIX_VERSION) && (_POSIX_VERSION >= 199506L)
#define BOOST_HAS_SIGACTION
#endif
// POSIX defines _POSIX_THREADS > 0 for pthread support,
// however some platforms define _POSIX_THREADS without
// a value, hence the (_POSIX_THREADS+0 >= 0) check.
// Strictly speaking this may catch platforms with a
// non-functioning stub <pthreads.h>, but such occurrences should
// occur very rarely if at all.
#if defined(_POSIX_THREADS) && (_POSIX_THREADS + 0 >= 0) && !defined(BOOST_HAS_WINTHREADS) \
    && !defined(BOOST_HAS_MPTASKS)
#define BOOST_HAS_PTHREADS
#endif

// BOOST_HAS_NANOSLEEP:
// This is predicated on _POSIX_TIMERS or _XOPEN_REALTIME:
#if (defined(_POSIX_TIMERS) && (_POSIX_TIMERS + 0 >= 0)) || (defined(_XOPEN_REALTIME) && (_XOPEN_REALTIME + 0 >= 0))
#define BOOST_HAS_NANOSLEEP
#endif

// BOOST_HAS_CLOCK_GETTIME:
// This is predicated on _POSIX_TIMERS (also on _XOPEN_REALTIME
// but at least one platform - linux - defines that flag without
// defining clock_gettime):
#if (defined(_POSIX_TIMERS) && (_POSIX_TIMERS + 0 >= 0))
#define BOOST_HAS_CLOCK_GETTIME
#endif

// BOOST_HAS_SCHED_YIELD:
// This is predicated on _POSIX_PRIORITY_SCHEDULING or
// on _POSIX_THREAD_PRIORITY_SCHEDULING or on _XOPEN_REALTIME.
#if defined(_POSIX_PRIORITY_SCHEDULING) && (_POSIX_PRIORITY_SCHEDULING + 0 > 0)                    \
    || (defined(_POSIX_THREAD_PRIORITY_SCHEDULING) && (_POSIX_THREAD_PRIORITY_SCHEDULING + 0 > 0)) \
    || (defined(_XOPEN_REALTIME) && (_XOPEN_REALTIME + 0 >= 0))
#define BOOST_HAS_SCHED_YIELD
#endif

// BOOST_HAS_GETTIMEOFDAY:
// BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE:
// These are predicated on _XOPEN_VERSION, and appears to be first released
// in issue 4, version 2 (_XOPEN_VERSION > 500).
// Likewise for the functions log1p and expm1.
#if defined(_XOPEN_VERSION) && (_XOPEN_VERSION + 0 >= 500)
#define BOOST_HAS_GETTIMEOFDAY
#if defined(_XOPEN_SOURCE) && (_XOPEN_SOURCE + 0 >= 500)
#define BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE
#endif
#ifndef BOOST_HAS_LOG1P
#define BOOST_HAS_LOG1P
#endif
#ifndef BOOST_HAS_EXPM1
#define BOOST_HAS_EXPM1
#endif
#endif

#endif

```

---
## High-Level Overview
This file is a hpp source file in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `unistd.h`

### Preprocessor Definitions
- **BOOST_HAS_NL_TYPES_H**: `#endif`
- **BOOST_HAS_STDINT_H**: `#endif`
- **BOOST_HAS_DIRENT_H**: `#endif`
- **BOOST_HAS_SIGACTION**: `#endif`
- **BOOST_HAS_PTHREADS**: `#endif`
- **BOOST_HAS_NANOSLEEP**: `#endif`
- **BOOST_HAS_CLOCK_GETTIME**: `#endif`
- **BOOST_HAS_SCHED_YIELD**: `#endif`
- **BOOST_HAS_GETTIMEOFDAY**: `#if defined(_XOPEN_SOURCE) && (_XOPEN_SOURCE + 0 >= 500)`
- **BOOST_HAS_PTHREAD_MUTEXATTR_SETTYPE**: `#endif`
- **BOOST_HAS_LOG1P**: `#endif`
- **BOOST_HAS_EXPM1**: `#endif`


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


---
## Performance & Security Notes
### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

