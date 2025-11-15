# Keywords: Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp
---

**Total Keywords**: 67

---

## B

### BOOST_ABI_PREFIX {#boostabiprefix}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_ABI_PREFIX "boost/config/abi/msvc_prefix.hpp"
#endif
#ifndef BOOST_ABI_SUFFIX
#define `

### BOOST_ABI_SUFFIX {#boostabisuffix}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_ABI_SUFFIX "boost/config/abi/msvc_suffix.hpp"
#endif

// TODO:
// these things are mos`

### BOOST_COMPILER {#boostcompiler}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_COMPILER "Microsoft Visual C++ version " BOOST_STRINGIZE(BOOST_COMPILER_VERSION)

//
/`

### BOOST_COMPILER_VERSION {#boostcompilerversion}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_COMPILER_VERSION _MSC_VER
#endif
#endif

#define BOOST_COMPILER "Microsoft Visual C++ `

### BOOST_DISABLE_WIN32 {#boostdisablewin32}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_DISABLE_WIN32
#endif
#if !defined(_CPPRTTI) && !defined(BOOST_NO_RTTI)
#define BOOST_N`

### BOOST_HAS_DECLSPEC {#boosthasdeclspec}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_HAS_DECLSPEC

//
// C++0x features
//
//   See above for BOOST_NO_LONG_LONG

// C++ fe`

### BOOST_HAS_LONG_LONG {#boosthaslonglong}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_HAS_LONG_LONG
#else
#define BOOST_NO_LONG_LONG
#endif
#if (_MSC_VER >= 1400) && !defin`

### BOOST_HAS_MS_INT64 {#boosthasmsint64}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_HAS_MS_INT64
#endif
#if (_MSC_VER >= 1310) && (defined(_MSC_EXTENSIONS) || (_MSC_VER >`

### BOOST_HAS_NRVO {#boosthasnrvo}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_HAS_NRVO
#endif
//
// disable Win32 API's if compiler extentions are
// turned off:
//`

### BOOST_MSVC {#boostmsvc}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_MSVC _MSC_VER

#if _MSC_FULL_VER > 100000000
#define BOOST_MSVC_FULL_VER _MSC_FULL_VER`

### BOOST_MSVC6_MEMBER_TEMPLATES {#boostmsvc6membertemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_MSVC6_MEMBER_TEMPLATES

#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#define BOOST_NO_TEMP`

### BOOST_MSVC_FULL_VER {#boostmsvcfullver}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_MSVC_FULL_VER (_MSC_FULL_VER * 10)
#endif

// turn off the warnings before we #include`

### BOOST_NO_ADL_BARRIER {#boostnoadlbarrier}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_ADL_BARRIER
#endif

#if _MSC_VER <= 1500 || !defined(BOOST_STRICT_CONFIG) // 1500 =`

### BOOST_NO_ARGUMENT_DEPENDENT_LOOKUP {#boostnoargumentdependentlookup}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_ARGUMENT_DEPENDENT_LOOKUP
#define BOOST_NO_INTEGRAL_INT64_T
#define BOOST_NO_DEDUCE`

### BOOST_NO_AUTO_DECLARATIONS {#boostnoautodeclarations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_AUTO_DECLARATIONS
#define BOOST_NO_AUTO_MULTIDECLARATIONS
#define BOOST_NO_DECLTYPE`

### BOOST_NO_AUTO_MULTIDECLARATIONS {#boostnoautomultideclarations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_AUTO_MULTIDECLARATIONS
#define BOOST_NO_DECLTYPE
#define BOOST_NO_LAMBDAS
#define B`

### BOOST_NO_CHAR16_T {#boostnochar16t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_CHAR16_T
#define BOOST_NO_CHAR32_T
#define BOOST_NO_CONCEPTS
#define BOOST_NO_CONST`

### BOOST_NO_CHAR32_T {#boostnochar32t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_CHAR32_T
#define BOOST_NO_CONCEPTS
#define BOOST_NO_CONSTEXPR
#define BOOST_NO_DEFA`

### BOOST_NO_CONCEPTS {#boostnoconcepts}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_CONCEPTS
#define BOOST_NO_CONSTEXPR
#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BO`

### BOOST_NO_CONSTEXPR {#boostnoconstexpr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_CONSTEXPR
#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BOOST_NO_DELETED_FUNCTIONS
#`

### BOOST_NO_CV_VOID_SPECIALIZATIONS {#boostnocvvoidspecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_CV_VOID_SPECIALIZATIONS
#define BOOST_NO_FUNCTION_TEMPLATE_ORDERING
#define BOOST_N`

### BOOST_NO_DECLTYPE {#boostnodecltype}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_DECLTYPE
#define BOOST_NO_LAMBDAS
#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_`

### BOOST_NO_DEDUCED_TYPENAME {#boostnodeducedtypename}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_DEDUCED_TYPENAME
#define BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE

