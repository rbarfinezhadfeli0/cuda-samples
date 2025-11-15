# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/possible.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/possible.hpp`
- **Filename**: `possible.hpp`
- **Language**: hpp
- **Size**: 3571 bytes
- **Lines**: 120
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/compare/possible.hpp template implementation file
 *
 * Copyright 2003 Guillaume Melquiond
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_COMPARE_POSSIBLE_HPP
#define BOOST_NUMERIC_INTERVAL_COMPARE_POSSIBLE_HPP

#include <boost/numeric/interval/detail/interval_prototype.hpp>
#include <boost/numeric/interval/detail/test_input.hpp>

namespace boost {
namespace numeric {
namespace interval_lib {
namespace compare {
namespace possible {

template <class T, class Policies1, class Policies2>
inline bool operator<(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() < y.upper();
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
    return x.lower() <= y.upper();
}

template <class T, class Policies> inline bool operator<=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() <= y;
}

template <class T, class Policies1, class Policies2>
inline bool operator>(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.upper() > y.lower();
}

template <class T, class Policies> inline bool operator>(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.upper() > y;
}

template <class T, class Policies1, class Policies2>
inline bool operator>=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.upper() >= y.lower();
}

template <class T, class Policies> inline bool operator>=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.upper() >= y;
}

template <class T, class Policies1, class Policies2>
inline bool operator==(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() <= y.upper() && x.upper() >= y.lower();
}

template <class T, class Policies> inline bool operator==(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() <= y && x.upper() >= y;
}

template <class T, class Policies1, class Policies2>
inline bool operator!=(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() != y.upper() || x.upper() != y.lower();
}

template <class T, class Policies> inline bool operator!=(const interval<T, Policies> &x, const T &y)
{
    if (detail::test_input(x, y))
        throw comparison_error();
    return x.lower() != y || x.upper() != y;
}

} // namespace possible
} // namespace compare
} // namespace interval_lib
} // namespace numeric
} // namespace boost


#endif // BOOST_NUMERIC_INTERVAL_COMPARE_POSSIBLE_HPP

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
- **BOOST_NUMERIC_INTERVAL_COMPARE_POSSIBLE_HPP**: `#include <boost/numeric/interval/detail/interval_prototype.hpp>`


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

