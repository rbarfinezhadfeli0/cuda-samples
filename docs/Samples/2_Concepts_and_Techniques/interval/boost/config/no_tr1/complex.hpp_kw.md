# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/complex.hpp
---

**Total Keywords**: 3

---

## B

### BOOST_CONFIG_COMPLEX {#boostconfigcomplex}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/complex.hpp](./complex.hpp_docs.md)
- **Context**: `#define BOOST_CONFIG_COMPLEX

#ifndef BOOST_TR1_NO_RECURSION
#define BOOST_TR1_NO_RECURSION
#define `

### BOOST_CONFIG_NO_COMPLEX_RECURSION {#boostconfignocomplexrecursion}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/complex.hpp](./complex.hpp_docs.md)
- **Context**: `#define BOOST_CONFIG_NO_COMPLEX_RECURSION
#endif

#include <complex>

#ifdef BOOST_CONFIG_NO_COMPLEX`

### BOOST_TR1_NO_RECURSION {#boosttr1norecursion}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/no_tr1/complex.hpp](./complex.hpp_docs.md)
- **Context**: `#define BOOST_TR1_NO_RECURSION
#define BOOST_CONFIG_NO_COMPLEX_RECURSION
#endif

#include <complex>
`

