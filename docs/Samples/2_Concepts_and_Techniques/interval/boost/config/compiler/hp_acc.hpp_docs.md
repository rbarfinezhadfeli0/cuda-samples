# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/hp_acc.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/hp_acc.hpp`
- **Filename**: `hp_acc.hpp`
- **Language**: hpp
- **Size**: 3982 bytes
- **Lines**: 128
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
//  (C) Copyright John Maddock 2001 - 2003.
//  (C) Copyright Jens Maurer 2001 - 2003.
//  (C) Copyright Aleksey Gurtovoy 2002.
//  (C) Copyright David Abrahams 2002 - 2003.
//  (C) Copyright Toon Knapen 2003.
//  (C) Copyright Boris Gubenko 2006 - 2007.
//  Use, modification and distribution are subject to the
//  Boost Software License, Version 1.0. (See accompanying file
//  LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

//  See http://www.boost.org for most recent version.

//  HP aCC C++ compiler setup:

#if defined(__EDG__)
#include "boost/config/compiler/common_edg.hpp"
#endif

#if (__HP_aCC <= 33100)
#define BOOST_NO_INTEGRAL_INT64_T
#define BOOST_NO_OPERATORS_IN_NAMESPACE
#if !defined(_NAMESPACE_STD)
#define BOOST_NO_STD_LOCALE
#define BOOST_NO_STRINGSTREAM
#endif
#endif

#if (__HP_aCC <= 33300)
// member templates are sufficiently broken that we disable them for now
#define BOOST_NO_MEMBER_TEMPLATES
#define BOOST_NO_DEPENDENT_NESTED_DERIVATIONS
#define BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE
#endif

#if (__HP_aCC <= 38000)
#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#endif

#if (__HP_aCC > 50000) && (__HP_aCC < 60000)
#define BOOST_NO_UNREACHABLE_RETURN_DETECTION
#define BOOST_NO_TEMPLATE_TEMPLATES
#define BOOST_NO_SWPRINTF
#define BOOST_NO_DEPENDENT_TYPES_IN_TEMPLATE_VALUE_PARAMETERS
#define BOOST_NO_IS_ABSTRACT
#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#endif

// optional features rather than defects:
#if (__HP_aCC >= 33900)
#define BOOST_HAS_LONG_LONG
#define BOOST_HAS_PARTIAL_STD_ALLOCATOR
#endif

#if (__HP_aCC >= 50000) && (__HP_aCC <= 53800) || (__HP_aCC < 31300)
#define BOOST_NO_MEMBER_TEMPLATE_KEYWORD
#endif

// This macro should not be defined when compiling in strict ansi
// mode, but, currently, we don't have the ability to determine
// what standard mode we are compiling with. Some future version
// of aCC6 compiler will provide predefined macros reflecting the
// compilation options, including the standard mode.
#if (__HP_aCC >= 60000) || ((__HP_aCC > 38000) && defined(__hpxstd98))
#define BOOST_NO_TWO_PHASE_NAME_LOOKUP
#endif

#define BOOST_COMPILER "HP aCC version " BOOST_STRINGIZE(__HP_aCC)

//
// versions check:
// we don't support HP aCC prior to version 33000:
#if __HP_aCC < 33000
#error "Compiler not supported or configured - please reconfigure"
#endif

//
// Extended checks for supporting aCC on PA-RISC
#if __HP_aCC > 30000 && __HP_aCC < 50000
#if __HP_aCC < 38000
// versions prior to version A.03.80 not supported
#error "Compiler version not supported - version A.03.80 or higher is required"
#elif !defined(__hpxstd98)
// must compile using the option +hpxstd98 with version A.03.80 and above
#error "Compiler option '+hpxstd98' is required for proper support"
#endif // PA-RISC
#endif

//
// C++0x features
//
//   See boost\config\suffix.hpp for BOOST_NO_LONG_LONG
//
#if !defined(__EDG__)

