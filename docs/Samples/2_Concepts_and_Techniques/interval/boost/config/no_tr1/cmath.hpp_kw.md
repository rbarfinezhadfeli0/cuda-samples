# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/cmath.hpp
---

**Total Keywords**: 3

---

## B

### BOOST_CONFIG_CMATH {#boostconfigcmath}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/cmath.hpp](./cmath.hpp_docs.md)
- **Context**: `#define BOOST_CONFIG_CMATH

#ifndef BOOST_TR1_NO_RECURSION
#define BOOST_TR1_NO_RECURSION
#define BO`

### BOOST_CONFIG_NO_CMATH_RECURSION {#boostconfignocmathrecursion}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/cmath.hpp](./cmath.hpp_docs.md)
- **Context**: `#define BOOST_CONFIG_NO_CMATH_RECURSION
#endif

#include <cmath>

#ifdef BOOST_CONFIG_NO_CMATH_RECUR`

### BOOST_TR1_NO_RECURSION {#boosttr1norecursion}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/cmath.hpp](./cmath.hpp_docs.md)
- **Context**: `#define BOOST_TR1_NO_RECURSION
#define BOOST_CONFIG_NO_CMATH_RECURSION
#endif

#include <cmath>

#if`

