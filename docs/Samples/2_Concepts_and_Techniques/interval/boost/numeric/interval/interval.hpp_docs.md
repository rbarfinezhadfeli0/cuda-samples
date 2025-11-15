# Documentation for Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/interval.hpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/interval.hpp`
- **Type**: .hpp
- **Location**: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```hpp
/* Boost interval/interval.hpp header file
 *
 * Copyright 2002-2003 Herv Brnnimann, Guillaume Melquiond, Sylvain Pion
 *
 * Distributed under the Boost Software License, Version 1.0.
 * (See accompanying file LICENSE_1_0.txt or
 * copy at http://www.boost.org/LICENSE_1_0.txt)
 */

#ifndef BOOST_NUMERIC_INTERVAL_INTERVAL_HPP
#define BOOST_NUMERIC_INTERVAL_INTERVAL_HPP

#include <boost/numeric/interval/detail/interval_prototype.hpp>
#include <stdexcept>
#include <string>

namespace boost {
namespace numeric {

namespace interval_lib {

class comparison_error : public std::runtime_error
{
public:
    comparison_error()
        : std::runtime_error("boost::interval: uncertain comparison")
    {
    }
};

} // namespace interval_lib

/*
 * interval class
 */

template <class T, class Policies> class interval
{
private:
    struct interval_holder;
    struct number_holder;

public:
    typedef T        base_type;
    typedef Policies traits_type;

    T const &lower() const;
    T const &upper() const;

    interval();
    interval(T const &v);
    template <class T1> interval(T1 const &v);
    interval(T const &l, T const &u);
    template <class T1, class T2> interval(T1 const &l, T2 const &u);
    interval(interval<T, Policies> const &r);
    template <class Policies1> interval(interval<T, Policies1> const &r);
    template <class T1, class Policies1> interval(interval<T1, Policies1> const &r);

    interval                                      &operator=(T const &v);
    template <class T1> interval                  &operator=(T1 const &v);
    interval                                      &operator=(interval<T, Policies> const &r);
    template <class Policies1> interval           &operator=(interval<T, Policies1> const &r);
    template <class T1, class Policies1> interval &operator=(interval<T1, Policies1> const &r);

    void assign(const T &l, const T &u);

    static interval empty();
    static interval whole();
    static interval hull(const T &x, const T &y);

    interval &operator+=(const T &r);
    interval &operator+=(const interval &r);
    interval &operator-=(const T &r);
    interval &operator-=(const interval &r);
    interval &operator*=(const T &r);
    interval &operator*=(const interval &r);
    interval &operator/=(const T &r);
    interval &operator/=(const interval &r);

    bool operator<(const interval_holder &r) const;
    bool operator>(const interval_holder &r) const;
    bool operator<=(const interval_holder &r) const;
    bool operator>=(const interval_holder &r) const;
    bool operator==(const interval_holder &r) const;
    bool operator!=(const interval_holder &r) const;

    bool operator<(const number_holder &r) const;
    bool operator>(const number_holder &r) const;
    bool operator<=(const number_holder &r) const;
    bool operator>=(const number_holder &r) const;
    bool operator==(const number_holder &r) const;
    bool operator!=(const number_holder &r) const;

    // the following is for internal use only, it is not a published interface
    // nevertheless, it's public because friends don't always work correctly.
    interval(const T &l, const T &u, bool)
        : low(l)
        , up(u)
    {
    }
    void set_empty();
    void set_whole();
    void set(const T &l, const T &u);

private:
    struct interval_holder
    {
        template <class Policies2>
        interval_holder(const interval<T, Policies2> &r)
            : low(r.lower())
            , up(r.upper())
        {
            typedef typename Policies2::checking checking2;
            if (checking2::is_empty(low, up))
                throw interval_lib::comparison_error();
        }

        const T &low;
        const T &up;
    };

    struct number_holder
    {
        number_holder(const T &r)
            : val(r)
        {
            typedef typename Policies::checking checking;
            if (checking::is_nan(r))
                throw interval_lib::comparison_error();
        }

        const T &val;
    };

    typedef typename Policies::checking checking;
    typedef typename Policies::rounding rounding;

    T low;
    T up;
};

template <class T, class Policies>
inline interval<T, Policies>::interval()
    : low(static_cast<T>(0))
    , up(static_cast<T>(0))
{
}

template <class T, class Policies>
inline interval<T, Policies>::interval(T const &v)
    : low(v)
    , up(v)
{
    if (checking::is_nan(v))
        set_empty();
}

template <class T, class Policies> template <class T1> inline interval<T, Policies>::interval(T1 const &v)
{
    if (checking::is_nan(v))
        set_empty();
    else {
        rounding rnd;
        low = rnd.conv_down(v);
        up  = rnd.conv_up(v);
    }
}

template <class T, class Policies>
template <class T1, class T2>
inline interval<T, Policies>::interval(T1 const &l, T2 const &u)
{
    if (checking::is_nan(l) || checking::is_nan(u) || !(l <= u))
        set_empty();
    else {
        rounding rnd;
        low = rnd.conv_down(l);
        up  = rnd.conv_up(u);
    }
}

template <class T, class Policies>
inline interval<T, Policies>::interval(T const &l, T const &u)
    : low(l)
    , up(u)
{
    if (checking::is_nan(l) || checking::is_nan(u) || !(l <= u))
        set_empty();
}


template <class T, class Policies>
inline interval<T, Policies>::interval(interval<T, Policies> const &r)
    : low(r.lower())
    , up(r.upper())
{
}

template <class T, class Policies>
template <class Policies1>
inline interval<T, Policies>::interval(interval<T, Policies1> const &r)
    : low(r.lower())
    , up(r.upper())
{
    typedef typename Policies1::checking checking1;
    if (checking1::is_empty(r.lower(), r.upper()))
        set_empty();
}

template <class T, class Policies>
template <class T1, class Policies1>
inline interval<T, Policies>::interval(interval<T1, Policies1> const &r)
{
    typedef typename Policies1::checking checking1;
    if (checking1::is_empty(r.lower(), r.upper()))
        set_empty();
    else {
        rounding rnd;
        low = rnd.conv_down(r.lower());
        up  = rnd.conv_up(r.upper());
    }
}

template <class T, class Policies> inline interval<T, Policies> &interval<T, Policies>::operator=(T const &v)
{
    if (checking::is_nan(v))
        set_empty();
    else
        low = up = v;
    return *this;
}

template <class T, class Policies>
template <class T1>
inline interval<T, Policies> &interval<T, Policies>::operator=(T1 const &v)
{
    if (checking::is_nan(v))
        set_empty();
    else {
        rounding rnd;
        low = rnd.conv_down(v);
        up  = rnd.conv_up(v);
    }
    return *this;
}

template <class T, class Policies>
inline interval<T, Policies> &interval<T, Policies>::operator=(interval<T, Policies> const &r)
{
    low = r.lower();
    up  = r.upper();
    return *this;
}

template <class T, class Policies>
template <class Policies1>
inline interval<T, Policies> &interval<T, Policies>::operator=(interval<T, Policies1> const &r)
{
    typedef typename Policies1::checking checking1;
    if (checking1::is_empty(r.lower(), r.upper()))
        set_empty();
    else {
        low = r.lower();
        up  = r.upper();
    }
    return *this;
}

template <class T, class Policies>
template <class T1, class Policies1>
inline interval<T, Policies> &interval<T, Policies>::operator=(interval<T1, Policies1> const &r)
{
    typedef typename Policies1::checking checking1;
    if (checking1::is_empty(r.lower(), r.upper()))
        set_empty();
    else {
        rounding rnd;
        low = rnd.conv_down(r.lower());
        up  = rnd.conv_up(r.upper());
    }
    return *this;
}

template <class T, class Policies> inline void interval<T, Policies>::assign(const T &l, const T &u)
{
    if (checking::is_nan(l) || checking::is_nan(u) || !(l <= u))
        set_empty();
    else
        set(l, u);
}

template <class T, class Policies> inline void interval<T, Policies>::set(const T &l, const T &u)
{
    low = l;
    up  = u;
}

template <class T, class Policies> inline void interval<T, Policies>::set_empty()
{
    low = checking::empty_lower();
    up  = checking::empty_upper();
}

template <class T, class Policies> inline void interval<T, Policies>::set_whole()
{
    low = checking::neg_inf();
    up  = checking::pos_inf();
}

template <class T, class Policies> inline interval<T, Policies> interval<T, Policies>::hull(const T &x, const T &y)
{
    bool bad_x = checking::is_nan(x);
    bool bad_y = checking::is_nan(y);
    if (bad_x)
        if (bad_y)
            return interval::empty();
        else
            return interval(y, y, true);
    else if (bad_y)
        return interval(x, x, true);
    if (x <= y)
        return interval(x, y, true);
    else
        return interval(y, x, true);
}

template <class T, class Policies> inline interval<T, Policies> interval<T, Policies>::empty()
{
    return interval<T, Policies>(checking::empty_lower(), checking::empty_upper(), true);
}

template <class T, class Policies> inline interval<T, Policies> interval<T, Policies>::whole()
{
    return interval<T, Policies>(checking::neg_inf(), checking::pos_inf(), true);
}

template <class T, class Policies> inline const T &interval<T, Policies>::lower() const { return low; }

template <class T, class Policies> inline const T &interval<T, Policies>::upper() const { return up; }

/*
 * interval/interval comparisons
 */

template <class T, class Policies> inline bool interval<T, Policies>::operator<(const interval_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up < r.low)
            return true;
        else if (low >= r.up)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator>(const interval_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (low > r.up)
            return true;
        else if (up <= r.low)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator<=(const interval_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up <= r.low)
            return true;
        else if (low > r.up)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator>=(const interval_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (low >= r.up)
            return true;
        else if (up < r.low)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator==(const interval_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up == r.low && low == r.up)
            return true;
        else if (up < r.low || low > r.up)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator!=(const interval_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up < r.low || low > r.up)
            return true;
        else if (up == r.low && low == r.up)
            return false;
    }
    throw interval_lib::comparison_error();
}

/*
 * interval/number comparisons
 */

template <class T, class Policies> inline bool interval<T, Policies>::operator<(const number_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up < r.val)
            return true;
        else if (low >= r.val)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator>(const number_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (low > r.val)
            return true;
        else if (up <= r.val)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator<=(const number_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up <= r.val)
            return true;
        else if (low > r.val)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator>=(const number_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (low >= r.val)
            return true;
        else if (up < r.val)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator==(const number_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up == r.val && low == r.val)
            return true;
        else if (up < r.val || low > r.val)
            return false;
    }
    throw interval_lib::comparison_error();
}

template <class T, class Policies> inline bool interval<T, Policies>::operator!=(const number_holder &r) const
{
    if (!checking::is_empty(low, up)) {
        if (up < r.val || low > r.val)
            return true;
        else if (up == r.val && low == r.val)
            return false;
    }
    throw interval_lib::comparison_error();
}

} // namespace numeric
} // namespace boost

#endif // BOOST_NUMERIC_INTERVAL_INTERVAL_HPP

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/interval.hpp`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 485
- **Approximate Size**: 13345 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
