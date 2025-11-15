# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/main.cpp
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/main.cpp`
- **Filename**: `main.cpp`
- **Language**: cpp
- **Size**: 3085 bytes
- **Lines**: 73
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```cpp
/* Copyright (c) 2024, NVIDIA CORPORATION. All rights reserved.
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
#include <cuda.h>
#include <helper_image.h>
#include <vector>

#include "cudaNvSciBufMultiplanar.h"

#define MAX_FILE_SIZE 100

int main(int argc, const char **argv)
{
    int              numOfGPUs = 0;
    std::vector<int> deviceIds;
    (cudaGetDeviceCount(&numOfGPUs));

    printf("%d GPUs found\n", numOfGPUs);
    if (!numOfGPUs) {
        exit(EXIT_WAIVED);
    }
    else {
        for (int devID = 0; devID < numOfGPUs; devID++) {
            int major = 0, minor = 0;
            (cudaDeviceGetAttribute(&major, cudaDevAttrComputeCapabilityMajor, devID));
            (cudaDeviceGetAttribute(&minor, cudaDevAttrComputeCapabilityMinor, devID));
            if (major >= 6) {
                deviceIds.push_back(devID);
            }
        }
        if (deviceIds.size() == 0) {
            printf("cudaNvSciBufMultiplanar requires one or more GPUs of Pascal(SM 6.0) or higher "
                   "archs\nWaiving..\n");
            exit(EXIT_WAIVED);
        }
    }

    std::string image_filename     = sdkFindFilePath("yuv_planar_img1.yuv", argv[0]);
    std::string image_filename_out = "image_out.yuv";
    uint32_t    imageWidth         = 720;
    uint32_t    imageHeight        = 480;

    printf("input image %s , width = %d, height = %d\n", image_filename.c_str(), imageWidth, imageHeight);

    cudaNvSciBufMultiplanar cudaNvSciBufMultiplanarApp(imageWidth, imageHeight, deviceIds);
    cudaNvSciBufMultiplanarApp.runCudaNvSciBufPlanar(image_filename, image_filename_out);

    return EXIT_SUCCESS;
}

```

---
## High-Level Overview
This file is a cpp source file with 1 function(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `cuda.h`
- `helper_image.h`
- `vector`
- `cudaNvSciBufMultiplanar.h`

### Preprocessor Definitions
- **MAX_FILE_SIZE**: `100`

### Functions
#### `int main(int argc, const char **argv)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciBufMultiplanar/main.cpp


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

