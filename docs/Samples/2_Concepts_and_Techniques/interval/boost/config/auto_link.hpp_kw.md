# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp
---

**Total Keywords**: 8

---

## B

### BOOST_DO_STRINGIZE {#boostdostringize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_DO_STRINGIZE(X) #X
#endif
//
// Only include what follows for known and supported comp`

### BOOST_LIB_PREFIX {#boostlibprefix}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_LIB_PREFIX "lib"
#endif

//
// now include the lib:
//
#if defined(BOOST_LIB_NAME) && `

### BOOST_LIB_RT_OPT {#boostlibrtopt}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_LIB_RT_OPT "-s"
#endif

#endif

#endif

//
// select linkage opt:
//
#if (defined(_DLL`

### BOOST_LIB_THREAD_OPT {#boostlibthreadopt}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_LIB_THREAD_OPT
#endif

#if defined(_MSC_VER) || defined(__MWERKS__)

#ifdef _DLL

#if `

### BOOST_LIB_TOOLSET {#boostlibtoolset}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_LIB_TOOLSET "cw9"

#endif
#endif // BOOST_LIB_TOOLSET

//
// select thread opt:
//
#if`

### BOOST_MSVC {#boostmsvc}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_MSVC            _MSC_VER
#define BOOST_STRINGIZE(X)    BOOST_DO_STRINGIZE(X)
#define B`

### BOOST_STRINGIZE {#booststringize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `#define BOOST_STRINGIZE(X)    BOOST_DO_STRINGIZE(X)
#define BOOST_DO_STRINGIZE(X) #X
#endif
//
// On`


## C

### CodeWarrior {#codewarrior}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/auto_link.hpp](./auto_link.hpp_docs.md)
- **Context**: `1FF)

// Metrowerks CodeWarrior 8.x
#define BOOST_L`

