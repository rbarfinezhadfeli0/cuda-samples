# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp`
- **Filename**: `ppc_rounding_control.hpp`
- **Language**: hpp
- **Size**: 3159 bytes
- **Lines**: 94
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/detail/ppc_rounding_control.hpp file
 *
 * Copyright 2000 Jens Maurer
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 * Copyright 2005 Guillaume Melquiond
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_DETAIL_PPC_ROUNDING_CONTROL_HPP
#define BOOST_NUMERIC_INTERVAL_DETAIL_PPC_ROUNDING_CONTROL_HPP

#if !defined(powerpc) && !defined(__powerpc__) && !defined(__ppc__)
#error This header only works on PPC CPUs.
#endif

#if defined(__GNUC__) || (__IBMCPP__ >= 700)

namespace boost {
namespace numeric {
namespace interval_lib {
namespace detail {

typedef union
{
    ::boost::long_long_type imode;
    double                  dmode;
} rounding_mode_struct;

static const rounding_mode_struct mode_upward      = {static_cast<boost::long_long_type>(0xFFF8000000000002LL)};
static const rounding_mode_struct mode_downward    = {static_cast<boost::long_long_type>(0xFFF8000000000003LL)};
static const rounding_mode_struct mode_to_nearest  = {static_cast<boost::long_long_type>(0xFFF8000000000000LL)};
static const rounding_mode_struct mode_toward_zero = {static_cast<boost::long_long_type>(0xFFF8000000000001LL)};

struct ppc_rounding_control
{
    typedef double rounding_mode;

    static void set_rounding_mode(const rounding_mode mode) { __asm__ __volatile__("mtfsf 255,%0" : : "f"(mode)); }

    static void get_rounding_mode(rounding_mode &mode) { __asm__ __volatile__("mffs %0" : "=f"(mode)); }

    static void downward() { set_rounding_mode(mode_downward.dmode); }
    static void upward() { set_rounding_mode(mode_upward.dmode); }
    static void to_nearest() { set_rounding_mode(mode_to_nearest.dmode); }
    static void toward_zero() { set_rounding_mode(mode_toward_zero.dmode); }
};

} // namespace detail

// Do not declare the following C99 symbols if <math.h> provides them.
// Otherwise, conflicts may occur, due to differences between prototypes.
#if !defined(_ISOC99_SOURCE) && !defined(__USE_ISOC99)
extern "C"
{
    float  rintf(float);
    double rint(double);
}
#endif

template <> struct rounding_control<float> : detail::ppc_rounding_control
{
    static float force_rounding(const float r)
    {
        float tmp;
        __asm__ __volatile__("frsp %0, %1" : "=f"(tmp) : "f"(r));
        return tmp;
    }
    static float to_int(const float &x) { return rintf(x); }
};

template <> struct rounding_control<double> : detail::ppc_rounding_control
{
    static const double &force_rounding(const double &r) { return r; }
    static double        to_int(const double &r) { return rint(r); }
};

template <> struct rounding_control<long double> : detail::ppc_rounding_control
{
    static const long double &force_rounding(const long double &r) { return r; }
    static long double        to_int(const long double &r) { return rint(static_cast<double>(r)); }
};

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#undef BOOST_NUMERIC_INTERVAL_NO_HARDWARE
#endif

#endif /* BOOST_NUMERIC_INTERVAL_DETAIL_PPC_ROUNDING_CONTROL_HPP */

```

---
## High-Level Overview
This file is a hpp source file with 10 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_DETAIL_PPC_ROUNDING_CONTROL_HPP**: `#if !defined(powerpc) && !defined(__powerpc__) && !defined(__ppc__)`

### Functions
#### `void set_rounding_mode(const rounding_mode mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `void get_rounding_mode(rounding_mode &mode)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `void downward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `void upward()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `void to_nearest()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `void toward_zero()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `float force_rounding(const float r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `float to_int(const float &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `double to_int(const double &r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

#### `double to_int(const long double &r)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp

### Classes / Structures
#### `struct ppc_rounding_control`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/detail/ppc_rounding_control.hpp


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

