# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp
---

**Total Keywords**: 78

---

## B

### BOOST_ABI_PREFIX {#boostabiprefix}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_ABI_PREFIX "boost/config/abi/borland_prefix.hpp"
#endif
#ifndef BOOST_ABI_SUFFIX
#defi`

### BOOST_ABI_SUFFIX {#boostabisuffix}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_ABI_SUFFIX "boost/config/abi/borland_suffix.hpp"
#endif
#endif
//
// Disable Win32 sup`

### BOOST_BCB_PARTIAL_SPECIALIZATION_BUG {#boostbcbpartialspecializationbug}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_BCB_PARTIAL_SPECIALIZATION_BUG
#define BOOST_NO_TEMPLATE_TEMPLATES

#define BOOST_NO_P`

### BOOST_BCB_WITH_DINKUMWARE {#boostbcbwithdinkumware}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_BCB_WITH_DINKUMWARE
#endif

//
// Version 5.0 and below:
#if __BORLANDC__ <= 0x0550
//`

### BOOST_BCB_WITH_ROGUE_WAVE {#boostbcbwithroguewave}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_BCB_WITH_ROGUE_WAVE
#elif __BORLANDC__ < 0x570
#define BOOST_BCB_WITH_STLPORT
#else
#d`

### BOOST_BCB_WITH_STLPORT {#boostbcbwithstlport}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_BCB_WITH_STLPORT
#else
#define BOOST_BCB_WITH_DINKUMWARE
#endif

//
// Version 5.0 and`

### BOOST_COMPILER {#boostcompiler}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_COMPILER "Borland C++ version " BOOST_STRINGIZE(__BORLANDC__)
`

### BOOST_DISABLE_WIN32 {#boostdisablewin32}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_DISABLE_WIN32
#endif
//
// MSVC compatibility mode does some nasty things:
// TODO: lo`

### BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL {#boostfunctionscopeusingdeclarationbreaksadl}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL
#define BOOST_NO_DEPENDENT_NESTED_DERIVATI`

### BOOST_HAS_ALIGNOF {#boosthasalignof}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_ALIGNOF
#define BOOST_HAS_CHAR16_T
#define BOOST_HAS_CHAR32_T
#define BOOST_HAS_DE`

### BOOST_HAS_CHAR16_T {#boosthaschar16t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_CHAR16_T
#define BOOST_HAS_CHAR32_T
#define BOOST_HAS_DECLTYPE
#define BOOST_HAS_E`

### BOOST_HAS_CHAR32_T {#boosthaschar32t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_CHAR32_T
#define BOOST_HAS_DECLTYPE
#define BOOST_HAS_EXPLICIT_CONVERSION_OPS
#def`

### BOOST_HAS_DECLSPEC {#boosthasdeclspec}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_DECLSPEC
#endif
//
// ABI fixing headers:
//
#if __BORLANDC__ != 0x600 // not impl`

### BOOST_HAS_DECLTYPE {#boosthasdecltype}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_DECLTYPE
#define BOOST_HAS_EXPLICIT_CONVERSION_OPS
#define BOOST_HAS_REF_QUALIFIER`

### BOOST_HAS_DIRENT_H {#boosthasdirenth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_DIRENT_H
#endif
//
// all versions support __declspec:
//
#ifndef __STRICT_ANSI__
`

### BOOST_HAS_EXPLICIT_CONVERSION_OPS {#boosthasexplicitconversionops}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_EXPLICIT_CONVERSION_OPS
#define BOOST_HAS_REF_QUALIFIER
#define BOOST_HAS_RVALUE_R`

### BOOST_HAS_LONG_LONG {#boosthaslonglong}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_LONG_LONG
#else
#define BOOST_NO_LONG_LONG
#endif
// On non-Win32 platforms let th`

### BOOST_HAS_MACRO_USE_FACET {#boosthasmacrousefacet}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_MACRO_USE_FACET
#endif

//
// Post 0x561 we have long long and stdint.h:
#if __BOR`

### BOOST_HAS_MS_INT64 {#boosthasmsint64}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_MS_INT64
#endif
//
// check for exception handling support:
//
#if !defined(_CPPUN`