/`

### BOOST_NO_DEFAULTED_FUNCTIONS {#boostnodefaultedfunctions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BOOST_NO_DELETED_FUNCTIONS
#define BOOST_NO_EXPLICIT_CO`

### BOOST_NO_DELETED_FUNCTIONS {#boostnodeletedfunctions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_DELETED_FUNCTIONS
#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS
#define BOOST_NO_E`

### BOOST_NO_DEPENDENT_TYPES_IN_TEMPLATE_VALUE_PARAMETERS {#boostnodependenttypesintemplatevalueparameters}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_DEPENDENT_TYPES_IN_TEMPLATE_VALUE_PARAMETERS
#endif

#define BOOST_NO_EXPLICIT_FUNC`

### BOOST_NO_EXCEPTIONS {#boostnoexceptions}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXCEPTIONS
#endif

//
// __int64 support:
//
#if (_MSC_VER >= 1200)
#define BOOST_H`

### BOOST_NO_EXCEPTION_STD_NAMESPACE {#boostnoexceptionstdnamespace}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXCEPTION_STD_NAMESPACE

#if BOOST_MSVC == 1202
#define BOOST_NO_STD_TYPEINFO
#endi`

### BOOST_NO_EXPLICIT_CONVERSION_OPERATORS {#boostnoexplicitconversionoperators}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS
#define BOOST_NO_EXTERN_TEMPLATE
#define BOOST_NO_FUN`

### BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS {#boostnoexplicitfunctiontemplatearguments}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS
#define BOOST_NO_INCLASS_MEMBER_INITIALIZATION`

### BOOST_NO_EXTERN_TEMPLATE {#boostnoexterntemplate}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_EXTERN_TEMPLATE
#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS
#define BOOST_NO_IN`

### BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS {#boostnofunctiontemplatedefaultargs}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS
#define BOOST_NO_INITIALIZER_LISTS
#define BOOST_NO_`

### BOOST_NO_FUNCTION_TEMPLATE_ORDERING {#boostnofunctiontemplateordering}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_FUNCTION_TEMPLATE_ORDERING
#define BOOST_NO_USING_TEMPLATE
#define BOOST_NO_SWPRINT`

### BOOST_NO_FUNCTION_TYPE_SPECIALIZATIONS {#boostnofunctiontypespecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_FUNCTION_TYPE_SPECIALIZATIONS
// TODO: what version is meant here? Have there reall`

### BOOST_NO_GETSYSTEMTIMEASFILETIME {#boostnogetsystemtimeasfiletime}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_GETSYSTEMTIMEASFILETIME
#define BOOST_NO_SWPRINTF
#endif

//
// check for exception`

### BOOST_NO_INCLASS_MEMBER_INITIALIZATION {#boostnoinclassmemberinitialization}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_INCLASS_MEMBER_INITIALIZATION
#define BOOST_NO_PRIVATE_IN_AGGREGATE
#define BOOST_N`

### BOOST_NO_INITIALIZER_LISTS {#boostnoinitializerlists}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_INITIALIZER_LISTS
#define BOOST_NO_NULLPTR
#define BOOST_NO_RAW_LITERALS
#define BO`

### BOOST_NO_INTEGRAL_INT64_T {#boostnointegralint64t}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_INTEGRAL_INT64_T
#define BOOST_NO_DEDUCED_TYPENAME
#define BOOST_NO_USING_DECLARATI`

### BOOST_NO_INTRINSIC_WCHAR_T {#boostnointrinsicwchart}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_INTRINSIC_WCHAR_T
#endif

#if defined(_WIN32_WCE) || defined(UNDER_CE)
#define BOOS`

### BOOST_NO_IS_ABSTRACT {#boostnoisabstract}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_IS_ABSTRACT
#define BOOST_NO_FUNCTION_TYPE_SPECIALIZATIONS
// TODO: what version is`

### BOOST_NO_LAMBDAS {#boostnolambdas}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_LAMBDAS
#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_NO_STATIC_ASSERT
#endif //`

### BOOST_NO_LONG_LONG {#boostnolonglong}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_LONG_LONG
#endif
#if (_MSC_VER >= 1400) && !defined(_DEBUG)
#define BOOST_HAS_NRVO
`

### BOOST_NO_MEMBER_FUNCTION_SPECIALIZATIONS {#boostnomemberfunctionspecializations}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_MEMBER_FUNCTION_SPECIALIZATIONS
#endif

#endif

#if _MSC_VER < 1400
// although a c`

### BOOST_NO_MEMBER_TEMPLATES {#boostnomembertemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_MEMBER_TEMPLATES
//    For VC++ experts wishing to attempt workarounds, we define:
`

### BOOST_NO_MEMBER_TEMPLATE_FRIENDS {#boostnomembertemplatefriends}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#endif

#if _MSC_VER <= 1600 // 1600 == VC++ 10.0
#define B`

