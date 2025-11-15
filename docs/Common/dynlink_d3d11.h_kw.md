# Keywords: Common/dynlink_d3d11.h
---

**Total Keywords**: 14

---

## C

### CreateDXGIFactory {#createdxgifactory}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `LPCREATEDXGIFACTORY)CreateDXGIFactory;
    return true;
#`

### CreateDXGIFactory1 {#createdxgifactory1}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `ddress(s_hModDXGI, "CreateDXGIFactory1");
        }

     `


## D

### DriverType {#drivertype}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `er, D3D_DRIVER_TYPE DriverType, HMODULE Software, `


## E

### ExtractIcon {#extracticon}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `<shellapi.h> // for ExtractIcon()
#include <new.h> `


## F

### FeatureLevels {#featurelevels}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `gs, __in_ecount_opt(FeatureLevels) CONST D3D_FEATURE_`

### FreeLibrary {#freelibrary}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `DXGI)
    {
        FreeLibrary(s_hModDXGI);
      `


## G

### GetProcAddress {#getprocaddress}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `LPD3D11CREATEDEVICE)GetProcAddress(s_hModD3D11, "D3D11`


## I

### InitCommonControls {#initcommoncontrols}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `<commctrl.h> // for InitCommonControls() 
#include <shella`


## L

### LoadLibrary {#loadlibrary}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `d
    s_hModD3D11 = LoadLibrary("d3d11.dll");

    `


## P

### ProcAddresses {#procaddresses}

- **Type**: identifier
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: ` guarantee that all ProcAddresses were found.
    if `


## S

### STRSAFE_NO_DEPRECATE {#strsafenodeprecate}

- **Type**: macro
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `#define STRSAFE_NO_DEPRECATE

#ifndef STRSAFE_NO_DEPRECATE
#pragma deprecated("strncpy")
#pragma dep`


## _

### _DYNLINK_D3D11_H_ {#dynlinkd3d11h}

- **Type**: macro
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `#define _DYNLINK_D3D11_H_

// Standard Windows includes
#include <windows.h>
#include <initguid.h>
#`


## D

### dynlinkLoadD3D11API {#dynlinkloadd3d11api}

- **Type**: function
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `bool dynlinkLoadD3D11API(void)
{`

### dynlinkUnloadD3D11API {#dynlinkunloadd3d11api}

- **Type**: function
- **File**: [Common/dynlink_d3d11.h](./dynlink_d3d11.h_docs.md)
- **Context**: `bool dynlinkUnloadD3D11API(void)
{`

