# Keywords: Common/helper_string.h
---

**Total Keywords**: 17

---

## C

### COMMON_HELPER_STRING_H_ {#commonhelperstringh}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define COMMON_HELPER_STRING_H_

#include <stdio.h>
#include <stdlib.h>
#include <fstream>
#include `


## E

### EXIT_WAIVED {#exitwaived}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define EXIT_WAIVED 2
#endif

// CUDA Utility Helper Functions
inline int stringRemoveDelimiter(char`


## F

### FOPEN {#fopen}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define FOPEN(fHandle, filename, mode) (fHandle = fopen(filename, mode))
#endif
#ifndef FOPEN_FAIL
#`

### FOPEN_FAIL {#fopenfail}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define FOPEN_FAIL(result) (result == NULL)
#endif
#ifndef SSCANF
#define SSCANF sscanf
#endif
#ifnd`


## S

### SPRINTF {#sprintf}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define SPRINTF sprintf
#endif
#endif

#ifndef EXIT_WAIVED
#define EXIT_WAIVED 2
#endif

// CUDA Uti`

### SSCANF {#sscanf}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define SSCANF sscanf
#endif
#ifndef SPRINTF
#define SPRINTF sprintf
#endif
#endif

#ifndef EXIT_WAI`

### STRCASECMP {#strcasecmp}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define STRCASECMP strcasecmp
#endif
#ifndef STRNCASECMP
#define STRNCASECMP strncasecmp
#endif
#ifn`

### STRCPY {#strcpy}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define STRCPY(sFilePath, nLength, sPath) strcpy(sFilePath, sPath)
#endif

#ifndef FOPEN
#define FOP`

### STRNCASECMP {#strncasecmp}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define STRNCASECMP strncasecmp
#endif
#ifndef STRCPY
#define STRCPY(sFilePath, nLength, sPath) strc`


## _

### _CRT_SECURE_NO_DEPRECATE {#crtsecurenodeprecate}

- **Type**: macro
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `#define _CRT_SECURE_NO_DEPRECATE
#endif
#ifndef STRCASECMP
#define STRCASECMP _stricmp
#endif
#ifnde`


## C

### checkCmdLineFlag {#checkcmdlineflag}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `bool checkCmdLineFlag(const int argc, const char **argv,
                             const char *st`


## G

### getCmdLineArgumentFloat {#getcmdlineargumentfloat}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `float getCmdLineArgumentFloat(const int argc, const char **argv,
                                   `

### getCmdLineArgumentInt {#getcmdlineargumentint}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `int getCmdLineArgumentInt(const int argc, const char **argv,
                                 const `

### getCmdLineArgumentString {#getcmdlineargumentstring}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `bool getCmdLineArgumentString(const int argc, const char **argv,
                                   `

### getCmdLineArgumentValue {#getcmdlineargumentvalue}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `bool getCmdLineArgumentValue(const int argc, const char **argv,
                                    `

### getFileExtension {#getfileextension}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `int getFileExtension(char *filename, char **extension) {`


## S

### stringRemoveDelimiter {#stringremovedelimiter}

- **Type**: function
- **File**: [Common/helper_string.h](./helper_string.h_docs.md)
- **Context**: `int stringRemoveDelimiter(char delimiter, const char *string) {`

