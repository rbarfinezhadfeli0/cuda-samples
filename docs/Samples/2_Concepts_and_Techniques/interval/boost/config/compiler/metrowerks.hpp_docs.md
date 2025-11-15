# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/metrowerks.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/metrowerks.hpp`
- **Filename**: `metrowerks.hpp`
- **Language**: hpp
- **Size**: 3792 bytes
- **Lines**: 133
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
//  (C) Copyright John Maddock 2001.
//  (C) Copyright Darin Adler 2001.
//  (C) Copyright Peter Dimov 2001.
//  (C) Copyright David Abrahams 2001 - 2002.
//  (C) Copyright Beman Dawes 2001 - 2003.
//  (C) Copyright Stefan Slapeta 2004.
//  Use, modification and distribution are subject to the
//  Boost Software License, Version 1.0. (See accompanying file
//  LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

//  See http://www.boost.org for most recent version.

//  Metrowerks C++ compiler setup:

// locale support is disabled when linking with the dynamic runtime
#ifdef _MSL_NO_LOCALE
#define BOOST_NO_STD_LOCALE
#endif

#if __MWERKS__ <= 0x2301 // 5.3
#define BOOST_NO_FUNCTION_TEMPLATE_ORDERING
#define BOOST_NO_POINTER_TO_MEMBER_CONST
#define BOOST_NO_DEPENDENT_TYPES_IN_TEMPLATE_VALUE_PARAMETERS
#define BOOST_NO_MEMBER_TEMPLATE_KEYWORD
#endif

#if __MWERKS__ <= 0x2401 // 6.2
// #     define BOOST_NO_FUNCTION_TEMPLATE_ORDERING
#endif

#if (__MWERKS__ <= 0x2407) // 7.x
#define BOOST_NO_MEMBER_FUNCTION_SPECIALIZATIONS
#define BOOST_NO_UNREACHABLE_RETURN_DETECTION
#endif

#if (__MWERKS__ <= 0x3003) // 8.x
#define BOOST_NO_SFINAE
#endif

// the "|| !defined(BOOST_STRICT_CONFIG)" part should apply to the last
// tested version *only*:
#if (__MWERKS__ <= 0x3207) || !defined(BOOST_STRICT_CONFIG) // 9.6
#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#define BOOST_NO_IS_ABSTRACT
#endif

#if !__option(wchar_type)
#define BOOST_NO_INTRINSIC_WCHAR_T
#endif

#if !__option(exceptions)
#define BOOST_NO_EXCEPTIONS
#endif

#if (__INTEL__ && _WIN32) || (__POWERPC__ && macintosh)
#if __MWERKS__ == 0x3000
#define BOOST_COMPILER_VERSION 8.0
#elif __MWERKS__ == 0x3001
#define BOOST_COMPILER_VERSION 8.1
#elif __MWERKS__ == 0x3002
#define BOOST_COMPILER_VERSION 8.2
#elif __MWERKS__ == 0x3003
#define BOOST_COMPILER_VERSION 8.3
#elif __MWERKS__ == 0x3200
#define BOOST_COMPILER_VERSION 9.0
#elif __MWERKS__ == 0x3201
#define BOOST_COMPILER_VERSION 9.1
#elif __MWERKS__ == 0x3202
#define BOOST_COMPILER_VERSION 9.2
#elif __MWERKS__ == 0x3204
#define BOOST_COMPILER_VERSION 9.3
#elif __MWERKS__ == 0x3205
#define BOOST_COMPILER_VERSION 9.4
#elif __MWERKS__ == 0x3206
#define BOOST_COMPILER_VERSION 9.5
#elif __MWERKS__ == 0x3207
#define BOOST_COMPILER_VERSION 9.6
#else
#define BOOST_COMPILER_VERSION __MWERKS__
#endif
#else
#define BOOST_COMPILER_VERSION __MWERKS__
#endif

//
// C++0x features
//
//   See boost\config\suffix.hpp for BOOST_NO_LONG_LONG
//
#if __MWERKS__ > 0x3206 && __option(rvalue_refs)
#define BOOST_HAS_RVALUE_REFS
#else
#define BOOST_NO_RVALUE_REFERENCES
#endif
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
#define BOOST_NO_SCOPED_ENUMS
#define BOOST_NO_SFINAE_EXPR
#define BOOST_NO_STATIC_ASSERT
#define BOOST_NO_TEMPLATE_ALIASES
#define BOOST_NO_UNICODE_LITERALS
#define BOOST_NO_VARIADIC_TEMPLATES

