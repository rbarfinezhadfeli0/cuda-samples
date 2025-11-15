# Keywords: Samples/7_libNVVM/ptxgen/ptxgen.c
---

**Total Keywords**: 13

---

## _

### __getLibDeviceName {#getlibdevicename}

- **Type**: macro
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `#define __getLibDeviceName(MAJOR, MINOR) ("/libdevice/libdevice." #MAJOR #MINOR ".bc")

#define getL`

### __getLibnvvmHome {#getlibnvvmhome}

- **Type**: macro
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `#define __getLibnvvmHome(NVVM_HOME) (#NVVM_HOME)

typedef enum {
    PTXGEN_SUCCESS                 `

### _getLibDeviceName {#getlibdevicename}

- **Type**: macro
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `#define _getLibDeviceName(MAJOR, MINOR)  __getLibDeviceName(MAJOR, MINOR)
#define __getLibDeviceName`

### _getLibnvvmHome {#getlibnvvmhome}

- **Type**: macro
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `#define _getLibnvvmHome(NVVM_HOME)  __getLibnvvmHome(NVVM_HOME)
#define __getLibnvvmHome(NVVM_HOME) `


## A

### addFileToProgram {#addfiletoprogram}

- **Type**: function
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `PTXGenStatus addFileToProgram(const char *filename, nvvmProgram prog, PTXGENInput inputType)
{`


## D

### dumpCompilationLog {#dumpcompilationlog}

- **Type**: function
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `PTXGenStatus dumpCompilationLog(nvvmProgram prog)
{`


## G

### generatePTX {#generateptx}

- **Type**: function
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `PTXGenStatus
generatePTX(unsigned numOptions, const char **options, unsigned numFilenames, const cha`

### getLibDeviceName {#getlibdevicename}

- **Type**: macro
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `#define getLibDeviceName()               _getLibDeviceName(LIBDEVICE_MAJOR_VERSION, LIBDEVICE_MINOR_`

### getLibDevicePath {#getlibdevicepath}

- **Type**: function
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `PTXGenStatus getLibDevicePath(char **buffer)
{`

### getLibnvvmHome {#getlibnvvmhome}

- **Type**: macro
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `#define getLibnvvmHome()            _getLibnvvmHome(LIBNVVM_HOME)
#define _getLibnvvmHome(NVVM_HOME)`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`


## S

### showUsage {#showusage}

- **Type**: function
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `void showUsage(void)
{`

### stat {#stat}

- **Type**: type
- **File**: [Samples/7_libNVVM/ptxgen/ptxgen.c](./ptxgen.c_docs.md)
- **Context**: `struct stat`

