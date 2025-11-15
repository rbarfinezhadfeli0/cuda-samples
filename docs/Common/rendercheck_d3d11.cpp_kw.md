# Keywords: Common/rendercheck_d3d11.cpp
---

**Total Keywords**: 12

---

## A

### ActiveRenderTargetToPPM {#activerendertargettoppm}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `T CheckRenderD3D11::ActiveRenderTargetToPPM(ID3D11Device *pDevi`


## B

### BindFlags {#bindflags}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `sc(&desc);
    desc.BindFlags = 0;
    desc.CPUAc`


## C

### CheckRenderD3D11 {#checkrenderd3d11}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `k_d3d11.h>

HRESULT CheckRenderD3D11::ActiveRenderTarget`

### CopyResource {#copyresource}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `}

    pDeviceCtxt->CopyResource(pTargetTexture,pSou`

### CreateTexture2D {#createtexture2d}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `if (FAILED(pDevice->CreateTexture2D(&desc,NULL,&pTarget`


## G

### GetDesc {#getdesc}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `    pSourceTexture->GetDesc(&desc);
    desc.Bi`

### GetImmediateContext {#getimmediatecontext}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `eCtxt;
    pDevice->GetImmediateContext(&pDeviceCtxt);
    `

### GetResource {#getresource}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `e = NULL;
    pRTV->GetResource(&pSourceResource);
`

### GetType {#gettype}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `ype;
    pResource->GetType(&rType);

    if (r`


## R

### ResourceToPPM {#resourcetoppm}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `ource);

    return ResourceToPPM(pDevice,pSourceReso`

### RowPitch {#rowpitch}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `iHeight*mappedTex2D.RowPitch,desc.Width*4);
    `


## S

### SurfaceToPPM {#surfacetoppm}

- **Type**: identifier
- **File**: [Common/rendercheck_d3d11.cpp](./rendercheck_d3d11.cpp_docs.md)
- **Context**: `  {
        printf("SurfaceToPPM: pResource is not a`

