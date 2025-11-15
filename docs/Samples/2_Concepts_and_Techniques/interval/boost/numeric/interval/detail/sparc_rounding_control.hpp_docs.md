# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp`
- **Filename**: `sparc_rounding_control.hpp`
- **Language**: hpp
- **Size**: 3085 bytes
- **Lines**: 108
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/detail/sparc_rounding_control.hpp file
 *
 * Copyright 2000 Jens Maurer
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 *
 * The basic code in this file was kindly provided by Jeremy Siek.
 */

#ifndef BOOST_NUMERIC_INTERVAL_DETAIL_SPARC_ROUNDING_CONTROL_HPP
#define BOOST_NUMERIC_INTERVAL_DETAIL_SPARC_ROUNDING_CONTROL_HPP

#if !defined(sparc) && !defined(__sparc__)
#error This header is only intended for SPARC CPUs.
#endif

#ifdef __SUNPRO_CC
#include <ieeefp.h>
#endif


namespace boost {
namespace numeric {
namespace interval_lib {
namespace detail {

struct sparc_rounding_control
{
    typedef unsigned int rounding_mode;

    static void set_rounding_mode(const rounding_mode &mode)
    {
#if defined(__GNUC__)
        __asm__ __volatile__("ld %0, %%fsr" : : "m"(mode));
#elif defined(__SUNPRO_CC)
        fpsetround(fp_rnd(mode));
#elif defined(__KCC)
        asm("sethi %hi(mode), %o1");
        asm("ld [%o1+%lo(mode)], %fsr");
#else
#error Unsupported compiler for Sparc rounding control.
#endif
    }

    static void get_rounding_mode(rounding_mode &mode)
    {
#if defined(__GNUC__)
        __asm__ __volatile__("st %%fsr, %0" : "=m"(mode));
#elif defined(__SUNPRO_CC)
        mode = fpgetround();
#elif defined(__KCC)
#error KCC on Sun SPARC get_round_mode: please fix me
        asm("st %fsr, [mode]");
#else
#error Unsupported compiler for Sparc rounding control.
#endif
    }

#if defined(__SUNPRO_CC)
    static void downward() { set_rounding_mode(FP_RM); }
    static void upward() { set_rounding_mode(FP_RP); }
    static void to_nearest() { set_rounding_mode(FP_RN); }
    static void toward_zero() { set_rounding_mode(FP_RZ); }
#else
    static void downward() { set_rounding_mode(0xc0000000); }
    static void upward() { set_rounding_mode(0x80000000); }
    static void to_nearest() { set_rounding_mode(0x00000000); }
    static void toward_zero() { set_rounding_mode(0x40000000); }
#endif
};

} // namespace detail

extern "C"
{
    float  rintf(float);
    double rint(double);
}

template <> struct rounding_control<float> : detail::sparc_rounding_control
{
    static const float &force_rounding(const float &x) { return x; }
    static float        to_int(const float &x) { return rintf(x); }
};

template <> struct rounding_control<double> : detail::sparc_rounding_control
{
    static const double &force_rounding(const double &x) { return x; }
    static double        to_int(const double &x) { return rint(x); }
};

template <> struct rounding_control<long double> : detail::sparc_rounding_control
{
    static const long double &force_rounding(const long double &x) { return x; }
    static long double        to_int(const long double &x) { return rint(x); }
};

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#undef BOOST_NUMERIC_INTERVAL_NO_HARDWARE

#endif /* BOOST_NUMERIC_INTERVAL_DETAIL_SPARC_ROUNDING_CONTROL_HPP */

```

---
## High-Level Overview
This file is a hpp source file with 13 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `ieeefp.h`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_DETAIL_SPARC_ROUNDING_CONTROL_HPP**: `#if !defined(sparc) && !defined(__sparc__)`

### Functions
#### `void set_rounding_mode(const rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void get_rounding_mode(rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void downward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void upward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void to_nearest()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void toward_zero()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void downward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void upward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void to_nearest()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `void toward_zero()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `float to_int(const float &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `double to_int(const double &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

#### `double to_int(const long double &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp

### Classes / Structures
#### `struct sparc_rounding_control`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/sparc_rounding_control.hpp


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

