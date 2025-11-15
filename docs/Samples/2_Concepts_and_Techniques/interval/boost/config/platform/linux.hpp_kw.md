# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp
---

**Total Keywords**: 13

---

## B

### BOOST_HAS_GETTIMEOFDAY {#boosthasgettimeofday}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_HAS_GETTIMEOFDAY
#endif

#ifdef __USE_POSIX199309
#define BOOST_HAS_NANOSLEEP
#endif

`

### BOOST_HAS_NANOSLEEP {#boosthasnanosleep}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NANOSLEEP
#endif

#if defined(__GLIBC__) && defined(__GLIBC_PREREQ)
// __GLIBC_PRE`

### BOOST_HAS_STDINT_H {#boosthasstdinth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_HAS_STDINT_H
#endif
#endif

#if defined(__LIBCOMO__)
//
// como on linux doesn't have `

### BOOST_HAS_UNISTD_H {#boosthasunistdh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_HAS_UNISTD_H
#include <boost/config/posix_features.hpp>

#ifndef __GNUC__
//
// if the`

### BOOST_NO_STDC_NAMESPACE {#boostnostdcnamespace}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_NO_STDC_NAMESPACE
#endif

#if __LIBCOMO_VERSION__ <= 21
#define BOOST_NO_SWPRINTF
#end`

### BOOST_NO_SWPRINTF {#boostnoswprintf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_NO_SWPRINTF
#endif

// boilerplate code:
#define BOOST_HAS_UNISTD_H
#include <boost/co`

### BOOST_PLATFORM {#boostplatform}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define BOOST_PLATFORM "linux"

// make sure we have __GLIBC_PREREQ if available at all
#include <cs`


## _

### __const__ {#const}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define __const__ const
#endif
#ifndef __volatile__
#define __volatile__ volatile
#endif
#ifndef __s`

### __extension__ {#extension}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define __extension__
#endif
#ifndef __const__
#define __const__ const
#endif
#ifndef __volatile__
#`

### __inline__ {#inline}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define __inline__ inline
#endif
#endif
`

### __signed__ {#signed}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define __signed__ signed
#endif
#ifndef __typeof__
#define __typeof__ typeof
#endif
#ifndef __inlin`

### __typeof__ {#typeof}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define __typeof__ typeof
#endif
#ifndef __inline__
#define __inline__ inline
#endif
#endif
`

### __volatile__ {#volatile}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/linux.hpp](./linux.hpp_docs.md)
- **Context**: `#define __volatile__ volatile
#endif
#ifndef __signed__
#define __signed__ signed
#endif
#ifndef __t`

