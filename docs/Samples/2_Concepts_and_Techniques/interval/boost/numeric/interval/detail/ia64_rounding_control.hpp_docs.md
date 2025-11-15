# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp`
- **Filename**: `ia64_rounding_control.hpp`
- **Language**: hpp
- **Size**: 2184 bytes
- **Lines**: 80
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/detail/ia64_rounding_control.hpp file
 *
 * Copyright 2006-2007 Boris Gubenko
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_DETAIL_IA64_ROUNDING_CONTROL_HPP
#define BOOST_NUMERIC_INTERVAL_DETAIL_IA64_ROUNDING_CONTROL_HPP

#if !defined(ia64) && !defined(__ia64) && !defined(__ia64__)
#error This header only works on ia64 CPUs.
#endif

#if defined(__hpux)

#include <fenv.h>

namespace boost {
namespace numeric {
namespace interval_lib {
namespace detail {


struct ia64_rounding_control
{
    typedef unsigned int rounding_mode;

    static void set_rounding_mode(const rounding_mode &mode) { fesetround(mode); }
    static void get_rounding_mode(rounding_mode &mode) { mode = fegetround(); }

    static void downward() { set_rounding_mode(FE_DOWNWARD); }
    static void upward() { set_rounding_mode(FE_UPWARD); }
    static void to_nearest() { set_rounding_mode(FE_TONEAREST); }
    static void toward_zero() { set_rounding_mode(FE_TOWARDZERO); }
};

} // namespace detail

extern "C"
{
    float       rintf(float);
    double      rint(double);
    long double rintl(long double);
}

template <> struct rounding_control<float> : detail::ia64_rounding_control
{
    static float force_rounding(const float r)
    {
        volatile float _r = r;
        return _r;
    }
    static float to_int(const float &x) { return rintf(x); }
};

template <> struct rounding_control<double> : detail::ia64_rounding_control
{
    static const double &force_rounding(const double &r) { return r; }
    static double        to_int(const double &r) { return rint(r); }
};

template <> struct rounding_control<long double> : detail::ia64_rounding_control
{
    static const long double &force_rounding(const long double &r) { return r; }
    static long double        to_int(const long double &r) { return rintl(r); }
};

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#undef BOOST_NUMERIC_INTERVAL_NO_HARDWARE

#endif /* __hpux */

#endif /* BOOST_NUMERIC_INTERVAL_DETAIL_IA64_ROUNDING_CONTROL_HPP */

```

---
## High-Level Overview
This file is a hpp source file with 10 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `fenv.h`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_DETAIL_IA64_ROUNDING_CONTROL_HPP**: `#if !defined(ia64) && !defined(__ia64) && !defined(__ia64__)`

### Functions
#### `void set_rounding_mode(const rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `void get_rounding_mode(rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `void downward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `void upward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `void to_nearest()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `void toward_zero()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `float force_rounding(const float r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `float to_int(const float &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `double to_int(const double &r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

#### `double to_int(const long double &r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp

### Classes / Structures
#### `struct ia64_rounding_control`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ia64_rounding_control.hpp


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

