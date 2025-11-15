# Keywords: Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp
---

**Total Keywords**: 25

---

## B

### BasicBlock {#basicblock}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `ernel function.
    BasicBlock *entry = BasicBlock`


## C

### CommandLine {#commandline}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `clude <llvm/Support/CommandLine.h>
#include <llvm/S`

### ConstantAsMetadata {#constantasmetadata}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `                    ConstantAsMetadata::get(ConstantInt::g`

### ConstantInt {#constantint}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `tantAsMetadata::get(ConstantInt::getTrue(context))}`

### CreateCall {#createcall}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `entry);
    builder.CreateCall(mandelbrotFunc, fun`

### CreateRetVoid {#createretvoid}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `gin());
    builder.CreateRetVoid();

    // Create k`


## E

### ExternalLinkage {#externallinkage}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `uncTy, GlobalValue::ExternalLinkage, "kernel", *mod);
 `


## F

### FileSystem {#filesystem}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `clude <llvm/Support/FileSystem.h>
#include <llvm/S`

### FunctionCallee {#functioncallee}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `ramTys, false);
    FunctionCallee mandelbrotFunc     `

### FunctionType {#functiontype}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `tGenericPtrTy};
    FunctionType  *mandelbrotTy     `


## G

### GlobalValue {#globalvalue}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `ion::Create(funcTy, GlobalValue::ExternalLinkage, "`


## N

### NamedMDNode {#namedmdnode}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `ntext, mdVals);
    NamedMDNode *nvvmAnnot = mod->g`


## P

### ParseCommandLineOptions {#parsecommandlineoptions}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `r **argv)
{
    cl::ParseCommandLineOptions(argc, argv, "cuda-c`

### PointerType {#pointertype}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `floatGenericPtrTy = PointerType::get(floatTy, /* ad`


## S

### SaveCubin {#savecubin}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `tatic cl::opt<bool> SaveCubin("save-cubin", cl::d`

### SaveIR {#saveir}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `tatic cl::opt<bool> SaveIR("save-ir", cl::desc`

### SavePTX {#saveptx}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `tatic cl::opt<bool> SavePTX("save-ptx", cl::des`

### SmallString {#smallstring}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: ` (void *)main);
    SmallString<256> libpath(libpat`

### StringExtras {#stringextras}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `
#include <llvm/ADT/StringExtras.h>
#include <llvm/I`


## V

### ValueAsMetadata {#valueasmetadata}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `ta    *mdVals[]  = {ValueAsMetadata::get(func),
       `


## _

### __checkCudaErrors {#checkcudaerrors}

- **Type**: function
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `void __checkCudaErrors(CUresult err, const char *filename, int line)
{`


## C

### checkCudaErrors {#checkcudaerrors}

- **Type**: macro
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `#define checkCudaErrors(err) __checkCudaErrors(err, __FILE__, __LINE__)
static void __checkCudaError`

### checkNVVMCall {#checknvvmcall}

- **Type**: function
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `void checkNVVMCall(nvvmResult res)
{`


## G

### generatePtx {#generateptx}

- **Type**: function
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `string generatePtx(const std::string &module, int devMajor, int devMinor, const char *moduleName)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/7_libNVVM/cuda-c-linking/cuda-c-linking.cpp](./cuda-c-linking.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

