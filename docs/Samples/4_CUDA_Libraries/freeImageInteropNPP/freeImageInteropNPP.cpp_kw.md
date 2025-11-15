# Keywords: Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp
---

**Total Keywords**: 23

---

## F

### FreeImage {#freeimage}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `nstrates how to use FreeImage library with NPP.
/`

### FreeImageErrorHandler {#freeimageerrorhandler}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `void FreeImageErrorHandler(FREE_IMAGE_FORMAT oFif, const char *zMessage) {`

### FreeImage_Allocate {#freeimageallocate}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `AP *pResultBitmap = FreeImage_Allocate(oSizeROI.width, oSi`

### FreeImage_FIFSupportsReading {#freeimagefifsupportsreading}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `itmap;

        if (FreeImage_FIFSupportsReading(eFormat)) {
       `

### FreeImage_GetBPP {#freeimagegetbpp}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: ` int nBPP         = FreeImage_GetBPP(const_cast<FIBITMAP`

### FreeImage_GetBits {#freeimagegetbits}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `har *pSrcData     = FreeImage_GetBits(pBitmap);

        `

### FreeImage_GetColorType {#freeimagegetcolortype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `_COLOR_TYPE eType = FreeImage_GetColorType(const_cast<FIBITMAP`

### FreeImage_GetFIFFromFilename {#freeimagegetfiffromfilename}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `          eFormat = FreeImage_GetFIFFromFilename(sFilename.c_str());`

### FreeImage_GetFileType {#freeimagegetfiletype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `GE_FORMAT eFormat = FreeImage_GetFileType(sFilename.c_str());`

### FreeImage_GetHeight {#freeimagegetheight}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: ` int nImageHeight = FreeImage_GetHeight(const_cast<FIBITMAP`

### FreeImage_GetPitch {#freeimagegetpitch}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: ` int nPitch       = FreeImage_GetPitch(const_cast<FIBITMAP`

### FreeImage_GetWidth {#freeimagegetwidth}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: ` int nImageWidth  = FreeImage_GetWidth(const_cast<FIBITMAP`

### FreeImage_Load {#freeimageload}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `          pBitmap = FreeImage_Load(eFormat, sFilename.`

### FreeImage_Save {#freeimagesave}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `
        bSuccess = FreeImage_Save(FIF_PGM, pResultBit`

### FreeImage_SetOutputMessage {#freeimagesetoutputmessage}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `ror handler
        FreeImage_SetOutputMessage(FreeImageErrorHandl`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#endif

// Common Helpers

#include "Exceptions.h"
#include "F`

### NppLibraryVersion {#npplibraryversion}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `   }

        const NppLibraryVersion *libVer = nppGetLib`

### NppStreamContext {#nppstreamcontext}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: ` **)argv);

        NppStreamContext nppStreamCtx;
     `

### NppiPoint {#nppipoint}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `, 7};
        const NppiPoint oMaskAchnor = {0, 0`

### NppiSize {#nppisize}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `ilter
        const NppiSize  oMaskSize   = {7, `


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

// Common Helpers

#incl`


## C

### cudaDeviceInit {#cudadeviceinit}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `int cudaDeviceInit(int argc, const char **argv)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp](./freeImageInteropNPP.cpp_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`

