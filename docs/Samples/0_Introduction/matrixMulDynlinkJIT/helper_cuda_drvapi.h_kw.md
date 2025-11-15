# Keywords: Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h
---

**Total Keywords**: 14

---

## E

### EXIT_WAIVED {#exitwaived}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define EXIT_WAIVED 2
#endif

//////////////////////////////////////////////////////////////////////`


## H

### HELPER_CUDA_DRVAPI_H {#helpercudadrvapih}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define HELPER_CUDA_DRVAPI_H

#include <helper_string.h>
#include <stdio.h>
#include <stdlib.h>
#inc`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define MAX(a, b) (a > b ? a : b)
#endif

#ifndef HELPER_CUDA_DRVAPI_H
inline int ftoi(float value) `

### MapSMtoCores {#mapsmtocores}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `roperly
    printf("MapSMtoCores for SM %d.%d is und`


## _

### _ConvertSMVer2CoresDRV {#convertsmver2coresdrv}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int _ConvertSMVer2CoresDRV(int major, int minor)
{`

### __checkCudaErrors {#checkcudaerrors}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `void __checkCudaErrors(CUresult err, const char *file, const int line)
{`


## C

### checkCudaCapabilitiesDRV {#checkcudacapabilitiesdrv}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `bool checkCudaCapabilitiesDRV(int major_version, int minor_version, int devID)
{`

### checkCudaErrors {#checkcudaerrors}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `#define checkCudaErrors(err) __checkCudaErrors(err, __FILE__, __LINE__)

extern "C" CUresult INIT_ER`


## F

### findCudaDeviceDRV {#findcudadevicedrv}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `CUdevice findCudaDeviceDRV(int argc, const char **argv)
{`

### findIntegratedGPUDrv {#findintegratedgpudrv}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `CUdevice findIntegratedGPUDrv()
{`

### ftoi {#ftoi}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int ftoi(float value) {`


## G

### getCudaAttribute {#getcudaattribute}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `void getCudaAttribute(T *attribute, CUdevice_attribute device_attribute, int device)
{`

### gpuDeviceInitDRV {#gpudeviceinitdrv}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int gpuDeviceInitDRV(int ARGC, const char **ARGV)
{`

### gpuGetMaxGflopsDeviceIdDRV {#gpugetmaxgflopsdeviceiddrv}

- **Type**: function
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/helper_cuda_drvapi.h](./helper_cuda_drvapi.h_docs.md)
- **Context**: `int gpuGetMaxGflopsDeviceIdDRV()
{`

