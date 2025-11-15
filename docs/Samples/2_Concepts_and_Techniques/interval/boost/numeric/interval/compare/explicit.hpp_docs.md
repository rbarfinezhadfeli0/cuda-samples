# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp`
- **Filename**: `explicit.hpp`
- **Language**: hpp
- **Size**: 6214 bytes
- **Lines**: 225
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
/* Boost interval/compare/explicit.hpp template implementation file
 *
 * Copyright 2000 Jens Maurer
 * Copyright 2002 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_COMPARE_EXPLICIT_HPP
#define BOOST_NUMERIC_INTERVAL_COMPARE_EXPLICIT_HPP

#include <boost/numeric/interval/detail/interval_prototype.hpp>

namespace boost {
namespace numeric {
namespace interval_lib {

/*
 * Certainly... operations
 */

template <class T, class Policies1, class Policies2>
inline bool cerlt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() < y.lower();
}

template <class T, class Policies> inline bool cerlt(const interval<T, Policies> &x, const T &y)
{
    return x.upper() < y;
}

template <class T, class Policies> inline bool cerlt(const T &x, const interval<T, Policies> &y)
{
    return x < y.lower();
}

template <class T, class Policies1, class Policies2>
inline bool cerle(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() <= y.lower();
}

template <class T, class Policies> inline bool cerle(const interval<T, Policies> &x, const T &y)
{
    return x.upper() <= y;
}

template <class T, class Policies> inline bool cerle(const T &x, const interval<T, Policies> &y)
{
    return x <= y.lower();
}

template <class T, class Policies1, class Policies2>
inline bool cergt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.lower() > y.upper();
}

template <class T, class Policies> inline bool cergt(const interval<T, Policies> &x, const T &y)
{
    return x.lower() > y;
}

template <class T, class Policies> inline bool cergt(const T &x, const interval<T, Policies> &y)
{
    return x > y.upper();
}

template <class T, class Policies1, class Policies2>
inline bool cerge(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.lower() >= y.upper();
}

template <class T, class Policies> inline bool cerge(const interval<T, Policies> &x, const T &y)
{
    return x.lower() >= y;
}

template <class T, class Policies> inline bool cerge(const T &x, const interval<T, Policies> &y)
{
    return x >= y.upper();
}

template <class T, class Policies1, class Policies2>
inline bool cereq(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.lower() == y.upper() && y.lower() == x.upper();
}

template <class T, class Policies> inline bool cereq(const interval<T, Policies> &x, const T &y)
{
    return x.lower() == y && x.upper() == y;
}

template <class T, class Policies> inline bool cereq(const T &x, const interval<T, Policies> &y)
{
    return x == y.lower() && x == y.upper();
}

template <class T, class Policies1, class Policies2>
inline bool cerne(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() < y.lower() || y.upper() < x.lower();
}

template <class T, class Policies> inline bool cerne(const interval<T, Policies> &x, const T &y)
{
    return x.upper() < y || y < x.lower();
}

template <class T, class Policies> inline bool cerne(const T &x, const interval<T, Policies> &y)
{
    return x < y.lower() || y.upper() < x;
}

/*
 * Possibly... comparisons
 */

template <class T, class Policies1, class Policies2>
inline bool poslt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.lower() < y.upper();
}

template <class T, class Policies> inline bool poslt(const interval<T, Policies> &x, const T &y)
{
    return x.lower() < y;
}

template <class T, class Policies> inline bool poslt(const T &x, const interval<T, Policies> &y)
{
    return x < y.upper();
}

template <class T, class Policies1, class Policies2>
inline bool posle(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.lower() <= y.upper();
}

template <class T, class Policies> inline bool posle(const interval<T, Policies> &x, const T &y)
{
    return x.lower() <= y;
}

template <class T, class Policies> inline bool posle(const T &x, const interval<T, Policies> &y)
{
    return x <= y.upper();
}

template <class T, class Policies1, class Policies2>
inline bool posgt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() > y.lower();
}

template <class T, class Policies> inline bool posgt(const interval<T, Policies> &x, const T &y)
{
    return x.upper() > y;
}

template <class T, class Policies> inline bool posgt(const T &x, const interval<T, Policies> &y)
{
    return x > y.lower();
}

template <class T, class Policies1, class Policies2>
inline bool posge(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() >= y.lower();
}

template <class T, class Policies> inline bool posge(const interval<T, Policies> &x, const T &y)
{
    return x.upper() >= y;
}

template <class T, class Policies> inline bool posge(const T &x, const interval<T, Policies> &y)
{
    return x >= y.lower();
}

template <class T, class Policies1, class Policies2>
inline bool poseq(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() >= y.lower() && y.upper() >= x.lower();
}

template <class T, class Policies> inline bool poseq(const interval<T, Policies> &x, const T &y)
{
    return x.upper() >= y && y >= x.lower();
}

template <class T, class Policies> inline bool poseq(const T &x, const interval<T, Policies> &y)
{
    return x >= y.lower() && y.upper() >= x;
}

template <class T, class Policies1, class Policies2>
inline bool posne(const interval<T, Policies1> &x, const interval<T, Policies2> &y)
{
    return x.upper() != y.lower() || y.upper() != x.lower();
}

template <class T, class Policies> inline bool posne(const interval<T, Policies> &x, const T &y)
{
    return x.upper() != y || y != x.lower();
}

template <class T, class Policies> inline bool posne(const T &x, const interval<T, Policies> &y)
{
    return x != y.lower() || y.upper() != x;
}

} // namespace interval_lib
} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_COMPARE_EXPLICIT_HPP

```

---
## High-Level Overview
This file is a hpp source file with 36 function(s) in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/numeric/interval/detail/interval_prototype.hpp`

### Preprocessor Definitions
- **BOOST_NUMERIC_INTERVAL_COMPARE_EXPLICIT_HPP**: `#include <boost/numeric/interval/detail/interval_prototype.hpp>`

### Functions
#### `bool cerlt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerlt(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerlt(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerle(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerle(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerle(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cergt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cergt(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cergt(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerge(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerge(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerge(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cereq(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cereq(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cereq(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerne(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerne(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool cerne(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool poslt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool poslt(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool poslt(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posle(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posle(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posle(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posgt(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posgt(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posgt(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posge(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posge(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posge(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool poseq(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool poseq(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool poseq(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posne(const interval<T, Policies1> &x, const interval<T, Policies2> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posne(const interval<T, Policies> &x, const T &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp

#### `bool posne(const T &x, const interval<T, Policies> &y)`
- Function in Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/compare/explicit.hpp


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