### BOOST_HAS_REF_QUALIFIER {#boosthasrefqualifier}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_REF_QUALIFIER
#define BOOST_HAS_RVALUE_REFS
#define BOOST_HAS_STATIC_ASSERT
#endif`

### BOOST_HAS_RVALUE_REFS {#boosthasrvaluerefs}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_RVALUE_REFS
#define BOOST_HAS_STATIC_ASSERT
#endif

#define BOOST_NO_AUTO_DECLARAT`

### BOOST_HAS_STATIC_ASSERT {#boosthasstaticassert}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_STATIC_ASSERT
#endif

#define BOOST_NO_AUTO_DECLARATIONS
#define BOOST_NO_AUTO_MUL`

### BOOST_HAS_STDINT_H {#boosthasstdinth}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_STDINT_H
#endif
#endif

// Borland C++Builder 6 defaults to using STLPort.  If _US`

### BOOST_HAS_TR1_HASH {#boosthastr1hash}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_HAS_TR1_HASH

#define BOOST_HAS_MACRO_USE_FACET
#endif

//
// Post 0x561 we have long `

### BOOST_ILLEGAL_CV_REFERENCES {#boostillegalcvreferences}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_ILLEGAL_CV_REFERENCES
#endif

//
//  Positive Feature detection
//
// Borland C++ Buil`

### BOOST_MPL_CFG_NO_PREPROCESSED_HEADERS {#boostmplcfgnopreprocessedheaders}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_MPL_CFG_NO_PREPROCESSED_HEADERS
#endif

// Borland C++ Builder 2008 and below:
#define`

### BOOST_NO_ARGUMENT_DEPENDENT_LOOKUP {#boostnoargumentdependentlookup}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_ARGUMENT_DEPENDENT_LOOKUP
#define BOOST_NO_VOID_RETURNS
#endif

#define BOOST_COMPI`

### BOOST_NO_AUTO_DECLARATIONS {#boostnoautodeclarations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_AUTO_DECLARATIONS
#define BOOST_NO_AUTO_MULTIDECLARATIONS
#define BOOST_NO_CONCEPTS`

### BOOST_NO_AUTO_MULTIDECLARATIONS {#boostnoautomultideclarations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_AUTO_MULTIDECLARATIONS
#define BOOST_NO_CONCEPTS
#define BOOST_NO_CONSTEXPR
#define`

### BOOST_NO_CHAR16_T {#boostnochar16t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_CHAR16_T
#define BOOST_NO_CHAR32_T
#define BOOST_NO_DECLTYPE
#define BOOST_NO_EXPLI`

### BOOST_NO_CHAR32_T {#boostnochar32t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_CHAR32_T
#define BOOST_NO_DECLTYPE
#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS
#`

### BOOST_NO_CONCEPTS {#boostnoconcepts}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_CONCEPTS
#define BOOST_NO_CONSTEXPR
#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BO`

### BOOST_NO_CONSTEXPR {#boostnoconstexpr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_CONSTEXPR
#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BOOST_NO_DELETED_FUNCTIONS
#`

### BOOST_NO_CV_SPECIALIZATIONS {#boostnocvspecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_CV_SPECIALIZATIONS
#define BOOST_NO_CV_VOID_SPECIALIZATIONS
#define BOOST_NO_DEDUCE`

### BOOST_NO_CV_VOID_SPECIALIZATIONS {#boostnocvvoidspecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_CV_VOID_SPECIALIZATIONS
#define BOOST_NO_DEDUCED_TYPENAME
// workaround for missing`

### BOOST_NO_DECLTYPE {#boostnodecltype}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_DECLTYPE
#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS
#define BOOST_NO_EXTERN_TEM`

### BOOST_NO_DEDUCED_TYPENAME {#boostnodeducedtypename}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_DEDUCED_TYPENAME
// workaround for missing WCHAR_MAX/WCHAR_MIN:
#include <climits>
`

### BOOST_NO_DEFAULTED_FUNCTIONS {#boostnodefaultedfunctions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BOOST_NO_DELETED_FUNCTIONS
#define BOOST_NO_FUNCTION_TE`

### BOOST_NO_DELETED_FUNCTIONS {#boostnodeletedfunctions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_DELETED_FUNCTIONS
#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS
#define BOOST_NO_`

