# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/tribool.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/tribool.hpp`
- **Filename**: `tribool.hpp`
- **Language**: hpp
- **Size**: 5013 bytes
- **Lines**: 169
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/compare/tribool.hpp template implementation file
 *
 * Copyright 2002-2003 Guillaume Melquiond
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_COMPARE_TRIBOOL_HPP
#define BOOST_NUMERIC_INTERVAL_COMPARE_TRIBOOL_HPP

#include <boost/logic/tribool.hpp>
#include <boost/numeric/interval/detail/interval_prototype.hpp>
#include <boost/numeric/interval/detail/test_input.hpp>

namespace boost {
namespace numeric {
namespace interval_lib {
namespace compare {
namespace tribool {

template <class T, class Policies1, class Policies2>
inline logic::tribool operator<(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() < y.lower())
        return true;
    if (x.lower() >= y.upper())
        return false;
    return logic::indeterminate;
}

template <class T, class Policies> inline logic::tribool operator<(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() < y)
        return true;
    if (x.lower() >= y)
        return false;
    return logic::indeterminate;
}

template <class T, class Policies1, class Policies2>
inline logic::tribool operator<=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() <= y.lower())
        return true;
    if (x.lower() > y.upper())
        return false;
    return logic::indeterminate;
}

template <class T, class Policies> inline logic::tribool operator<=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() <= y)
        return true;
    if (x.lower() > y)
        return false;
    return logic::indeterminate;
}

template <class T, class Policies1, class Policies2>
inline logic::tribool operator>(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.lower() > y.upper())
        return true;
    if (x.upper() <= y.lower())
        return false;
    return logic::indeterminate;
}

template <class T, class Policies> inline logic::tribool operator>(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.lower() > y)
        return true;
    if (x.upper() <= y)
        return false;
    return logic::indeterminate;
}

template <class T, class Policies1, class Policies2>
inline logic::tribool operator>=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.lower() >= y.upper())
        return true;
    if (x.upper() < y.lower())
        return false;
    return logic::indeterminate;
}

template <class T, class Policies> inline logic::tribool operator>=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.lower() >= y)
        return true;
    if (x.upper() < y)
        return false;
    return logic::indeterminate;
}

template <class T, class Policies1, class Policies2>
inline logic::tribool operator==(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() == y.lower() && x.lower() == y.upper())
        return true;
    if (x.upper() < y.lower() || x.lower() > y.upper())
        return false;
    return logic::indeterminate;
}

template <class T, class Policies> inline logic::tribool operator==(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() == y && x.lower() == y)
        return true;
    if (x.upper() < y || x.lower() > y)
        return false;
    return logic::indeterminate;
}

template <class T, class Policies1, class Policies2>
inline logic::tribool operator!=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() < y.lower() || x.lower() > y.upper())
        return true;
    if (x.upper() == y.lower() && x.lower() == y.upper())
        return false;
    return logic::indeterminate;
}

template <class T, class Policies> inline logic::tribool operator!=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    if (x.upper() < y || x.lower() > y)
        return true;
    if (x.upper() == y && x.lower() == y)
        return false;
    return logic::indeterminate;
}

} // namespace tribool
} // namespace compare
} // namespace interval_lib
} // namespace numeric
} // namespace boost


#endif // BOOST_NUMERIC_INTERVAL_COMPARE_TRIBOOL_HPP

```

---
## High-Level Overview
This file is a hpp source file in the CUDA Samples repository.

**Dependencies**: 3 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/logic/tribool.hpp`
- `boost/numeric/interval/detail/interval_prototype.hpp`
- `boost/numeric/interval/detail/test_input.hpp`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_COMPARE_TRIBOOL_HPP**: `#include <boost/logic/tribool.hpp>`


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

