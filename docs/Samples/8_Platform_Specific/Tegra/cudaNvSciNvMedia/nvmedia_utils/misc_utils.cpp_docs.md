# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/misc_utils.cpp
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/misc_utils.cpp`
- **Filename**: `misc_utils.cpp`
- **Language**: cpp
- **Size**: 2495 bytes
- **Lines**: 61
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```cpp
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

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <unistd.h>
#if defined(__QNX__)
#include <sys/time.h>
#endif
#include "misc_utils.h"

uint32_t u32(const uint8_t *ptr) { return ptr[0] | (ptr[1] << 8) | (ptr[2] << 16) | (ptr[3] << 24); }

NvMediaStatus GetTimeMicroSec(uint64_t *uTime)
{
    struct timespec t;
#if !(defined(CLOCK_MONOTONIC) && defined(_POSIX_MONOTONIC_CLOCK) && _POSIX_MONOTONIC_CLOCK >= 0 && _POSIX_TIMERS > 0)
    struct timeval tv;
#endif

    if (!uTime)
        return NVMEDIA_STATUS_BAD_PARAMETER;

#if !(defined(CLOCK_MONOTONIC) && defined(_POSIX_MONOTONIC_CLOCK) && _POSIX_MONOTONIC_CLOCK >= 0 && _POSIX_TIMERS > 0)
    gettimeofday(&tv, NULL);
    t.tv_sec  = tv.tv_sec;
    t.tv_nsec = tv.tv_usec * 1000L;
#else
    clock_gettime(CLOCK_MONOTONIC, &t);
#endif

    *uTime = (uint64_t)t.tv_sec * 1000000LL + (uint64_t)t.tv_nsec / 1000LL;
    return NVMEDIA_STATUS_OK;
}

```

---
## High-Level Overview
This file is a cpp source file with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 7 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `stdio.h`
- `stdlib.h`
- `string.h`
- `time.h`
- `unistd.h`
- `sys/time.h`
- `misc_utils.h`

### Functions
#### `uint32_t u32(const uint8_t *ptr)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/misc_utils.cpp

#### `NvMediaStatus GetTimeMicroSec(uint64_t *uTime)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/misc_utils.cpp


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

