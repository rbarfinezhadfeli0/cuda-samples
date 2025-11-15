# Documentation: Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h
---
## File Metadata
- **Path**: `Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h`
- **Filename**: `bindlessTexture.h`
- **Language**: h
- **Size**: 2514 bytes
- **Lines**: 67
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```h
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

#ifndef _BINDLESSTEXTURE_CU_
#define _BINDLESSTEXTURE_CU_

// includes, cuda
#include <cuda_runtime.h>
#include <vector_types.h>

// CUDA utilities and system includes
#include <helper_cuda.h>
#include <vector_types.h>

typedef unsigned int  uint;
typedef unsigned char uchar;

#pragma pack(push, 4)
struct Image
{
    void                *h_data;
    cudaExtent           size;
    cudaResourceType     type;
    cudaArray_t          dataArray;
    cudaMipmappedArray_t mipmapArray;
    cudaTextureObject_t  textureObject;

    Image() { memset(this, 0, sizeof(Image)); }
};
#pragma pack(pop)

inline void _checkHost(bool test, const char *condition, const char *file, int line, const char *func)
{
    if (!test) {
        fprintf(stderr, "HOST error at %s:%d (%s) \"%s\" \n", file, line, condition, func);
        exit(EXIT_FAILURE);
    }
}

#define checkHost(condition) _checkHost(condition, #condition, __FILE__, __LINE__, __FUNCTION__)

#endif

```

---
## High-Level Overview
This file is a h source file with 1 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda_runtime.h`
- `vector_types.h`
- `helper_cuda.h`
- `vector_types.h`

### Preprocessor Definitions
- **_BINDLESSTEXTURE_CU_**: `// includes, cuda`
- **checkHost**: ``

### Functions
#### `void _checkHost(bool test, const char *condition, const char *file, int line, const char *func)`
- Function in Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h

### Classes / Structures
#### `struct Image`
- Defined in Samples/3_CUDA_Features/bindlessTexture/bindlessTexture.h


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


---
## Performance & Security Notes
### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

