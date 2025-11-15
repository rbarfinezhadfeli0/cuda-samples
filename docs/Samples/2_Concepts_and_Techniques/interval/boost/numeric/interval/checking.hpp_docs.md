# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp`
- **Filename**: `checking.hpp`
- **Language**: hpp
- **Size**: 2900 bytes
- **Lines**: 110
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/checking.hpp template implementation file
 *
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_CHECKING_HPP
#define BOOST_NUMERIC_INTERVAL_CHECKING_HPP

#include <boost/limits.hpp>
#include <cassert>
#include <stdexcept>
#include <string>

namespace boost {
namespace numeric {
namespace interval_lib {

struct exception_create_empty
{
    void operator()() { throw std::runtime_error("boost::interval: empty interval created"); }
};

struct exception_invalid_number
{
    void operator()() { throw std::invalid_argument("boost::interval: invalid number"); }
};

template <class T> struct checking_base
{
    static T pos_inf()
    {
        assert(std::numeric_limits<T>::has_infinity);
        return std::numeric_limits<T>::infinity();
    }
    static T neg_inf()
    {
        assert(std::numeric_limits<T>::has_infinity);
        return -std::numeric_limits<T>::infinity();
    }
    static T nan()
    {
        assert(std::numeric_limits<T>::has_quiet_NaN);
        return std::numeric_limits<T>::quiet_NaN();
    }
    static bool is_nan(const T &x) { return std::numeric_limits<T>::has_quiet_NaN && (x != x); }
    static T    empty_lower()
    {
        return (std::numeric_limits<T>::has_quiet_NaN ? std::numeric_limits<T>::quiet_NaN() : static_cast<T>(1));
    }
    static T empty_upper()
    {
        return (std::numeric_limits<T>::has_quiet_NaN ? std::numeric_limits<T>::quiet_NaN() : static_cast<T>(0));
    }
    static bool is_empty(const T &l, const T &u)
    {
        return !(l <= u); // safety for partial orders
    }
};

template <class T, class Checking = checking_base<T>, class Exception = exception_create_empty>
struct checking_no_empty : Checking
{
    static T nan()
    {
        assert(false);
        return Checking::nan();
    }
    static T empty_lower()
    {
        Exception()();
        return Checking::empty_lower();
    }
    static T empty_upper()
    {
        Exception()();
        return Checking::empty_upper();
    }
    static bool is_empty(const T &, const T &) { return false; }
};

template <class T, class Checking = checking_base<T>> struct checking_no_nan : Checking
{
    static bool is_nan(const T &) { return false; }
};

template <class T, class Checking = checking_base<T>, class Exception = exception_invalid_number>
struct checking_catch_nan : Checking
{
    static bool is_nan(const T &x)
    {
        if (Checking::is_nan(x))
            Exception()();
        return false;
    }
};

template <class T> struct checking_strict : checking_no_nan<T, checking_no_empty<T>>
{
};

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_CHECKING_HPP

```

---
## High-Level Overview
This file is a hpp source file with 13 function(s) and 3 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/limits.hpp`
- `cassert`
- `stdexcept`
- `string`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_CHECKING_HPP**: `#include <boost/limits.hpp>`

### Functions
#### `T pos_inf()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T neg_inf()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T nan()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `bool is_nan(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T empty_lower()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T empty_upper()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `bool is_empty(const T &l, const T &u)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T nan()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T empty_lower()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `T empty_upper()`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `bool is_empty(const T &, const T &)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `bool is_nan(const T &)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `bool is_nan(const T &x)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

### Classes / Structures
#### `struct exception_create_empty`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `struct exception_invalid_number`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp

#### `struct checking_base`
- Defined in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/checking.hpp


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

