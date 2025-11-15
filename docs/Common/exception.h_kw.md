# Keywords: Common/exception.h
---

**Total Keywords**: 8

---

## C

### COMMON_EXCEPTION_H_ {#commonexceptionh}

- **Type**: macro
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `#define COMMON_EXCEPTION_H_

// includes, system
#include <stdlib.h>
#include <exception>
#include <`


## E

### Exception {#exception}

- **Type**: type
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `class Exception`

### Exception_Typ {#exceptiontyp}

- **Type**: type
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `class Exception_Typ`


## L

### LOGIC_EXCEPTION {#logicexception}

- **Type**: macro
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `#define LOGIC_EXCEPTION(msg) \
  Exception<std::logic_error>::throw_it(__FILE__, __LINE__, msg)

//!`


## R

### RANGE_EXCEPTION {#rangeexception}

- **Type**: macro
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `#define RANGE_EXCEPTION(msg) \
  Exception<std::range_error>::throw_it(__FILE__, __LINE__, msg)

///`

### RUNTIME_EXCEPTION {#runtimeexception}

- **Type**: macro
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `#define RUNTIME_EXCEPTION(msg) \
  Exception<std::runtime_error>::throw_it(__FILE__, __LINE__, msg)
`


## S

### Std_Exception {#stdexception}

- **Type**: type
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `class Std_Exception`


## H

### handleException {#handleexception}

- **Type**: function
- **File**: [Common/exception.h](./exception.h_docs.md)
- **Context**: `void handleException(const Exception_Typ &ex) {`

