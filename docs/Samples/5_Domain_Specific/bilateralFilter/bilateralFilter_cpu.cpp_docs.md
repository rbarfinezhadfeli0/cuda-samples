# Documentation: Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp`
- **Filename**: `bilateralFilter_cpu.cpp`
- **Language**: cpp
- **Size**: 5189 bytes
- **Lines**: 161
- **Generated**: 2025-11-15 12:53:52 UTC

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

#include <math.h>
#include <string.h>

////////////////////////////////////////////////////////////////////////////////
// export C interface
#define EPSILON 1e-3
extern "C" void updateGaussianGold(float delta, int radius);
extern "C" void bilateralFilterGold(unsigned int *pSrc, unsigned int *pDest, float e_d, int w, int h, int r);
// variables
float gaussian[50];

struct float4
{
    float x;
    float y;
    float z;
    float w;

    float4() {};
    float4(float value) { x = y = z = w = value; }
};

void updateGaussianGold(float delta, int radius)
{
    for (int i = 0; i < 2 * radius + 1; i++) {
        int x       = i - radius;
        gaussian[i] = expf(-(x * x) / (2 * delta * delta));
    }
}

float heuclideanLen(float4 a, float4 b, float d)
{
    float mod =
        (b.x - a.x) * (b.x - a.x) + (b.y - a.y) * (b.y - a.y) + (b.z - a.z) * (b.z - a.z) + (b.w - a.w) * (b.w - a.w);

    return expf(-mod / (2 * d * d));
}

unsigned int hrgbaFloatToInt(float4 rgba)
{
    unsigned int w = (((unsigned int)(fabs(rgba.w) * 255.0f)) & 0xff) << 24;
    unsigned int z = (((unsigned int)(fabs(rgba.z) * 255.0f)) & 0xff) << 16;
    unsigned int y = (((unsigned int)(fabs(rgba.y) * 255.0f)) & 0xff) << 8;
    unsigned int x = ((unsigned int)(fabs(rgba.x) * 255.0f)) & 0xff;

    return (w | z | y | x);
}

float4 hrgbaIntToFloat(unsigned int c)
{
    float4 rgba;
    rgba.x = (c & 0xff) * 0.003921568627f;         //  /255.0f;
    rgba.y = ((c >> 8) & 0xff) * 0.003921568627f;  //  /255.0f;
    rgba.z = ((c >> 16) & 0xff) * 0.003921568627f; //  /255.0f;
    rgba.w = ((c >> 24) & 0xff) * 0.003921568627f; //  /255.0f;
    return rgba;
}

float4 mul(float a, float4 b)
{
    float4 ans;
    ans.x = a * b.x;
    ans.y = a * b.y;
    ans.z = a * b.z;
    ans.w = a * b.w;

    return ans;
}

float4 add4(float4 a, float4 b)
{
    float4 ans;
    ans.x = a.x + b.x;
    ans.y = a.y + b.y;
    ans.z = a.z + b.z;
    ans.w = a.w + b.w;

    return ans;
}

void bilateralFilterGold(unsigned int *pSrc, unsigned int *pDest, float e_d, int w, int h, int r)
{
    float4 *hImage = new float4[w * h];
    float   domainDist, colorDist, factor;

    for (int y = 0; y < h; y++) {
        for (int x = 0; x < w; x++) {
            hImage[y * w + x] = hrgbaIntToFloat(pSrc[y * w + x]);
        }
    }

    for (int y = 0; y < h; y++) {
        for (int x = 0; x < w; x++) {
            float4 t(0.0f);
            float  sum = 0.0f;

            for (int i = -r; i <= r; i++) {
                int neighborY = y + i;

                // clamp the neighbor pixel, prevent overflow
                if (neighborY < 0) {
                    neighborY = 0;
                }
                else if (neighborY >= h) {
                    neighborY = h - 1;
                }

                for (int j = -r; j <= r; j++) {
                    domainDist = gaussian[r + i] * gaussian[r + j];

                    // clamp the neighbor pixel, prevent overflow
                    int neighborX = x + j;

                    if (neighborX < 0) {
                        neighborX = 0;
                    }
                    else if (neighborX >= w) {
                        neighborX = w - 1;
                    }

                    colorDist = heuclideanLen(hImage[neighborY * w + neighborX], hImage[y * w + x], e_d);
                    factor    = domainDist * colorDist;
                    sum += factor;
                    t = add4(t, mul(factor, hImage[neighborY * w + neighborX]));
                }
            }

            pDest[y * w + x] = hrgbaFloatToInt(mul(1 / sum, t));
        }
    }

    delete[] hImage;
}

```

---
## High-Level Overview
This file is a cpp source file with 10 function(s) and 1 class/struct definition(s) in the CUDA Samples repository.

**Dependencies**: 2 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `math.h`
- `string.h`

### Preprocessor Definitions
- **EPSILON**: `1e-3`

### Functions
#### `void updateGaussianGold(float delta, int radius)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `float heuclideanLen(float4 a, float4 b, float d)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `int hrgbaFloatToInt(float4 rgba)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `float4 hrgbaIntToFloat(unsigned int c)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `float4 mul(float a, float4 b)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `float4 add4(float4 a, float4 b)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `void bilateralFilterGold(unsigned int *pSrc, unsigned int *pDest, float e_d, int w, int h, int r)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `overflow if(neighborY < 0)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `else if(neighborY >= h)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

#### `else if(neighborX >= w)`
- Function in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp

### Classes / Structures
#### `struct float4`
- Defined in Samples/5_Domain_Specific/bilateralFilter/bilateralFilter_cpu.cpp


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

