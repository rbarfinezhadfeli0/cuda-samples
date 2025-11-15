# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp`
- **Filename**: `msvc_rounding_control.hpp`
- **Language**: hpp
- **Size**: 4160 bytes
- **Lines**: 153
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/detail/msvc_rounding_control.hpp file
 *
 * Copyright 2000 Maarten Keijzer
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_DETAIL_MSVC_ROUNDING_CONTROL_HPP
#define BOOST_NUMERIC_INTERVAL_DETAIL_MSVC_ROUNDING_CONTROL_HPP

#ifndef _MSC_VER
#error This header is only intended for MSVC, but might work for Borland as well
#endif

#include <float.h> // MSVC rounding control

// Although the function is called _control87, it seems to work for
// other FPUs too, so it does not have to be changed to _controlfp.

namespace boost {
namespace numeric {
namespace interval_lib {
namespace detail {

extern "C"
{
    double rint(double);
}

struct x86_rounding
{
    static unsigned int hard2msvc(unsigned short m)
    {
        unsigned int n = 0;
        if (m & 0x01)
            n |= _EM_INVALID;
        if (m & 0x02)
            n |= _EM_DENORMAL;
        if (m & 0x04)
            n |= _EM_ZERODIVIDE;
        if (m & 0x08)
            n |= _EM_OVERFLOW;
        if (m & 0x10)
            n |= _EM_UNDERFLOW;
        if (m & 0x20)
            n |= _EM_INEXACT;
        switch (m & 0x300) {
        case 0x000:
            n |= _PC_24;
            break;
        case 0x200:
            n |= _PC_53;
            break;
        case 0x300:
            n |= _PC_64;
            break;
        }
        switch (m & 0xC00) {
        case 0x000:
            n |= _RC_NEAR;
            break;
        case 0x400:
            n |= _RC_DOWN;
            break;
        case 0x800:
            n |= _RC_UP;
            break;
        case 0xC00:
            n |= _RC_CHOP;
            break;
        }
        if (m & 0x1000)
            n |= _IC_AFFINE; // only useful on 287
        return n;
    }

    static unsigned short msvc2hard(unsigned int n)
    {
        unsigned short m = 0;
        if (n & _EM_INVALID)
            m |= 0x01;
        if (n & _EM_DENORMAL)
            m |= 0x02;
        if (n & _EM_ZERODIVIDE)
            m |= 0x04;
        if (n & _EM_OVERFLOW)
            m |= 0x08;
        if (n & _EM_UNDERFLOW)
            m |= 0x10;
        if (n & _EM_INEXACT)
            m |= 0x20;
        switch (n & _MCW_RC) {
        case _RC_NEAR:
            m |= 0x000;
            break;
        case _RC_DOWN:
            m |= 0x400;
            break;
        case _RC_UP:
            m |= 0x800;
            break;
        case _RC_CHOP:
            m |= 0xC00;
            break;
        }
        switch (n & _MCW_PC) {
        case _PC_24:
            m |= 0x000;
            break;
        case _PC_53:
            m |= 0x200;
            break;
        case _PC_64:
            m |= 0x300;
            break;
        }
        if ((n & _MCW_IC) == _IC_AFFINE)
            m |= 0x1000;
        return m;
    }

    typedef unsigned short rounding_mode;
    static void            get_rounding_mode(rounding_mode &mode) { mode = msvc2hard(_control87(0, 0)); }
    static void            set_rounding_mode(const rounding_mode mode)
    {
        _control87(hard2msvc(mode),
                   _MCW_EM
                       | _MCW_RC
// This is updated as per fix in boost 1.58
#if !defined(_M_AMD64) && !defined(_M_ARM)
                       // x64 ignores _MCW_PC and _MCW_IC, and the Debug CRT library actually
                       // asserts when these are passed to _control87.
                       // MSDN says on '_control87' that changing precision (_MCW_PC) or
                       // infinity (_MCW_IC) handling is not supported on the ARM and x64
                       // architectures and that _control87 raises an assertion
                       // and the invalid parameter handler is invoked.
                       | _MCW_PC | _MCW_IC
#endif
        );
    }
    static double to_int(const double &x) { return rint(x); }
};

} // namespace detail
} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif /* BOOST_NUMERIC_INTERVAL_DETAIL_MSVC_ROUNDING_CONTROL_HPP */

```

---
## High-Level Overview
This file is a hpp source file with 5 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `float.h`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_DETAIL_MSVC_ROUNDING_CONTROL_HPP**: `#ifndef _MSC_VER`

### Functions
#### `int hard2msvc(unsigned short m)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp

#### `short msvc2hard(unsigned int n)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp

#### `void get_rounding_mode(rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp

#### `void set_rounding_mode(const rounding_mode mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp

#### `double to_int(const double &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp

### Classes / Structures
#### `struct x86_rounding`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/msvc_rounding_control.hpp


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