### BOOST_NO_NULLPTR {#boostnonullptr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_NULLPTR
#define BOOST_NO_RAW_LITERALS
#define BOOST_NO_SCOPED_ENUMS
#define BOOST_N`

### BOOST_NO_POINTER_TO_MEMBER_TEMPLATE_PARAMETERS {#boostnopointertomembertemplateparameters}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_POINTER_TO_MEMBER_TEMPLATE_PARAMETERS
#define BOOST_NO_IS_ABSTRACT
#define BOOST_NO`

### BOOST_NO_PRIVATE_IN_AGGREGATE {#boostnoprivateinaggregate}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_PRIVATE_IN_AGGREGATE
#define BOOST_NO_ARGUMENT_DEPENDENT_LOOKUP
#define BOOST_NO_IN`

### BOOST_NO_RAW_LITERALS {#boostnorawliterals}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_RAW_LITERALS
#define BOOST_NO_SCOPED_ENUMS
#define BOOST_NO_SFINAE_EXPR
#define BOO`

### BOOST_NO_RTTI {#boostnortti}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_RTTI
#endif

//
// all versions support __declspec:
//
#define BOOST_HAS_DECLSPEC

`

### BOOST_NO_RVALUE_REFERENCES {#boostnorvaluereferences}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_NO_STATIC_ASSERT
#endif // _MSC_VER < 1600

// C++0`

### BOOST_NO_SCOPED_ENUMS {#boostnoscopedenums}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_SCOPED_ENUMS
#define BOOST_NO_SFINAE_EXPR
#define BOOST_NO_TEMPLATE_ALIASES
#define`

### BOOST_NO_SFINAE {#boostnosfinae}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_SFINAE
#define BOOST_NO_POINTER_TO_MEMBER_TEMPLATE_PARAMETERS
#define BOOST_NO_IS_A`

### BOOST_NO_SFINAE_EXPR {#boostnosfinaeexpr}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_SFINAE_EXPR
#define BOOST_NO_TEMPLATE_ALIASES
#define BOOST_NO_UNICODE_LITERALS
#de`

### BOOST_NO_STATIC_ASSERT {#boostnostaticassert}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_STATIC_ASSERT
#endif // _MSC_VER < 1600

// C++0x features not supported by any ver`

### BOOST_NO_STD_TYPEINFO {#boostnostdtypeinfo}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_STD_TYPEINFO
#endif

// disable min/max macro defines on vc6:
//
#endif

#if (_MSC_`

### BOOST_NO_SWPRINTF {#boostnoswprintf}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_SWPRINTF
#endif

//
// check for exception handling support:
#ifndef _CPPUNWIND
#de`

### BOOST_NO_TEMPLATE_ALIASES {#boostnotemplatealiases}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_TEMPLATE_ALIASES
#define BOOST_NO_UNICODE_LITERALS
#define BOOST_NO_VARIADIC_TEMPLA`

### BOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION {#boostnotemplatepartialspecialization}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION
#define BOOST_NO_CV_VOID_SPECIALIZATIONS
#define BO`

### BOOST_NO_TEMPLATE_TEMPLATES {#boostnotemplatetemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_TEMPLATE_TEMPLATES
#define BOOST_NO_SFINAE
#define BOOST_NO_POINTER_TO_MEMBER_TEMPL`

### BOOST_NO_THREADEX {#boostnothreadex}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_THREADEX
#define BOOST_NO_GETSYSTEMTIMEASFILETIME
#define BOOST_NO_SWPRINTF
#endif
`

### BOOST_NO_TWO_PHASE_NAME_LOOKUP {#boostnotwophasenamelookup}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#endif

#if _MSC_VER == 1500 // 1500 == VC++ 9.0
            `

### BOOST_NO_UNICODE_LITERALS {#boostnounicodeliterals}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_UNICODE_LITERALS
#define BOOST_NO_VARIADIC_TEMPLATES

//
// prefix and suffix heade`

### BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE {#boostnousingdeclarationoverloadsfromtypenamebase}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE

//    VC++ 6/7 has member templates`

### BOOST_NO_USING_TEMPLATE {#boostnousingtemplate}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_USING_TEMPLATE
#define BOOST_NO_SWPRINTF
#define BOOST_NO_TEMPLATE_TEMPLATES
#defin`

### BOOST_NO_VARIADIC_TEMPLATES {#boostnovariadictemplates}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_VARIADIC_TEMPLATES

//
// prefix and suffix headers:
//
#ifndef BOOST_ABI_PREFIX
#d`

### BOOST_NO_VOID_RETURNS {#boostnovoidreturns}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/visualc.hpp](./visualc.hpp_docs.md)
- **Context**: `#define BOOST_NO_VOID_RETURNS
#define BOOST_NO_EXCEPTION_STD_NAMESPACE

#if BOOST_MSVC == 1202
#defi`

