# Keywords: Common/helper_cuda_drvapi.h
---

**Total Keywords**: 15

---

## C

### COMMON_HELPER_CUDA_DRVAPI_H_ {#commonhelpercudadrvapih}

- **Type**: macro
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define COMMON_HELPER_CUDA_DRVAPI_H_

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#in`


## E

### EXIT_WAIVED {#exitwaived}

- **Type**: macro
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define EXIT_WAIVED 2
#endif

//////////////////////////////////////////////////////////////////////`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define MAX(a, b) (a > b ? a : b)
#endif

#ifndef COMMON_HELPER_CUDA_H_
inline int ftoi(float value)`

### MapSMtoCores {#mapsmtocores}

- **Type**: identifier
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `ly
  printf(
      "MapSMtoCores for SM %d.%d is und`


## _

### _ConvertSMVer2CoresDRV {#convertsmver2coresdrv}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int _ConvertSMVer2CoresDRV(int major, int minor) {`

### __checkCudaErrors {#checkcudaerrors}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `void __checkCudaErrors(CUresult err, const char *file, const int line) {`


## C

### checkCudaCapabilitiesDRV {#checkcudacapabilitiesdrv}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `bool checkCudaCapabilitiesDRV(int major_version, int minor_version,
                                `

### checkCudaErrors {#checkcudaerrors}

- **Type**: macro
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define checkCudaErrors(err) __checkCudaErrors(err, __FILE__, __LINE__)

// These are the inline ver`


## F

### findCudaDeviceDRV {#findcudadevicedrv}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `CUdevice findCudaDeviceDRV(int argc, const char **argv) {`

### findFatbinPath {#findfatbinpath}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `inline findFatbinPath(const char *module_file, std::string &module_path, char **argv, std::ostringst`

### findIntegratedGPUDrv {#findintegratedgpudrv}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `CUdevice findIntegratedGPUDrv() {`

### ftoi {#ftoi}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int ftoi(float value) {`


## G

### getCudaAttribute {#getcudaattribute}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `void getCudaAttribute(T *attribute, CUdevice_attribute device_attribute,
                           `

### gpuDeviceInitDRV {#gpudeviceinitdrv}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int gpuDeviceInitDRV(int ARGC, const char **ARGV) {`

### gpuGetMaxGflopsDeviceIdDRV {#gpugetmaxgflopsdeviceiddrv}

- **Type**: function
- **File**: [Common/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int gpuGetMaxGflopsDeviceIdDRV() {`