#define BOOST_NO_AUTO_DECLARATIONS
#define BOOST_NO_AUTO_MULTIDECLARATIONS
#define BOOST_NO_CHAR16_T
#define BOOST_NO_CHAR32_T
#define BOOST_NO_CONCEPTS
#define BOOST_NO_CONSTEXPR
#define BOOST_NO_DECLTYPE
#define BOOST_NO_DEFAULTED_FUNCTIONS
#define BOOST_NO_DELETED_FUNCTIONS
#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS
#define BOOST_NO_EXTERN_TEMPLATE
#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS
#define BOOST_NO_INITIALIZER_LISTS
#define BOOST_NO_LAMBDAS
#define BOOST_NO_NULLPTR
#define BOOST_NO_RAW_LITERALS
#define BOOST_NO_RVALUE_REFERENCES
#define BOOST_NO_SCOPED_ENUMS
#define BOOST_NO_SFINAE_EXPR
#define BOOST_NO_STATIC_ASSERT
#define BOOST_NO_TEMPLATE_ALIASES
#define BOOST_NO_UNICODE_LITERALS
#define BOOST_NO_VARIADIC_TEMPLATES
#endif

//
// last known and checked version for HP-UX/ia64 is 61300
// last known and checked version for PA-RISC is 38000
#if ((__HP_aCC > 61300) || ((__HP_aCC > 38000) && defined(__hpxstd98)))
#if defined(BOOST_ASSERT_CONFIG)
#error "Unknown compiler version - please run the configure tests and report the results"
#endif
#endif

```

---
## High-Level Overview
This file is a hpp source file in the CUDA Samples repository.

**Dependencies**: 1 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `boost/config/compiler/common_edg.hpp`

### Preprocessor Definitions
- **BOOST_NO_INTEGRAL_INT64_T**: `#define BOOST_NO_OPERATORS_IN_NAMESPACE`
- **BOOST_NO_STD_LOCALE**: `#define BOOST_NO_STRINGSTREAM`
- **BOOST_NO_MEMBER_TEMPLATES**: `#define BOOST_NO_DEPENDENT_NESTED_DERIVATIONS`
- **BOOST_NO_USING_DECLARATION_OVERLOADS_FROM_TYPENAME_BASE**: `#endif`
- **BOOST_NO_TWO_PHASE_NAME_LOOKUP**: `#endif`
- **BOOST_NO_UNREACHABLE_RETURN_DETECTION**: `#define BOOST_NO_TEMPLATE_TEMPLATES`
- **BOOST_NO_SWPRINTF**: `#define BOOST_NO_DEPENDENT_TYPES_IN_TEMPLATE_VALUE_PARAMETERS`
- **BOOST_NO_IS_ABSTRACT**: `#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS`
- **BOOST_HAS_LONG_LONG**: `#define BOOST_HAS_PARTIAL_STD_ALLOCATOR`
- **BOOST_NO_MEMBER_TEMPLATE_KEYWORD**: `#endif`
- **BOOST_NO_TWO_PHASE_NAME_LOOKUP**: `#endif`
- **BOOST_COMPILER**: `"HP aCC version " BOOST_STRINGIZE(__HP_aCC)`
- **BOOST_NO_AUTO_DECLARATIONS**: `#define BOOST_NO_AUTO_MULTIDECLARATIONS`
- **BOOST_NO_CHAR16_T**: `#define BOOST_NO_CHAR32_T`
- **BOOST_NO_CONCEPTS**: `#define BOOST_NO_CONSTEXPR`
- **BOOST_NO_DECLTYPE**: `#define BOOST_NO_DEFAULTED_FUNCTIONS`
- **BOOST_NO_DELETED_FUNCTIONS**: `#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS`
- **BOOST_NO_EXTERN_TEMPLATE**: `#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS`
- **BOOST_NO_INITIALIZER_LISTS**: `#define BOOST_NO_LAMBDAS`
- **BOOST_NO_NULLPTR**: `#define BOOST_NO_RAW_LITERALS`
- **BOOST_NO_RVALUE_REFERENCES**: `#define BOOST_NO_SCOPED_ENUMS`
- **BOOST_NO_SFINAE_EXPR**: `#define BOOST_NO_STATIC_ASSERT`
- **BOOST_NO_TEMPLATE_ALIASES**: `#define BOOST_NO_UNICODE_LITERALS`
- **BOOST_NO_VARIADIC_TEMPLATES**: `#endif`


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

