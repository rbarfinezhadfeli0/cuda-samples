# Documentation: Samples/5_Domain_Specific/dxtc/dds.h
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/dxtc/dds.h`
- **Filename**: `dds.h`
- **Language**: h
- **Size**: 2959 bytes
- **Lines**: 87
- **Generated**: 2025-11-15 12:53:51 UTC

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

#ifndef DDS_H
#define DDS_H

#if !defined(MAKEFOURCC)
#define MAKEFOURCC(ch0, ch1, ch2, ch3) \
    ((unsigned int)(ch0) | ((unsigned int)(ch1) << 8) | ((unsigned int)(ch2) << 16) | ((unsigned int)(ch3) << 24))
#endif

typedef unsigned int   uint;
typedef unsigned short ushort;

struct DDSPixelFormat
{
    uint size;
    uint flags;
    uint fourcc;
    uint bitcount;
    uint rmask;
    uint gmask;
    uint bmask;
    uint amask;
};

struct DDSCaps
{
    uint caps1;
    uint caps2;
    uint caps3;
    uint caps4;
};

/// DDS file header.
struct DDSHeader
{
    uint           fourcc;
    uint           size;
    uint           flags;
    uint           height;
    uint           width;
    uint           pitch;
    uint           depth;
    uint           mipmapcount;
    uint           reserved[11];
    DDSPixelFormat pf;
    DDSCaps        caps;
    uint           notused;
};

static const uint FOURCC_DDS       = MAKEFOURCC('D', 'D', 'S', ' ');
static const uint FOURCC_DXT1      = MAKEFOURCC('D', 'X', 'T', '1');
static const uint DDSD_WIDTH       = 0x00000004U;
static const uint DDSD_HEIGHT      = 0x00000002U;
static const uint DDSD_CAPS        = 0x00000001U;
static const uint DDSD_PIXELFORMAT = 0x00001000U;
static const uint DDSCAPS_TEXTURE  = 0x00001000U;
static const uint DDPF_FOURCC      = 0x00000004U;
static const uint DDSD_LINEARSIZE  = 0x00080000U;

#endif // DDS_H

```

---
## High-Level Overview
This file is a h source file and 3 class/struct definition(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **DDS_H**: `#if !defined(MAKEFOURCC)`
- **MAKEFOURCC**: ``

### Classes / Structures
#### `struct DDSPixelFormat`
- Defined in Samples/5_Domain_Specific/dxtc/dds.h

#### `struct DDSCaps`
- Defined in Samples/5_Domain_Specific/dxtc/dds.h

#### `struct DDSHeader`
- Defined in Samples/5_Domain_Specific/dxtc/dds.h


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

