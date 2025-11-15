# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_transc.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_transc.hpp`
- **Filename**: `rounded_transc.hpp`
- **Language**: hpp
- **Size**: 7577 bytes
- **Lines**: 165
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/rounded_transc.hpp template implementation file
 *
 * Copyright 2002-2003 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_ROUNDED_TRANSC_HPP
#define BOOST_NUMERIC_INTERVAL_ROUNDED_TRANSC_HPP

#include <boost/config/no_tr1/cmath.hpp>
#include <boost/numeric/interval/detail/bugs.hpp>
#include <boost/numeric/interval/rounding.hpp>

namespace boost {
namespace numeric {
namespace interval_lib {

template <class T, class Rounding> struct rounded_transc_exact : Rounding
{
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        return f(x);                          \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        return f(x);                          \
    }
    BOOST_NUMERIC_INTERVAL_new_func(exp) BOOST_NUMERIC_INTERVAL_new_func(log) BOOST_NUMERIC_INTERVAL_new_func(sin)
        BOOST_NUMERIC_INTERVAL_new_func(cos) BOOST_NUMERIC_INTERVAL_new_func(tan) BOOST_NUMERIC_INTERVAL_new_func(asin)
            BOOST_NUMERIC_INTERVAL_new_func(acos) BOOST_NUMERIC_INTERVAL_new_func(atan)
                BOOST_NUMERIC_INTERVAL_new_func(sinh) BOOST_NUMERIC_INTERVAL_new_func(cosh)
                    BOOST_NUMERIC_INTERVAL_new_func(tanh)
#undef BOOST_NUMERIC_INTERVAL_new_func
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        return f(x);                          \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        return f(x);                          \
    }
                        BOOST_NUMERIC_INTERVAL_new_func(asinh) BOOST_NUMERIC_INTERVAL_new_func(acosh)
                            BOOST_NUMERIC_INTERVAL_new_func(atanh)
#undef BOOST_NUMERIC_INTERVAL_new_func
};

template <class T, class Rounding> struct rounded_transc_std : Rounding
{
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        this->downward();                     \
        return this->force_rounding(f(x));    \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        this->upward();                       \
        return this->force_rounding(f(x));    \
    }
    BOOST_NUMERIC_INTERVAL_new_func(exp) BOOST_NUMERIC_INTERVAL_new_func(log) BOOST_NUMERIC_INTERVAL_new_func(sin)
        BOOST_NUMERIC_INTERVAL_new_func(cos) BOOST_NUMERIC_INTERVAL_new_func(tan) BOOST_NUMERIC_INTERVAL_new_func(asin)
            BOOST_NUMERIC_INTERVAL_new_func(acos) BOOST_NUMERIC_INTERVAL_new_func(atan)
                BOOST_NUMERIC_INTERVAL_new_func(sinh) BOOST_NUMERIC_INTERVAL_new_func(cosh)
                    BOOST_NUMERIC_INTERVAL_new_func(tanh)
#undef BOOST_NUMERIC_INTERVAL_new_func
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        this->downward();                     \
        return this->force_rounding(f(x));    \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        this->upward();                       \
        return this->force_rounding(f(x));    \
    }
                        BOOST_NUMERIC_INTERVAL_new_func(asinh) BOOST_NUMERIC_INTERVAL_new_func(acosh)
                            BOOST_NUMERIC_INTERVAL_new_func(atanh)
#undef BOOST_NUMERIC_INTERVAL_new_func
};

template <class T, class Rounding> struct rounded_transc_opp : Rounding
{
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        this->downward();                     \
        T y = this->force_rounding(f(x));     \
        this->upward();                       \
        return y;                             \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        return this->force_rounding(f(x));    \
    }
    BOOST_NUMERIC_INTERVAL_new_func(exp) BOOST_NUMERIC_INTERVAL_new_func(log) BOOST_NUMERIC_INTERVAL_new_func(cos)
        BOOST_NUMERIC_INTERVAL_new_func(acos) BOOST_NUMERIC_INTERVAL_new_func(cosh)
#undef BOOST_NUMERIC_INTERVAL_new_func
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        return -this->force_rounding(-f(x));  \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_math(f); \
        return this->force_rounding(f(x));    \
    }
            BOOST_NUMERIC_INTERVAL_new_func(sin) BOOST_NUMERIC_INTERVAL_new_func(tan)
                BOOST_NUMERIC_INTERVAL_new_func(asin) BOOST_NUMERIC_INTERVAL_new_func(atan)
                    BOOST_NUMERIC_INTERVAL_new_func(sinh) BOOST_NUMERIC_INTERVAL_new_func(tanh)
#undef BOOST_NUMERIC_INTERVAL_new_func
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        this->downward();                     \
        T y = this->force_rounding(f(x));     \
        this->upward();                       \
        return y;                             \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        return this->force_rounding(f(x));    \
    }
                        BOOST_NUMERIC_INTERVAL_new_func(asinh) BOOST_NUMERIC_INTERVAL_new_func(atanh)
#undef BOOST_NUMERIC_INTERVAL_new_func
#define BOOST_NUMERIC_INTERVAL_new_func(f)    \
    T f##_down(const T &x)                    \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        return -this->force_rounding(-f(x));  \
    }                                         \
    T f##_up(const T &x)                      \
    {                                         \
        BOOST_NUMERIC_INTERVAL_using_ahyp(f); \
        return this->force_rounding(f(x));    \
    }
                            BOOST_NUMERIC_INTERVAL_new_func(acosh)
#undef BOOST_NUMERIC_INTERVAL_new_func
};

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_ROUNDED_TRANSC_HPP

```

---
## High-Level Overview
This file is a hpp source file in the CUDA Samples repository.

**Dependencies**: 3 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/config/no_tr1/cmath.hpp`
- `boost/numeric/interval/detail/bugs.hpp`
- `boost/numeric/interval/rounding.hpp`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_ROUNDED_TRANSC_HPP**: `#include <boost/config/no_tr1/cmath.hpp>`
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``
- **BOOST_NUMERIC_INTERVAL_new_func**: ``


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

