# Keywords: Common/UtilNPP/ImageIO.h
---

**Total Keywords**: 23

---

## F

### FreeImage {#freeimage}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `esNPP.h"

#include "FreeImage.h"
#include "Except`

### FreeImageErrorHandler {#freeimageerrorhandler}

- **Type**: function
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `void
FreeImageErrorHandler(FREE_IMAGE_FORMAT oFif, const char *zMessage)
{`

### FreeImage_Allocate {#freeimageallocate}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `AP *pResultBitmap = FreeImage_Allocate(rImage.width(), rIm`

### FreeImage_FIFSupportsReading {#freeimagefifsupportsreading}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `itmap;

        if (FreeImage_FIFSupportsReading(eFormat))
        {`

### FreeImage_GetBPP {#freeimagegetbpp}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `
        NPP_ASSERT(FreeImage_GetBPP(pBitmap) == 8);

  `

### FreeImage_GetBits {#freeimagegetbits}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `t Npp8u *pSrcLine = FreeImage_GetBits(pBitmap) + nSrcPitc`

### FreeImage_GetColorType {#freeimagegetcolortype}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `
        NPP_ASSERT(FreeImage_GetColorType(pBitmap) == FIC_MIN`

### FreeImage_GetFIFFromFilename {#freeimagegetfiffromfilename}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `          eFormat = FreeImage_GetFIFFromFilename(rFileName.c_str());`

### FreeImage_GetFileType {#freeimagegetfiletype}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `GE_FORMAT eFormat = FreeImage_GetFileType(rFileName.c_str());`

### FreeImage_GetHeight {#freeimagegetheight}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `_GetWidth(pBitmap), FreeImage_GetHeight(pBitmap));

       `

### FreeImage_GetPitch {#freeimagegetpitch}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `ned int nSrcPitch = FreeImage_GetPitch(pBitmap);
        c`

### FreeImage_GetWidth {#freeimagegetwidth}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `ageCPU_8u_C1 oImage(FreeImage_GetWidth(pBitmap), FreeImage`

### FreeImage_Load {#freeimageload}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `          pBitmap = FreeImage_Load(eFormat, rFileName.`

### FreeImage_Save {#freeimagesave}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `
        bSuccess = FreeImage_Save(FIF_PGM, pResultBit`

### FreeImage_SetOutputMessage {#freeimagesetoutputmessage}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `ror handler
        FreeImage_SetOutputMessage(FreeImageErrorHandl`


## I

### ImageCPU {#imagecpu}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `       // create an ImageCPU to receive the load`

### ImageCPU_8u_C1 {#imagecpu8uc1}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `:string &rFileName, ImageCPU_8u_C1 &rImage)
    {
    `

### ImageNPP_8u_C1 {#imagenpp8uc1}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `:string &rFileName, ImageNPP_8u_C1 &rImage)
    {
    `

### ImagesCPU {#imagescpu}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `AGE_IO_H

#include "ImagesCPU.h"
#include "Images`

### ImagesNPP {#imagesnpp}

- **Type**: identifier
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `gesCPU.h"
#include "ImagesNPP.h"

#include "FreeI`


## N

### NV_UTIL_NPP_IMAGE_IO_H {#nvutilnppimageioh}

- **Type**: macro
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `#define NV_UTIL_NPP_IMAGE_IO_H

#include "ImagesCPU.h"
#include "ImagesNPP.h"

#include "FreeImage.h`


## L

### loadImage {#loadimage}

- **Type**: function
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `void
    loadImage(const std::string &rFileName, ImageNPP_8u_C1 &rImage)
    {`


## S

### saveImage {#saveimage}

- **Type**: function
- **File**: [Common/UtilNPP/ImageIO.h](./ImageIO.h_docs.md)
- **Context**: `void
    saveImage(const std::string &rFileName, const ImageNPP_8u_C1 &rImage)
    {`

