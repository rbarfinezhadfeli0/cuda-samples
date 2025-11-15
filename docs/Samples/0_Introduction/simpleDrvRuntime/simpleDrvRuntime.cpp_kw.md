# Keywords: Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp
---

**Total Keywords**: 8

---

## C

### CleanupNoFailure {#cleanupnofailure}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `int CleanupNoFailure(CUcontext &cuContext)
{`


## F

### FATBIN_FILE {#fatbinfile}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `#define FATBIN_FILE "vectorAdd_kernel64.fatbin"
#endif

// Variables
float *h_A;
float *h_B;
float *`


## R

### RandomInit {#randominit}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `void RandomInit(float *data, int n)
{`


## V

### VecAdd_kernel {#vecaddkernel}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `_kernel, cuModule, "VecAdd_kernel"));

    // Allocat`


## C

### check {#check}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `void check(CUresult result, char const *const func, const char *const file, int const line)
{`

### checkCudaDrvErrors {#checkcudadrverrors}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `#define checkCudaDrvErrors(val) check((val), #val, __FILE__, __LINE__)

// Host code
int main(int ar`


## F

### findModulePath {#findmodulepath}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `inline findModulePath(const char *module_file, string &module_path, char **argv, ostringstream &ostr`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleDrvRuntime/simpleDrvRuntime.cpp](./simpleDrvRuntime.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

