# Documentation: Samples/5_Domain_Specific/smokeParticles/nvMath.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/smokeParticles/nvMath.h`
- **Filename**: `nvMath.h`
- **Language**: h
- **Size**: 3081 bytes
- **Lines**: 90
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```h
/*
 * Copyright 1993-2015 NVIDIA Corporation.  All rights reserved.
 *
 * Please refer to the NVIDIA end user license agreement (EULA) associated
 * with this source code for terms and conditions that govern your use of
 * this software. Any use, reproduction, disclosure, or distribution of
 * this software and related documentation outside the terms of the EULA
 * is strictly prohibited.
 *
 */

//
// Template math library for common 3D functionality
//
// This code is in part deriver from glh, a cross platform glut helper library.
// The copyright for glh follows this notice.
//
// Copyright (c) NVIDIA Corporation. All rights reserved.
////////////////////////////////////////////////////////////////////////////////

/*
    Copyright (c) 2000 Cass Everitt
    Copyright (c) 2000 NVIDIA Corporation
    All rights reserved.

    Redistribution and use in source and binary forms, with or
    without modification, are permitted provided that the following
    conditions are met:

     * Redistributions of source code must retain the above
       copyright notice, this list of conditions and the following
       disclaimer.

     * Redistributions in binary form must reproduce the above
       copyright notice, this list of conditions and the following
       disclaimer in the documentation and/or other materials
       provided with the distribution.

     * The names of contributors to this software may not be used
       to endorse or promote products derived from this software
       without specific prior written permission.

       THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
       ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
       LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
       FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
       REGENTS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
       INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
       BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
       LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
       CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
       LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
       ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
       POSSIBILITY OF SUCH DAMAGE.


    Cass Everitt - cass@r3.nu
*/

#ifndef NV_MATH_H
#define NV_MATH_H

#include <math.h>
#include <nvMatrix.h>
#include <nvQuaternion.h>
#include <nvVector.h>

#define NV_PI float(3.1415926535897932384626433832795)

namespace nv {

typedef vec2<float>        vec2f;
typedef vec3<float>        vec3f;
typedef vec3<int>          vec3i;
typedef vec3<unsigned int> vec3ui;
typedef vec4<float>        vec4f;
typedef matrix4<float>     matrix4f;
typedef quaternion<float>  quaternionf;

inline void applyRotation(const quaternionf &r)
{
    float angle;
    vec3f axis;
    r.get_value(axis, angle);
    glRotatef(angle / 3.1415926f * 180.0f, axis[0], axis[1], axis[2]);
}
}; // namespace nv

#endif

```

---
## High-Level Overview
This file is a h source file with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `math.h`
- `nvMatrix.h`
- `nvQuaternion.h`
- `nvVector.h`

### Preprocessor Definitions
- **NV_MATH_H**: `#include <math.h>`
- **NV_PI**: `float(3.1415926535897932384626433832795)`

### Functions
#### `void applyRotation(const quaternionf &r)`
- Function in Samples/5_Domain_Specific/smokeParticles/nvMath.h


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

