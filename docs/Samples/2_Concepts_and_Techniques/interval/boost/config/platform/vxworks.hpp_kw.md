# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp
---

**Total Keywords**: 8

---

## B

### BOOST_ASIO_DISABLE_SERIAL_PORT {#boostasiodisableserialport}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define BOOST_ASIO_DISABLE_SERIAL_PORT

// boilerplate code:
#include <boost/config/posix_features.h`

### BOOST_HAS_UNISTD_H {#boosthasunistdh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define BOOST_HAS_UNISTD_H

// these allow posix_features to work, since vxWorks doesn't
// define t`

### BOOST_NO_CWCHAR {#boostnocwchar}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define BOOST_NO_CWCHAR
#define BOOST_NO_INTRINSIC_WCHAR_T

#if defined(__GNUC__) && defined(__STRIC`

### BOOST_NO_INT64_T {#boostnoint64t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define BOOST_NO_INT64_T
#endif

#define BOOST_HAS_UNISTD_H

// these allow posix_features to work, `

### BOOST_NO_INTRINSIC_WCHAR_T {#boostnointrinsicwchart}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define BOOST_NO_INTRINSIC_WCHAR_T

#if defined(__GNUC__) && defined(__STRICT_ANSI__)
#define BOOST_`

### BOOST_PLATFORM {#boostplatform}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define BOOST_PLATFORM "vxWorks"

#define BOOST_NO_CWCHAR
#define BOOST_NO_INTRINSIC_WCHAR_T

#if de`


## _

### _POSIX_THREADS {#posixthreads}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define _POSIX_THREADS 1

// vxworks doesn't work with asio serial ports
#define BOOST_ASIO_DISABLE_`

### _POSIX_TIMERS {#posixtimers}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/platform/vxworks.hpp](./vxworks.hpp_docs.md)
- **Context**: `#define _POSIX_TIMERS  1
#define _POSIX_THREADS 1

// vxworks doesn't work with asio serial ports
#d`

