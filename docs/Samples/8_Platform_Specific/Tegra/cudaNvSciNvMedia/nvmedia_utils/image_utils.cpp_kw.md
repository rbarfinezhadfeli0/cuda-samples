# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp
---

**Total Keywords**: 29

---

## A

### AllocateBufferToWriteImage {#allocatebuffertowriteimage}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus
AllocateBufferToWriteImage(Blit2DTest *ctx, NvMediaImage *image, NvMediaBool uvOrderFl`


## G

### GetBytesPerCompForPackedYUV {#getbytespercompforpackedyuv}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus GetBytesPerCompForPackedYUV(unsigned int surfBPCidx, unsigned int *bytespercomp)
{`

### GetSurfParams {#getsurfparams}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus GetSurfParams(unsigned int   surfaceType,
                                   float    `


## I

### ImgBytesPerPixelTable_Alpha {#imgbytesperpixeltablealpha}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `, 0};

unsigned int ImgBytesPerPixelTable_Alpha[][6] = {
    {1, 0,`

### ImgBytesPerPixelTable_RAW {#imgbytesperpixeltableraw}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `*/
};

unsigned int ImgBytesPerPixelTable_RAW[][6] = {
    {1, 0,`

### ImgBytesPerPixelTable_RG16 {#imgbytesperpixeltablerg16}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `*/
};

unsigned int ImgBytesPerPixelTable_RG16[6] = {4, 0, 0, 0, 0`

### ImgBytesPerPixelTable_RGBA {#imgbytesperpixeltablergba}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `,
};


unsigned int ImgBytesPerPixelTable_RGBA[][6] = {
    {4, 0,`

### ImgBytesPerPixelTable_RGBA16 {#imgbytesperpixeltablergba16}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `*/
};

unsigned int ImgBytesPerPixelTable_RGBA16[][6] = {
    {8, 0,`

### ImgBytesPerPixelTable_YUV {#imgbytesperpixeltableyuv}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `*/
};

unsigned int ImgBytesPerPixelTable_YUV[][9][6] = {{
      `

### ImgSurfParamsTable_Packed {#imgsurfparamstablepacked}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `

ImgUtilSurfParams ImgSurfParamsTable_Packed = {
    .heightFact`

### ImgSurfParamsTable_RAW {#imgsurfparamstableraw}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `

ImgUtilSurfParams ImgSurfParamsTable_RAW = {
    .heightFact`

### ImgSurfParamsTable_RGBA {#imgsurfparamstablergba}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `

ImgUtilSurfParams ImgSurfParamsTable_RGBA = {
    .heightFact`

### ImgSurfParamsTable_YUV {#imgsurfparamstableyuv}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `

ImgUtilSurfParams ImgSurfParamsTable_YUV[][4] = {
    {
    `

### ImgUtilSurfParams {#imgutilsurfparams}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: ` int numSurfaces;
} ImgUtilSurfParams;

ImgUtilSurfParams`

### InitImage {#initimage}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus InitImage(NvMediaImage *image, uint32_t width, uint32_t height)
{`


## M

### MAXM_NUM_SURFACES {#maxmnumsurfaces}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `#define MAXM_NUM_SURFACES 6

typedef struct
{
    float        heightFactor[6];
    float        wid`


## N

### NvMediaBool {#nvmediabool}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `vMediaImage *image, NvMediaBool uvOrderFlag, NvMedi`

### NvMediaImage {#nvmediaimage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `ge(Blit2DTest *ctx, NvMediaImage *image, NvMediaBool`

### NvMediaImageGetBits {#nvmediaimagegetbits}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `
    }
    status = NvMediaImageGetBits(image, NULL, (void `

### NvMediaImageLock {#nvmediaimagelock}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `    }

    status = NvMediaImageLock(image, NVMEDIA_IMAG`

### NvMediaImagePutBits {#nvmediaimageputbits}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `
    }
    status = NvMediaImagePutBits(image, NULL, (void `

### NvMediaImageSurfaceMap {#nvmediaimagesurfacemap}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `           = 1;
    NvMediaImageSurfaceMap surfaceMap;
    NvM`

### NvMediaImageUnlock {#nvmediaimageunlock}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `n status;
    }
    NvMediaImageUnlock(image);

    ctx->d`

### NvMediaStatus {#nvmediastatus}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `        }};

static NvMediaStatus GetBytesPerCompForP`

### NvMediaSurfaceFormatGetAttrs {#nvmediasurfaceformatgetattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: ` = 1;

    status = NvMediaSurfaceFormatGetAttrs(surfaceType, srcAtt`

### NvMediaVideoSurfaceGetBits {#nvmediavideosurfacegetbits}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `        printf("%s: NvMediaVideoSurfaceGetBits() failed \n", __fun`


## R

### ReadImage {#readimage}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus ReadImage(char         *fileName,
                        uint32_t      frameNum,
    `

### ReadImageNew {#readimagenew}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus ReadImageNew(char         *fileName,
                                  uint32_t      f`


## W

### WriteImageToAllocatedBuffer {#writeimagetoallocatedbuffer}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/image_utils.cpp](./image_utils.cpp_docs.md)
- **Context**: `NvMediaStatus WriteImageToAllocatedBuffer(Blit2DTest   *ctx,
                                       `

