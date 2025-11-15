# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp`
- **Filename**: `bcc_rounding_control.hpp`
- **Language**: hpp
- **Size**: 1609 bytes
- **Lines**: 63
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/detail/bcc_rounding_control.hpp file
 *
 * Copyright 2000 Jens Maurer
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_DETAIL_BCC_ROUNDING_CONTROL_HPP
#define BOOST_NUMERIC_INTERVAL_DETAIL_BCC_ROUNDING_CONTROL_HPP

#ifndef __BORLANDC__
#error This header is only intended for Borland C++.
#endif

#ifndef _M_IX86
#error This header only works on x86 CPUs.
#endif

#include <float.h> // Borland C++ rounding control

namespace boost {
namespace numeric {
namespace interval_lib {
namespace detail {

#ifndef BOOST_NUMERIC_INTERVAL_KEEP_EXCEPTIONS_FOR_BCC
extern "C"
{
    unsigned int _RTLENTRY _fm_init(void);
}

struct borland_workaround
{
    borland_workaround() { _fm_init(); }
};

static borland_workaround borland_workaround_exec;
#endif // BOOST_NUMERIC_INTERVAL_KEEP_EXCEPTIONS_FOR_BCC

__inline double rint(double)
{
    __emit__(0xD9);
    __emit__(0xFC); /* asm FRNDINT */
}

struct x86_rounding
{
    typedef unsigned int rounding_mode;
    static void          get_rounding_mode(rounding_mode &mode) { mode = _control87(0, 0); }
    static void          set_rounding_mode(const rounding_mode mode) { _control87(mode, 0xffff); }
    static double        to_int(const double &x) { return rint(x); }
};

} // namespace detail
} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif /* BOOST_NUMERIC_INTERVAL_DETAIL_BCC_ROUNDING_CONTROL_HPP */

```

---
## High-Level Overview
This file is a hpp source file with 4 function(s) and 2 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `float.h`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_DETAIL_BCC_ROUNDING_CONTROL_HPP**: `#ifndef __BORLANDC__`

### Functions
#### `double rint(double)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp

#### `void get_rounding_mode(rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp

#### `void set_rounding_mode(const rounding_mode mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp

#### `double to_int(const double &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp

### Classes / Structures
#### `struct borland_workaround`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp

#### `struct x86_rounding`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/bcc_rounding_control.hpp


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

