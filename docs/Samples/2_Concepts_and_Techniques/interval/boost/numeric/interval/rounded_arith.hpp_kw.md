# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp
---

**Total Keywords**: 25

---

## B

### BOOST_DN {#boostdn}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `#define BOOST_DN(EXPR)                \
    this->downward();                 \
    T r = this->forc`

### BOOST_NR {#boostnr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `#define BOOST_NR(EXPR)                \
    this->to_nearest();               \
    T r = this->forc`

### BOOST_NUMERIC_INTERVAL_ROUNDED_ARITH_HPP {#boostnumericintervalroundedarithhpp}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `#define BOOST_NUMERIC_INTERVAL_ROUNDED_ARITH_HPP

#include <boost/config/no_tr1/cmath.hpp>
#include `

### BOOST_UP {#boostup}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `#define BOOST_UP(EXPR)     return this->force_rounding(EXPR)
#define BOOST_UP_NEG(EXPR) return -this`

### BOOST_UP_NEG {#boostupneg}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `#define BOOST_UP_NEG(EXPR) return -this->force_rounding(EXPR)
    template <class U> T conv_down(U c`


## R

### Rounding {#rounding}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `class Rounding`


## A

### add_down {#adddown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    add_down(const T &x, const T &y) {`

### add_up {#addup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    add_up(const T &x, const T &y) {`


## C

### conv_down {#convdown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T conv_down(U const &v) {`

### conv_up {#convup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T conv_up(U const &v) {`


## D

### div_down {#divdown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    div_down(const T &x, const T &y) {`

### div_up {#divup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    div_up(const T &x, const T &y) {`


## I

### init {#init}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `void init() {`

### int_down {#intdown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T int_down(const T &x) {`

### int_up {#intup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T int_up(const T &x) {`


## M

### median {#median}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    median(const T &x, const T &y) {`

### mul_down {#muldown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    mul_down(const T &x, const T &y) {`

### mul_up {#mulup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    mul_up(const T &x, const T &y) {`


## R

### rounded_arith_exact {#roundedarithexact}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `struct rounded_arith_exact`

### rounded_arith_opp {#roundedarithopp}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `struct rounded_arith_opp`

### rounded_arith_std {#roundedarithstd}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `struct rounded_arith_std`


## S

### sqrt_down {#sqrtdown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    sqrt_down(const T &x)
    {`

### sqrt_up {#sqrtup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T sqrt_up(const T &x)
    {`

### sub_down {#subdown}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    sub_down(const T &x, const T &y) {`

### sub_up {#subup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/numeric/interval/rounded_arith.hpp](./rounded_arith.hpp_docs.md)
- **Context**: `T                    sub_up(const T &x, const T &y) {`