#define BOOST_COMPILER "Metrowerks CodeWarrior C++ version " BOOST_STRINGIZE(BOOST_COMPILER_VERSION)

//
// versions check:
// we don't support Metrowerks prior to version 5.3:
#if __MWERKS__ < 0x2301
#error "Compiler not supported or configured - please reconfigure"
#endif
//
// last known and checked version:
#if (__MWERKS__ > 0x3205)
#if defined(BOOST_ASSERT_CONFIG)
#error "Unknown compiler version - please run the configure tests and report the results"
#endif
#endif

```

---
## High-Level Overview
This file is a hpp source file in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **BOOST_NO_STD_LOCALE**: `#endif`
- **BOOST_NO_FUNCTION_TEMPLATE_ORDERING**: `#define BOOST_NO_POINTER_TO_MEMBER_CONST`
- **BOOST_NO_DEPENDENT_TYPES_IN_TEMPLATE_VALUE_PARAMETERS**: `#define BOOST_NO_MEMBER_TEMPLATE_KEYWORD`
- **BOOST_NO_MEMBER_FUNCTION_SPECIALIZATIONS**: `#define BOOST_NO_UNREACHABLE_RETURN_DETECTION`
- **BOOST_NO_SFINAE**: `#endif`
- **BOOST_NO_MEMBER_TEMPLATE_FRIENDS**: `#define BOOST_NO_IS_ABSTRACT`
- **BOOST_NO_INTRINSIC_WCHAR_T**: `#endif`
- **BOOST_NO_EXCEPTIONS**: `#endif`
- **BOOST_COMPILER_VERSION**: `8.0`
- **BOOST_COMPILER_VERSION**: `8.1`
- **BOOST_COMPILER_VERSION**: `8.2`
- **BOOST_COMPILER_VERSION**: `8.3`
- **BOOST_COMPILER_VERSION**: `9.0`
- **BOOST_COMPILER_VERSION**: `9.1`
- **BOOST_COMPILER_VERSION**: `9.2`
- **BOOST_COMPILER_VERSION**: `9.3`
- **BOOST_COMPILER_VERSION**: `9.4`
- **BOOST_COMPILER_VERSION**: `9.5`
- **BOOST_COMPILER_VERSION**: `9.6`
- **BOOST_COMPILER_VERSION**: `__MWERKS__`
- **BOOST_COMPILER_VERSION**: `__MWERKS__`
- **BOOST_HAS_RVALUE_REFS**: `#else`
- **BOOST_NO_RVALUE_REFERENCES**: `#endif`
- **BOOST_NO_AUTO_DECLARATIONS**: `#define BOOST_NO_AUTO_MULTIDECLARATIONS`
- **BOOST_NO_CHAR16_T**: `#define BOOST_NO_CHAR32_T`
- **BOOST_NO_CONCEPTS**: `#define BOOST_NO_CONSTEXPR`
- **BOOST_NO_DECLTYPE**: `#define BOOST_NO_DEFAULTED_FUNCTIONS`
- **BOOST_NO_DELETED_FUNCTIONS**: `#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS`
- **BOOST_NO_EXTERN_TEMPLATE**: `#define BOOST_NO_FUNCTION_TEMPLATE_DEFAULT_ARGS`
- **BOOST_NO_INITIALIZER_LISTS**: `#define BOOST_NO_LAMBDAS`
- **BOOST_NO_NULLPTR**: `#define BOOST_NO_RAW_LITERALS`
- **BOOST_NO_SCOPED_ENUMS**: `#define BOOST_NO_SFINAE_EXPR`
- **BOOST_NO_STATIC_ASSERT**: `#define BOOST_NO_TEMPLATE_ALIASES`
- **BOOST_NO_UNICODE_LITERALS**: `#define BOOST_NO_VARIADIC_TEMPLATES`
- **BOOST_COMPILER**: `"Metrowerks CodeWarrior C++ version " BOOST_STRINGIZE(BOOST_COMPILER_VERSION)`


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

