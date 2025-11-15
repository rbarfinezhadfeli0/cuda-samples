# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp`
- **Filename**: `rounded_arith.hpp`
- **Language**: hpp
- **Size**: 5746 bytes
- **Lines**: 159
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/rounded_arith.hpp template implementation file
 *
 * Copyright 2002-2003 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_ROUNDED_ARITH_HPP
#define BOOST_NUMERIC_INTERVAL_ROUNDED_ARITH_HPP

#include <boost/config/no_tr1/cmath.hpp>
#include <boost/numeric/interval/detail/bugs.hpp>
#include <boost/numeric/interval/rounding.hpp>

namespace boost {
namespace numeric {
namespace interval_lib {

/*
 * Three classes of rounding: exact, std, opp
 * See documentation for details.
 */

template <class T, class Rounding> struct rounded_arith_exact : Rounding
{
    void                 init() {}
    template <class U> T conv_down(U const &v) { return v; }
    template <class U> T conv_up(U const &v) { return v; }
    T                    add_down(const T &x, const T &y) { return x + y; }
    T                    add_up(const T &x, const T &y) { return x + y; }
    T                    sub_down(const T &x, const T &y) { return x - y; }
    T                    sub_up(const T &x, const T &y) { return x - y; }
    T                    mul_down(const T &x, const T &y) { return x * y; }
    T                    mul_up(const T &x, const T &y) { return x * y; }
    T                    div_down(const T &x, const T &y) { return x / y; }
    T                    div_up(const T &x, const T &y) { return x / y; }
    T                    median(const T &x, const T &y) { return (x + y) / 2; }
    T                    sqrt_down(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(sqrt);
        return sqrt(x);
    }
    T sqrt_up(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(sqrt);
        return sqrt(x);
    }
    T int_down(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(floor);
        return floor(x);
    }
    T int_up(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(ceil);
        return ceil(x);
    }
};

template <class T, class Rounding> struct rounded_arith_std : Rounding
{
#define BOOST_DN(EXPR) \
    this->downward();  \
    return this->force_rounding(EXPR)
#define BOOST_NR(EXPR)  \
    this->to_nearest(); \
    return this->force_rounding(EXPR)
#define BOOST_UP(EXPR) \
    this->upward();    \
    return this->force_rounding(EXPR)
    void                 init() {}
    template <class U> T conv_down(U const &v) { BOOST_DN(v); }
    template <class U> T conv_up(U const &v) { BOOST_UP(v); }
    T                    add_down(const T &x, const T &y) { BOOST_DN(x + y); }
    T                    sub_down(const T &x, const T &y) { BOOST_DN(x - y); }
    T                    mul_down(const T &x, const T &y) { BOOST_DN(x * y); }
    T                    div_down(const T &x, const T &y) { BOOST_DN(x / y); }
    T                    add_up(const T &x, const T &y) { BOOST_UP(x + y); }
    T                    sub_up(const T &x, const T &y) { BOOST_UP(x - y); }
    T                    mul_up(const T &x, const T &y) { BOOST_UP(x * y); }
    T                    div_up(const T &x, const T &y) { BOOST_UP(x / y); }
    T                    median(const T &x, const T &y) { BOOST_NR((x + y) / 2); }
    T                    sqrt_down(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(sqrt);
        BOOST_DN(sqrt(x));
    }
    T sqrt_up(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(sqrt);
        BOOST_UP(sqrt(x));
    }
    T int_down(const T &x)
    {
        this->downward();
        return to_int(x);
    }
    T int_up(const T &x)
    {
        this->upward();
        return to_int(x);
    }
#undef BOOST_DN
#undef BOOST_NR
#undef BOOST_UP
};

template <class T, class Rounding> struct rounded_arith_opp : Rounding
{
    void init() { this->upward(); }
#define BOOST_DN(EXPR)                \
    this->downward();                 \
    T r = this->force_rounding(EXPR); \
    this->upward();                   \
    return r
#define BOOST_NR(EXPR)                \
    this->to_nearest();               \
    T r = this->force_rounding(EXPR); \
    this->upward();                   \
    return r
#define BOOST_UP(EXPR)     return this->force_rounding(EXPR)
#define BOOST_UP_NEG(EXPR) return -this->force_rounding(EXPR)
    template <class U> T conv_down(U const &v) { BOOST_UP_NEG(-v); }
    template <class U> T conv_up(U const &v) { BOOST_UP(v); }
    T                    add_down(const T &x, const T &y) { BOOST_UP_NEG((-x) - y); }
    T                    sub_down(const T &x, const T &y) { BOOST_UP_NEG(y - x); }
    T                    mul_down(const T &x, const T &y) { BOOST_UP_NEG(x * (-y)); }
    T                    div_down(const T &x, const T &y) { BOOST_UP_NEG(x / (-y)); }
    T                    add_up(const T &x, const T &y) { BOOST_UP(x + y); }
    T                    sub_up(const T &x, const T &y) { BOOST_UP(x - y); }
    T                    mul_up(const T &x, const T &y) { BOOST_UP(x * y); }
    T                    div_up(const T &x, const T &y) { BOOST_UP(x / y); }
    T                    median(const T &x, const T &y) { BOOST_NR((x + y) / 2); }
    T                    sqrt_down(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(sqrt);
        BOOST_DN(sqrt(x));
    }
    T sqrt_up(const T &x)
    {
        BOOST_NUMERIC_INTERVAL_using_math(sqrt);
        BOOST_UP(sqrt(x));
    }
    T int_down(const T &x) { return -to_int(-x); }
    T int_up(const T &x) { return to_int(x); }
#undef BOOST_DN
#undef BOOST_NR
#undef BOOST_UP
#undef BOOST_UP_NEG
};

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_ROUNDED_ARITH_HPP

```

---
## High-Level Overview
This file is a hpp source file with 48 function(s) in the CUDA Samples repository.

**Dependencies**: 3 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/config/no_tr1/cmath.hpp`
- `boost/numeric/interval/detail/bugs.hpp`
- `boost/numeric/interval/rounding.hpp`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_ROUNDED_ARITH_HPP**: `#include <boost/config/no_tr1/cmath.hpp>`
- **BOOST_DN**: ``
- **BOOST_NR**: ``
- **BOOST_UP**: ``
- **BOOST_DN**: ``
- **BOOST_NR**: ``
- **BOOST_UP**: ``
- **BOOST_UP_NEG**: ``

### Functions
#### `void init()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T conv_down(U const &v)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T conv_up(U const &v)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T add_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T add_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sub_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sub_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T mul_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T mul_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T div_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T div_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T median(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sqrt_down(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sqrt_up(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T int_down(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T int_up(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `void init()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T conv_down(U const &v)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T conv_up(U const &v)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T add_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sub_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T mul_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T div_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T add_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sub_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T mul_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T div_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T median(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sqrt_down(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sqrt_up(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T int_down(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T int_up(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `void init()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T conv_down(U const &v)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T conv_up(U const &v)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T add_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sub_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T mul_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T div_down(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T add_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sub_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T mul_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T div_up(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T median(const T &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sqrt_down(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T sqrt_up(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T int_down(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp

#### `T int_up(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp


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

