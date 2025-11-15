# Keywords: Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h
---

**Total Keywords**: 4

---

## I

### Image {#image}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h](./bindlessTexture.h_docs.md)
- **Context**: `struct Image`


## _

### _BINDLESSTEXTURE_CU_ {#bindlesstexturecu}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h](./bindlessTexture.h_docs.md)
- **Context**: `#define _BINDLESSTEXTURE_CU_

// includes, cuda
#include <cuda_runtime.h>
#include <vector_types.h>
`

### _checkHost {#checkhost}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h](./bindlessTexture.h_docs.md)
- **Context**: `void _checkHost(bool test, const char *condition, const char *file, int line, const char *func)
{`


## C

### checkHost {#checkhost}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h](./bindlessTexture.h_docs.md)
- **Context**: `#define checkHost(condition) _checkHost(condition, #condition, __FILE__, __LINE__, __FUNCTION__)

#e`

