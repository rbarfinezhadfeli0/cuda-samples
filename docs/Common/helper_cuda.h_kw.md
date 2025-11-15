# Keywords: Common/helper_cuda.h
---

**Total Keywords**: 19

---

## C

### COMMON_HELPER_CUDA_H_ {#commonhelpercudah}

- **Type**: macro
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `#define COMMON_HELPER_CUDA_H_

#pragma once

#include <stdint.h>
#include <stdio.h>
#include <stdlib`


## E

### EXIT_WAIVED {#exitwaived}

- **Type**: macro
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `#define EXIT_WAIVED 2
#endif

// Note, it is required that your SDK sample to include the proper hea`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `#define MAX(a, b) (a > b ? a : b)
#endif

// Float To Int conversion
inline int ftoi(float value) {
`

### MapSMtoArchName {#mapsmtoarchname}

- **Type**: identifier
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `ly
  printf(
      "MapSMtoArchName for SM %d.%d is und`

### MapSMtoCores {#mapsmtocores}

- **Type**: identifier
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `ly
  printf(
      "MapSMtoCores for SM %d.%d is und`


## N

### NppStatus {#nppstatus}

- **Type**: identifier
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: ` *_cudaGetErrorEnum(NppStatus error) {
  switch (`


## _

### _ConvertSMVer2Cores {#convertsmver2cores}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `int _ConvertSMVer2Cores(int major, int minor) {`

### __getLastCudaError {#getlastcudaerror}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `void __getLastCudaError(const char *errorMessage, const char *file,
                               c`

### __printLastCudaError {#printlastcudaerror}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `void __printLastCudaError(const char *errorMessage, const char *file,
                              `


## C

### check {#check}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `void check(T result, char const *const func, const char *const file,
           int const line) {`

### checkCudaCapabilities {#checkcudacapabilities}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `bool checkCudaCapabilities(int major_version, int minor_version) {`

### checkCudaErrors {#checkcudaerrors}

- **Type**: macro
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `#define checkCudaErrors(val) check((val), #val, __FILE__, __LINE__)

// This will output the proper `


## F

### findCudaDevice {#findcudadevice}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `int findCudaDevice(int argc, const char **argv) {`

### findIntegratedGPU {#findintegratedgpu}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `int findIntegratedGPU() {`

### ftoi {#ftoi}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `int ftoi(float value) {`


## G

### getLastCudaError {#getlastcudaerror}

- **Type**: macro
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `#define getLastCudaError(msg) __getLastCudaError(msg, __FILE__, __LINE__)

inline void __getLastCuda`

### gpuDeviceInit {#gpudeviceinit}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `int gpuDeviceInit(int devID) {`

### gpuGetMaxGflopsDeviceId {#gpugetmaxgflopsdeviceid}

- **Type**: function
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `int gpuGetMaxGflopsDeviceId() {`


## P

### printLastCudaError {#printlastcudaerror}

- **Type**: macro
- **File**: [Common/helper_cuda.h](./helper_cuda.h_docs.md)
- **Context**: `#define printLastCudaError(msg) __printLastCudaError(msg, __FILE__, __LINE__)

inline void __printLa`

