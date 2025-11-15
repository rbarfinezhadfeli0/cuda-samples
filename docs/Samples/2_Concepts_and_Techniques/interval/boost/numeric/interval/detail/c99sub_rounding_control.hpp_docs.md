# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp`
- **Filename**: `c99sub_rounding_control.hpp`
- **Language**: hpp
- **Size**: 1344 bytes
- **Lines**: 46
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/detail/c99sub_rounding_control.hpp file
 *
 * Copyright 2000 Jens Maurer
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_DETAIL_C99SUB_ROUNDING_CONTROL_HPP
#define BOOST_NUMERIC_INTERVAL_DETAIL_C99SUB_ROUNDING_CONTROL_HPP

#include <fenv.h> // ISO C 99 rounding mode control

namespace boost {
namespace numeric {
namespace interval_lib {
namespace detail {

extern "C"
{
    double rint(double);
}

struct c99_rounding
{
    typedef int rounding_mode;

    static void set_rounding_mode(const rounding_mode mode) { fesetround(mode); }
    static void get_rounding_mode(rounding_mode &mode) { mode = fegetround(); }
    static void downward() { set_rounding_mode(FE_DOWNWARD); }
    static void upward() { set_rounding_mode(FE_UPWARD); }
    static void to_nearest() { set_rounding_mode(FE_TONEAREST); }
    static void toward_zero() { set_rounding_mode(FE_TOWARDZERO); }

    template <class T> static T to_int(const T &r) { return rint(r); }
};

} // namespace detail
} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_DETAIL_C99SUB_ROUBDING_CONTROL_HPP

```

---
## High-Level Overview
This file is a hpp source file with 7 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `fenv.h`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_DETAIL_C99SUB_ROUNDING_CONTROL_HPP**: `#include <fenv.h> // ISO C 99 rounding mode control`

### Functions
#### `void set_rounding_mode(const rounding_mode mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

#### `void get_rounding_mode(rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

#### `void downward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

#### `void upward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

#### `void to_nearest()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

#### `void toward_zero()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

#### `T to_int(const T &r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp

### Classes / Structures
#### `struct c99_rounding`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/c99sub_rounding_control.hpp


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

