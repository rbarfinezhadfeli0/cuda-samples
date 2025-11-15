# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp
---

**Total Keywords**: 20

---

## B

### BOOST_COMPILER {#boostcompiler}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_COMPILER "Intel C++ version " BOOST_STRINGIZE(BOOST_INTEL_CXX_VERSION)
#define BOOST_I`

### BOOST_DISABLE_WIN32 {#boostdisablewin32}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_DISABLE_WIN32
#endif

// I checked version 6.0 build 020312Z, it implements the NRVO.
`

### BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL {#boostfunctionscopeusingdeclarationbreaksadl}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL
#endif
#endif
#if (defined(__GNUC__) && (_`

### BOOST_HAS_MS_INT64 {#boosthasmsint64}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_HAS_MS_INT64
#endif
#define BOOST_NO_SWPRINTF
#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#`

### BOOST_HAS_NRVO {#boosthasnrvo}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NRVO
#endif

//
// versions check:
// we don't support Intel prior to version 5.0:`

### BOOST_INTEL {#boostintel}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_INTEL    BOOST_INTEL_CXX_VERSION

#if defined(_WIN32) || defined(_WIN64)
#define BOOST`

### BOOST_INTEL_CXX_VERSION {#boostintelcxxversion}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_INTEL_CXX_VERSION __ECC
#endif

#define BOOST_COMPILER "Intel C++ version " BOOST_STRI`

### BOOST_INTEL_LINUX {#boostintellinux}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_INTEL_LINUX BOOST_INTEL
#endif

#if (BOOST_INTEL_CXX_VERSION <= 500) && defined(_MSC_V`

### BOOST_INTEL_WIN {#boostintelwin}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_INTEL_WIN BOOST_INTEL
#else
#define BOOST_INTEL_LINUX BOOST_INTEL
#endif

#if (BOOST_I`

### BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS {#boostnoexplicitfunctiontemplatearguments}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS
#define BOOST_NO_TEMPLATE_TEMPLATES
#endif

#i`

### BOOST_NO_INTEGRAL_INT64_T {#boostnointegralint64t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_INTEGRAL_INT64_T
#endif

#endif

#if (BOOST_INTEL_CXX_VERSION <= 710) && defined(_W`

### BOOST_NO_INTRINSIC_WCHAR_T {#boostnointrinsicwchart}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_INTRINSIC_WCHAR_T
#endif
#endif

#if defined(__GNUC__) && !defined(BOOST_FUNCTION_S`

### BOOST_NO_POINTER_TO_MEMBER_TEMPLATE_PARAMETERS {#boostnopointertomembertemplateparameters}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_POINTER_TO_MEMBER_TEMPLATE_PARAMETERS
#endif

// See http://aspn.activestate.com/AS`

### BOOST_NO_SWPRINTF {#boostnoswprintf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_SWPRINTF
#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#elif defined(_WIN32)
#define BOOST`

### BOOST_NO_TEMPLATE_TEMPLATES {#boostnotemplatetemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_TEMPLATE_TEMPLATES
#endif

#if (BOOST_INTEL_CXX_VERSION <= 600)

#if defined(_MSC_V`

### BOOST_NO_TWO_PHASE_NAME_LOOKUP {#boostnotwophasenamelookup}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#endif

//
// last known and checked version:
#if (BOOST_INTE`

### BOOST_NO_VOID_RETURNS {#boostnovoidreturns}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#define BOOST_NO_VOID_RETURNS
#define BOOST_NO_INTEGRAL_INT64_T
#endif

#endif

#if (BOOST_INTEL_CXX`


## M

### MacOS {#macos}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `#endif

// Intel on MacOS requires
#if define`


## A

### assert_intrinsic_wchar_t {#assertintrinsicwchart}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `struct assert_intrinsic_wchar_t`

### assert_no_intrinsic_wchar_t {#assertnointrinsicwchart}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/intel.hpp](./intel.hpp_docs.md)
- **Context**: `struct assert_no_intrinsic_wchar_t`

