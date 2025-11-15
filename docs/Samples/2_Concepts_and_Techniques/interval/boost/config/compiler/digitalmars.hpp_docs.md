# Documentation: Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/digitalmars.hpp
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/interval/boost/config/compiler/digitalmars.hpp`
- **Filename**: `digitalmars.hpp`
- **Language**: hpp
- **Size**: 2612 bytes
- **Lines**: 94
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```hpp
//  Copyright (C) Christof Meerwald 2003
//  Copyright (C) Dan Watkins 2003
//
//  Use, modification and distribution are subject to the
//  Boost Software License, Version 1.0. (See accompanying file
//  LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

//  Digital Mars C++ compiler setup:
#define BOOST_COMPILER __DMC_VERSION_STRING__

#define BOOST_HAS_LONG_LONG
#define BOOST_HAS_PRAGMA_ONCE

#if (__DMC__ <= 0x833)
#define BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL
#define BOOST_NO_TEMPLATE_TEMPLATES
#define BOOST_NEEDS_TOKEN_PASTING_OP_FOR_TOKENS_JUXTAPOSING
#define BOOST_NO_ARRAY_TYPE_SPECIALIZATIONS
#define BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS
#endif
#if (__DMC__ <= 0x840) || !defined(BOOST_STRICT_CONFIG)
#define BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS
#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS
#define BOOST_NO_OPERATORS_IN_NAMESPACE
#define BOOST_NO_UNREACHABLE_RETURN_DETECTION
#define BOOST_NO_SFINAE
#define BOOST_NO_USING_TEMPLATE
#define BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL
#endif

//
// has macros:
#if (__DMC__ >= 0x840)
#define BOOST_HAS_DIRENT_H
#define BOOST_HAS_STDINT_H
#define BOOST_HAS_WINTHREADS
#endif

#if (__DMC__ >= 0x847)
#define BOOST_HAS_EXPM1
#define BOOST_HAS_LOG1P
#endif

//
// Is this really the best way to detect whether the std lib is in namespace std?
//
#include <cstddef>
#if !defined(__STL_IMPORT_VENDOR_CSTD) && !defined(_STLP_IMPORT_VENDOR_CSTD)
#define BOOST_NO_STDC_NAMESPACE
#endif


// check for exception handling support:
#ifndef _CPPUNWIND
#define BOOST_NO_EXCEPTIONS
#endif

//
// C++0x features
//
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

#if __DMC__ < 0x800
#error "Compiler not supported or configured - please reconfigure"
#endif
//
// last known and checked version is ...:
#if (__DMC__ > 0x848)
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
- `cstddef`

### Preprocessor Definitions
- **BOOST_COMPILER**: `__DMC_VERSION_STRING__`
- **BOOST_HAS_LONG_LONG**: `#define BOOST_HAS_PRAGMA_ONCE`
- **BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL**: `#define BOOST_NO_TEMPLATE_TEMPLATES`
- **BOOST_NEEDS_TOKEN_PASTING_OP_FOR_TOKENS_JUXTAPOSING**: `#define BOOST_NO_ARRAY_TYPE_SPECIALIZATIONS`
- **BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS**: `#endif`
- **BOOST_NO_EXPLICIT_FUNCTION_TEMPLATE_ARGUMENTS**: `#define BOOST_NO_MEMBER_TEMPLATE_FRIENDS`
- **BOOST_NO_OPERATORS_IN_NAMESPACE**: `#define BOOST_NO_UNREACHABLE_RETURN_DETECTION`
- **BOOST_NO_SFINAE**: `#define BOOST_NO_USING_TEMPLATE`
- **BOOST_FUNCTION_SCOPE_USING_DECLARATION_BREAKS_ADL**: `#endif`
- **BOOST_HAS_DIRENT_H**: `#define BOOST_HAS_STDINT_H`
- **BOOST_HAS_WINTHREADS**: `#endif`
- **BOOST_HAS_EXPM1**: `#define BOOST_HAS_LOG1P`
- **BOOST_NO_STDC_NAMESPACE**: `#endif`
- **BOOST_NO_EXCEPTIONS**: `#endif`
- **BOOST_NO_AUTO_DECLARATIONS**: `#define BOOST_NO_AUTO_MULTIDECLARATIONS`
- **BOOST_NO_CHAR16_T**: `#define BOOST_NO_CHAR32_T`
- **BOOST_NO_CONCEPTS**: `#define BOOST_NO_CONSTEXPR`
- **BOOST_NO_DECLTYPE**: `#define BOOST_NO_DEFAULTED_FUNCTIONS`
- **BOOST_NO_DELETED_FUNCTIONS**: `#define BOOST_NO_EXPLICIT_CONVERSION_OPERATORS`
- **BOOST_NO_EXTERN_TEMPLATE**: `#define BOOST_NO_INITIALIZER_LISTS`
- **BOOST_NO_LAMBDAS**: `#define BOOST_NO_NULLPTR`
- **BOOST_NO_RAW_LITERALS**: `#define BOOST_NO_RVALUE_REFERENCES`
- **BOOST_NO_SCOPED_ENUMS**: `#define BOOST_NO_SFINAE_EXPR`
- **BOOST_NO_STATIC_ASSERT**: `#define BOOST_NO_TEMPLATE_ALIASES`
- **BOOST_NO_UNICODE_LITERALS**: `#define BOOST_NO_VARIADIC_TEMPLATES`


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

