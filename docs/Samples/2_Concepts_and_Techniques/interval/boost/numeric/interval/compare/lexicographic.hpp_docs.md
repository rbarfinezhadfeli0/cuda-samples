# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/lexicographic.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/lexicographic.hpp`
- **Filename**: `lexicographic.hpp`
- **Language**: hpp
- **Size**: 4041 bytes
- **Lines**: 129
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/compare/lexicographic.hpp template implementation file
 *
 * Copyright 2002-2003 Guillaume Melquiond
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_COMPARE_LEXICOGRAPHIC_HPP
#define BOOST_NUMERIC_INTERVAL_COMPARE_LEXICOGRAPHIC_HPP

#include <boost/numeric/interval/detail/interval_prototype.hpp>
#include <boost/numeric/interval/detail/test_input.hpp>

namespace boost {
namespace numeric {
namespace interval_lib {
namespace compare {
namespace lexicographic {

template <class T, class Policies1, class Policies2>
inline bool operator<(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    const T &xl = x.lower();
    const T &yl = y.lower();
    return xl < yl || (xl == yl && x.upper() < y.upper());
}

template <class T, class Policies> inline bool operator<(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() < y;
}

template <class T, class Policies1, class Policies2>
inline bool operator<=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    const T &xl = x.lower();
    const T &yl = y.lower();
    return xl < yl || (xl == yl && x.upper() <= y.upper());
}

template <class T, class Policies> inline bool operator<=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    const T &xl = x.lower();
    return xl < y || (xl == y && x.upper() <= y);
}

template <class T, class Policies1, class Policies2>
inline bool operator>(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    const T &xl = x.lower();
    const T &yl = y.lower();
    return xl > yl || (xl == yl && x.upper() > y.upper());
}

template <class T, class Policies> inline bool operator>(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    const T &xl = x.lower();
    return xl > y || (xl == y && x.upper() > y);
}

template <class T, class Policies1, class Policies2>
inline bool operator>=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    const T &xl = x.lower();
    const T &yl = y.lower();
    return xl > yl || (xl == yl && x.upper() >= y.upper());
}

template <class T, class Policies> inline bool operator>=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() >= y;
}

template <class T, class Policies1, class Policies2>
inline bool operator==(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() == y.lower() && x.upper() == y.upper();
}

template <class T, class Policies> inline bool operator==(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() == y && x.upper() == y;
}

template <class T, class Policies1, class Policies2>
inline bool operator!=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() != y.lower() || x.upper() != y.upper();
}

template <class T, class Policies> inline bool operator!=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() != y || x.upper() != y;
}

} // namespace lexicographic
} // namespace compare
} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_COMPARE_LEXICOGRAPHIC_HPP

```

---
## High-Level Overview
This file is a hpp source file in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/numeric/interval/detail/interval_prototype.hpp`
- `boost/numeric/interval/detail/test_input.hpp`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_COMPARE_LEXICOGRAPHIC_HPP**: `#include <boost/numeric/interval/detail/interval_prototype.hpp>`


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