### BOOST_NO_DEPENDENT_NESTED_DERIVATIONS {#boostnodependentnestedderivations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_DEPENDENT_NESTED_DERIVATIONS
#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#define BOOST`

### BOOST_NO_EXCEPTIONS {#boostnoexceptions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXCEPTIONS
#endif
//
// all versions have a <dirent.h>:
//
#ifndef __STRICT_ANSI__
`

### BOOST_NO_EXPLICIT_CONVERSION_OPERATORS {#boostnoexplicitconversionoperators}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS
#define BOOST_NO_EXTERN_TEMPLATE
#define BOOST_NO_RVA`

### BOOST_NO_EXTERN_TEMPLATE {#boostnoexterntemplate}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXTERN_TEMPLATE
#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_NO_SCOPED_ENUMS
#d`

### BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS {#boostnofunctiontemplatedefaultargs}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS
#define BOOST_NO_INITIALIZER_LISTS
#define BOOST_NO_`

### BOOST_NO_FUNCTION_TYPE_SPECIALIZATIONS {#boostnofunctiontypespecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_FUNCTION_TYPE_SPECIALIZATIONS
#define BOOST_NO_USING_TEMPLATE
#define BOOST_SP_NO_S`

### BOOST_NO_INITIALIZER_LISTS {#boostnoinitializerlists}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_INITIALIZER_LISTS
#define BOOST_NO_LAMBDAS
#define BOOST_NO_NULLPTR
#define BOOST_N`

### BOOST_NO_INTEGRAL_INT64_T {#boostnointegralint64t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_INTEGRAL_INT64_T
#define BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL
#define `

### BOOST_NO_IS_ABSTRACT {#boostnoisabstract}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_IS_ABSTRACT
#define BOOST_NO_FUNCTION_TYPE_SPECIALIZATIONS
#define BOOST_NO_USING_T`

### BOOST_NO_LAMBDAS {#boostnolambdas}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_LAMBDAS
#define BOOST_NO_NULLPTR
#define BOOST_NO_RAW_LITERALS
#define BOOST_NO_RVA`

### BOOST_NO_LIMITS_COMPILE_TIME_CONSTANTS {#boostnolimitscompiletimeconstants}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_LIMITS_COMPILE_TIME_CONSTANTS
#define BOOST_NO_IS_ABSTRACT
#define BOOST_NO_FUNCTIO`

### BOOST_NO_LONG_LONG {#boostnolonglong}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_LONG_LONG
#endif
// On non-Win32 platforms let the platform config figure this out:`

### BOOST_NO_MEMBER_FUNCTION_SPECIALIZATIONS {#boostnomemberfunctionspecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_MEMBER_FUNCTION_SPECIALIZATIONS
#endif

// Borland C++ Builder 2006 Update 2 and be`

### BOOST_NO_MEMBER_TEMPLATE_FRIENDS {#boostnomembertemplatefriends}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#define BOOST_NO_USI`

### BOOST_NO_NESTED_FRIENDSHIP {#boostnonestedfriendship}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_NESTED_FRIENDSHIP
#define BOOST_NO_TYPENAME_WITH_CTOR
#if (__BORLANDC__ < 0x600)
#d`

### BOOST_NO_NULLPTR {#boostnonullptr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_NULLPTR
#define BOOST_NO_RAW_LITERALS
#define BOOST_NO_RVALUE_REFERENCES
#define BO`

### BOOST_NO_OPERATORS_IN_NAMESPACE {#boostnooperatorsinnamespace}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_OPERATORS_IN_NAMESPACE
#endif
#endif

// Version 5.51 and below:
#if (__BORLANDC__ `

### BOOST_NO_PRIVATE_IN_AGGREGATE {#boostnoprivateinaggregate}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_PRIVATE_IN_AGGREGATE

#ifdef _WIN32
#define BOOST_NO_SWPRINTF
#elif defined(linux) `

### BOOST_NO_RAW_LITERALS {#boostnorawliterals}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_RAW_LITERALS
#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_NO_SCOPED_ENUMS
#defi`

### BOOST_NO_RVALUE_REFERENCES {#boostnorvaluereferences}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_NO_SCOPED_ENUMS
#define BOOST_NO_SFINAE_EXPR
#defin`

