# Keywords: Samples/7_libNVVM/common/include/DDSWriter.h
---

**Total Keywords**: 16

---

## D

### DDPF_ALPHAPIXELS {#ddpfalphapixels}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDPF_ALPHAPIXELS 0x1
#define DDPF_RGB         0x40

#define DDSD_CAPS        0x1
#define DDS`

### DDPF_RGB {#ddpfrgb}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDPF_RGB         0x40

#define DDSD_CAPS        0x1
#define DDSD_HEIGHT      0x2
#define DDS`

### DDSCAPS_TEXTURE {#ddscapstexture}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDSCAPS_TEXTURE 0x1000


/// WriteDDS - Writes image data to a .dds file.  The data is expec`

### DDSD_CAPS {#ddsdcaps}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDSD_CAPS        0x1
#define DDSD_HEIGHT      0x2
#define DDSD_WIDTH       0x4
#define DDSD_`

### DDSD_HEIGHT {#ddsdheight}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDSD_HEIGHT      0x2
#define DDSD_WIDTH       0x4
#define DDSD_PIXELFORMAT 0x1000

#define D`

### DDSD_PIXELFORMAT {#ddsdpixelformat}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDSD_PIXELFORMAT 0x1000

#define DDSCAPS_TEXTURE 0x1000


/// WriteDDS - Writes image data t`

### DDSD_WIDTH {#ddsdwidth}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDSD_WIDTH       0x4
#define DDSD_PIXELFORMAT 0x1000

#define DDSCAPS_TEXTURE 0x1000


/// W`

### DDSHeader {#ddsheader}

- **Type**: type
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `struct DDSHeader`

### DDSPixelFormat {#ddspixelformat}

- **Type**: type
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `struct DDSPixelFormat`

### DDSWRITER_H {#ddswriterh}

- **Type**: macro
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `#define DDSWRITER_H

#include <fstream>

typedef int DWORD;

/// DDS File Structures
struct DDSPixel`


## F

### FourCC {#fourcc}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `RD Flags;
    DWORD FourCC;
    DWORD RGBBitCo`


## M

### MipMapCount {#mipmapcount}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `
    DWORD          MipMapCount;
    DWORD         `


## P

### PitchOrLinearSize {#pitchorlinearsize}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `
    DWORD          PitchOrLinearSize;
    DWORD         `

### PixelFormat {#pixelformat}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `
    DDSPixelFormat PixelFormat;
    DWORD         `


## W

### WriteDDS {#writedds}

- **Type**: identifier
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `EXTURE 0x1000


/// WriteDDS - Writes image data`

### writeDDS {#writedds}

- **Type**: function
- **File**: [Samples/7_libNVVM/common/include/DDSWriter.h](./DDSWriter.h_docs.md)
- **Context**: `void writeDDS(const char *filename, const float *data, unsigned width, unsigned height)
{`

