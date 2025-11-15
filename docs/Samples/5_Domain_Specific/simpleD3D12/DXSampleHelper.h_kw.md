# Keywords: Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h
---

**Total Keywords**: 25

---

## C

### CalculateConstantBufferByteSize {#calculateconstantbufferbytesize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `UINT CalculateConstantBufferByteSize(UINT byteSize)
{`

### ComPtr {#comptr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `             // For ComPtr
#include <wrl/wrapp`

### CompileShader {#compileshader}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `L::ComPtr<ID3DBlob> CompileShader(const std::wstring `

### CreateFile2 {#createfile2}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `rs::FileHandle file(CreateFile2(filename, GENERIC_R`


## E

### EndOfFile {#endoffile}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `}

    if (fileInfo.EndOfFile.HighPart != 0) {
  `


## F

### FileHandle {#filehandle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `h> // For Wrappers::FileHandle
// Note that while `

### FileStandardInfo {#filestandardinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `andleEx(file.Get(), FileStandardInfo, &fileInfo, sizeof(`


## G

### GetBufferPointer {#getbufferpointer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `ngA((char *)errors->GetBufferPointer());
    }
    Throw`

### GetFileInformationByHandleEx {#getfileinformationbyhandleex}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `Info = {};
    if (!GetFileInformationByHandleEx(file.Get(), FileSta`


## H

### HelloTexture {#hellotexture}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `D3D12HelloWorld/src/HelloTexture,
  which is license`

### HighPart {#highpart}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `(fileInfo.EndOfFile.HighPart != 0) {
        thr`

### HrException {#hrexception}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `class HrException`

### HrToString {#hrtostring}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `string HrToString(HRESULT hr)
{`


## L

### LowPart {#lowpart}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `(fileInfo.EndOfFile.LowPart));
    *size = file`


## N

### NAME_D3D12_OBJECT {#named3d12object}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `#define NAME_D3D12_OBJECT(x)            SetName((x).Get(), L#x)
#define NAME_D3D12_OBJECT_INDEXED(x,`

### NAME_D3D12_OBJECT_INDEXED {#named3d12objectindexed}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `#define NAME_D3D12_OBJECT_INDEXED(x, n) SetNameIndexed((x)[n].Get(), L#x, n)

inline UINT CalculateC`


## O

### OutputDebugStringA {#outputdebugstringa}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: ` nullptr) {
        OutputDebugStringA((char *)errors->Get`


## R

### ReadDataFromFile {#readdatafromfile}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `HRESULT ReadDataFromFile(LPCWSTR filename, byte **data, UINT *size)
{`

### ReadFile {#readfile}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `.LowPart;

    if (!ReadFile(file.Get(), *data, `

### ResetComPtrArray {#resetcomptrarray}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `void ResetComPtrArray(T *comPtrArray)
{`

### ResetUniquePtrArray {#resetuniqueptrarray}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `void ResetUniquePtrArray(T *uniquePtrArray)
{`


## S

### SAFE_RELEASE {#saferelease}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `#define SAFE_RELEASE(p) \
    if (p)              \
    (p)->Release()

inline void ThrowIfFailed(HR`

### SetName {#setname}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `void SetName(ID3D12Object *, LPCWSTR) {`

### SetNameIndexed {#setnameindexed}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `void SetNameIndexed(ID3D12Object *, LPCWSTR, UINT) {`


## T

### ThrowIfFailed {#throwiffailed}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D12/DXSampleHelper.h](./DXSampleHelper.h_docs.md)
- **Context**: `void ThrowIfFailed(HRESULT hr)
{`