### BOOST_NO_SCOPED_ENUMS {#boostnoscopedenums}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_SCOPED_ENUMS
#define BOOST_NO_SFINAE_EXPR
#define BOOST_NO_TEMPLATE_ALIASES
#define`

### BOOST_NO_SFINAE {#boostnosfinae}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_SFINAE
#define BOOST_BCB_PARTIAL_SPECIALIZATION_BUG
#define BOOST_NO_TEMPLATE_TEMPL`

### BOOST_NO_SFINAE_EXPR {#boostnosfinaeexpr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_SFINAE_EXPR
#define BOOST_NO_TEMPLATE_ALIASES
#define BOOST_NO_UNICODE_LITERALS // `

### BOOST_NO_STATIC_ASSERT {#boostnostaticassert}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_STATIC_ASSERT
#else
#define BOOST_HAS_ALIGNOF
#define BOOST_HAS_CHAR16_T
#define BO`

### BOOST_NO_STDC_NAMESPACE {#boostnostdcnamespace}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_STDC_NAMESPACE
// _CPPUNWIND doesn't get automatically set for some reason:
#pragma`

### BOOST_NO_SWPRINTF {#boostnoswprintf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_SWPRINTF
#elif defined(linux) || defined(__linux__) || defined(__linux)
// we shoul`

### BOOST_NO_TEMPLATE_ALIASES {#boostnotemplatealiases}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_TEMPLATE_ALIASES
#define BOOST_NO_UNICODE_LITERALS // UTF-8 still not supported
#de`

### BOOST_NO_TEMPLATE_TEMPLATES {#boostnotemplatetemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_TEMPLATE_TEMPLATES

#define BOOST_NO_PRIVATE_IN_AGGREGATE

#ifdef _WIN32
#define BO`

### BOOST_NO_TWO_PHASE_NAME_LOOKUP {#boostnotwophasenamelookup}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#define BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BA`

### BOOST_NO_TYPENAME_WITH_CTOR {#boostnotypenamewithctor}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_TYPENAME_WITH_CTOR
#if (__BORLANDC__ < 0x600)
#define BOOST_ILLEGAL_CV_REFERENCES
#`

### BOOST_NO_UNICODE_LITERALS {#boostnounicodeliterals}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_UNICODE_LITERALS // UTF-8 still not supported
#define BOOST_NO_VARIADIC_TEMPLATES

`

### BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE {#boostnousingdeclarationoverloadsfromtypenamebase}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE
#define BOOST_NO_NESTED_FRIENDSHIP
#`

### BOOST_NO_USING_TEMPLATE {#boostnousingtemplate}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_USING_TEMPLATE
#define BOOST_SP_NO_SP_CONVERTIBLE

// Temporary workaround
#define `

### BOOST_NO_VARIADIC_TEMPLATES {#boostnovariadictemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_VARIADIC_TEMPLATES

#if __BORLANDC__ >= 0x590
#define BOOST_HAS_TR1_HASH

#define B`

### BOOST_NO_VOID_RETURNS {#boostnovoidreturns}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_NO_VOID_RETURNS
#endif

#define BOOST_COMPILER "Borland C++ version " BOOST_STRINGIZE(`

### BOOST_SP_NO_SP_CONVERTIBLE {#boostspnospconvertible}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define BOOST_SP_NO_SP_CONVERTIBLE

// Temporary workaround
#define BOOST_MPL_CFG_NO_PREPROCESSED_HE`


## W

### WCHAR_MAX {#wcharmax}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define WCHAR_MAX 0xffff
#endif
#ifndef WCHAR_MIN
#define WCHAR_MIN 0
#endif
#endif

// Borland C++ `

### WCHAR_MIN {#wcharmin}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define WCHAR_MIN 0
#endif
#endif

// Borland C++ Builder 6 and below:
#if (__BORLANDC__ <= 0x564)

`


## E

### errno {#errno}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/borland.hpp](./borland.hpp_docs.md)
- **Context**: `#define errno errno
#endif

#endif

//
// new bug in 5.61:
#if (__BORLANDC__ >= 0x561) && (__BORLAND`

