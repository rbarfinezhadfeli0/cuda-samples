# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h
---

**Total Keywords**: 15

---

## A

### ATTR_SIZE {#attrsize}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define ATTR_SIZE   20
#define DEFAULT_GPU 0

#define checkNvSciErrors(call)                        `


## C

### CUDA_NVSCIBUF_MULTIPLANAR_H {#cudanvscibufmultiplanarh}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define CUDA_NVSCIBUF_MULTIPLANAR_H

#include <cuda.h>
#include <cuda_runtime.h>
#include <helper_cu`

### Caller {#caller}

- **Type**: type
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `class Caller`


## D

### DEFAULT_GPU {#defaultgpu}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define DEFAULT_GPU 0

#define checkNvSciErrors(call)                                   \
    do {  `


## N

### NvSciBufAttrKeyValuePair {#nvscibufattrkeyvaluepair}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `   attrListOut;
    NvSciBufAttrKeyValuePair pairArrayOut[ATTR_S`

### NvSciBufAttrList {#nvscibufattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `ller
{
private:
    NvSciBufAttrList         attrListOut`

### NvSciBufImageAttributes {#nvscibufimageattributes}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `*caller2);
};

enum NvSciBufImageAttributes {
    PLANE_SIZE,
 `

### NvSciError {#nvscierror}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `          \
        NvSciError _status = call;    `

### NvSciError_Success {#nvscierrorsuccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `      \
        if (NvSciError_Success != _status) {      `


## P

### PLANAR_CHROMA_HEIGHT_ORDER {#planarchromaheightorder}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define PLANAR_CHROMA_HEIGHT_ORDER 2

#define ATTR_SIZE   20
#define DEFAULT_GPU 0

#define checkNvS`

### PLANAR_CHROMA_WIDTH_ORDER {#planarchromawidthorder}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define PLANAR_CHROMA_WIDTH_ORDER  2
#define PLANAR_CHROMA_HEIGHT_ORDER 2

#define ATTR_SIZE   20
#d`

### PLANAR_NUM_PLANES {#planarnumplanes}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define PLANAR_NUM_PLANES          3
#define PLANAR_CHROMA_WIDTH_ORDER  2
#define PLANAR_CHROMA_HEIG`


## C

### checkCudaDrvErrors {#checkcudadrverrors}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define checkCudaDrvErrors(call)                           \
    do {                               `

### checkNvSciErrors {#checknvscierrors}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `#define checkNvSciErrors(call)                                   \
    do {                         `

### cudaNvSciBufMultiplanar {#cudanvscibufmultiplanar}

- **Type**: type
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/cudaNvSciBufMultiplanar.h](./cudaNvSciBufMultiplanar.h_docs.md)
- **Context**: `class cudaNvSciBufMultiplanar`

