# Keywords: Common/helper_image.h
---

**Total Keywords**: 29

---

## C

### COMMON_HELPER_IMAGE_H_ {#commonhelperimageh}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define COMMON_HELPER_IMAGE_H_

#include <assert.h>
#include <exception.h>
#include <math.h>
#includ`

### ConverterFromUByte {#converterfromubyte}

- **Type**: type
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `struct ConverterFromUByte`

### ConverterToUByte {#convertertoubyte}

- **Type**: type
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `struct ConverterToUByte`


## E

### EXIT_WAIVED {#exitwaived}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define EXIT_WAIVED 2
#endif

#include <helper_string.h>

// namespace unnamed (internal)
namespace `


## F

### FOPEN {#fopen}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define FOPEN(fHandle, filename, mode) (fHandle = fopen(filename, mode))
#endif
#ifndef FOPEN_FAIL
#`

### FOPEN_FAIL {#fopenfail}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define FOPEN_FAIL(result) (result == NULL)
#endif
#ifndef SSCANF
#define SSCANF sscanf
#endif
#endi`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define MAX(a, b) ((a > b) ? a : b)
#endif

#ifndef EXIT_WAIVED
#define EXIT_WAIVED 2
#endif

#inclu`

### MIN {#min}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define MIN(a, b) ((a < b) ? a : b)
#endif
#ifndef MAX
#define MAX(a, b) ((a > b) ? a : b)
#endif

#`


## S

### SSCANF {#sscanf}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define SSCANF sscanf
#endif
#endif

inline bool __loadPPM(const char *file, unsigned char **data, u`


## _

### __MIN_EPSILON_ERROR {#minepsilonerror}

- **Type**: macro
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `#define __MIN_EPSILON_ERROR 1e-3f
#endif

//////////////////////////////////////////////////////////`

### __loadPPM {#loadppm}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool __loadPPM(const char *file, unsigned char **data, unsigned int *w,
                      unsign`

### __savePPM {#saveppm}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool __savePPM(const char *file, unsigned char *data, unsigned int w,
                      unsigned`


## C

### compareData {#comparedata}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool compareData(const T *reference, const T *data,
                        const unsigned int len, `

### compareDataAsFloatThreshold {#comparedataasfloatthreshold}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool compareDataAsFloatThreshold(const T *reference, const T *data,
                                `


## S

### sdkCompareBin2BinFloat {#sdkcomparebin2binfloat}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkCompareBin2BinFloat(const char *src_file, const char *ref_file,
                            `

### sdkCompareBin2BinUint {#sdkcomparebin2binuint}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkCompareBin2BinUint(const char *src_file, const char *ref_file,
                             `

### sdkCompareL2fe {#sdkcomparel2fe}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkCompareL2fe(const float *reference, const float *data,
                           const unsi`

### sdkComparePGM {#sdkcomparepgm}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkComparePGM(const char *src_file, const char *ref_file,
                          const float`

### sdkComparePPM {#sdkcompareppm}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkComparePPM(const char *src_file, const char *ref_file,
                          const float`

### sdkDumpBin {#sdkdumpbin}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `void sdkDumpBin(void *data, unsigned int bytes, const char *filename) {`

### sdkLoadPGM {#sdkloadpgm}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkLoadPGM(const char *file, T **data, unsigned int *w,
                       unsigned int *h)`

### sdkLoadPPM4 {#sdkloadppm4}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkLoadPPM4(const char *file, T **data, unsigned int *w,
                        unsigned int *`

### sdkLoadPPM4ub {#sdkloadppm4ub}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkLoadPPM4ub(const char *file, unsigned char **data,
                          unsigned int *w`

### sdkLoadPPMub {#sdkloadppmub}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkLoadPPMub(const char *file, unsigned char **data,
                         unsigned int *w, `

### sdkReadFile {#sdkreadfile}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkReadFile(const char *filename, T **data, unsigned int *len,
                        bool ver`

### sdkReadFileBlocks {#sdkreadfileblocks}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkReadFileBlocks(const char *filename, T **data, unsigned int *len,
                          `

### sdkSavePGM {#sdksavepgm}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkSavePGM(const char *file, T *data, unsigned int w,
                       unsigned int h) {`

### sdkSavePPM4ub {#sdksaveppm4ub}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkSavePPM4ub(const char *file, unsigned char *data, unsigned int w,
                          `

### sdkWriteFile {#sdkwritefile}

- **Type**: function
- **File**: [Common/helper_image.h](./helper_image.h_docs.md)
- **Context**: `bool sdkWriteFile(const char *filename, const T *data, unsigned int len,
                         co`

