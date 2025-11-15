# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp
---

**Total Keywords**: 65

---

## A

### AccessPermSetAttr {#accesspermsetattr}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: AccessPermSetAttr failed. Error: %d \`

### AllocateBufferToWriteImage {#allocatebuffertowriteimage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `esPerPixel = 1;
    AllocateBufferToWriteImage(ctx,
              `


## I

### ImageCreatefromSciBuf {#imagecreatefromscibuf}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: ImageCreatefromSciBuf failed. Error: %d \`

### ImageFillSciBufAttrs {#imagefillscibufattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: ImageFillSciBufAttrs failed. Error: %d \`

### InitImage {#initimage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `US_ERROR;
    }
    InitImage(*image, surfAllocAt`


## N

### NvMedia {#nvmedia}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `
    }
}

// Create NvMedia src & dst image wit`

### NvMedia2Blit {#nvmedia2blit}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `unch inorder to for NvMedia2Blit to
    // wait
    `

### NvMedia2DBlitEx {#nvmedia2dblitex}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `age */
    status = NvMedia2DBlitEx(ctx->i2d,          `

### NvMedia2DGetEOFNvSciSyncFence {#nvmedia2dgeteofnvscisyncfence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `    }

    status = NvMedia2DGetEOFNvSciSyncFence(ctx->i2d, nvMediaSi`

### NvMedia2DImageRegister {#nvmedia2dimageregister}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `urface
    status = NvMedia2DImageRegister(ctx->i2d, ctx->srcI`

### NvMedia2DImageUnRegister {#nvmedia2dimageunregister}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: ` != NULL) {
        NvMedia2DImageUnRegister(ctx->i2d, ctx->srcI`

### NvMedia2DInsertPreNvSciSyncFence {#nvmedia2dinsertprenvscisyncfence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: ` {
        status = NvMedia2DInsertPreNvSciSyncFence(ctx->i2d, preSyncFe`

### NvMedia2DRegisterNvSciSyncObj {#nvmedia2dregisternvscisyncobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `    }

    status = NvMedia2DRegisterNvSciSyncObj(ctx->i2d, NVMEDIA_E`

### NvMedia2DSetNvSciSyncObjforEOF {#nvmedia2dsetnvscisyncobjforeof}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: NvMedia2DSetNvSciSyncObjforEOF   failed: %d\n", __`

### NvMedia2DUnregisterNvSciSyncObj {#nvmedia2dunregisternvscisyncobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `(ctx);
    status = NvMedia2DUnregisterNvSciSyncObj(ctx->i2d, syncObj);`

### NvMediaDevice {#nvmediadevice}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `CreateUsingNvScibuf(NvMediaDevice              *devic`

### NvMediaImage {#nvmediaimage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `de "nvsci_setup.h"

NvMediaImage *NvMediaImageCreate`

### NvMediaImageCreate {#nvmediaimagecreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `  /*    printf("%s: NvMediaImageCreate:: Image size: %ux%u`

### NvMediaImageCreateFromNvSciBuf {#nvmediaimagecreatefromnvscibuf}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `eId);

    status = NvMediaImageCreateFromNvSciBuf(device, bufobj, &im`

### NvMediaImageCreateNew {#nvmediaimagecreatenew}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `MAX);

    *image = NvMediaImageCreateNew(ctx->device, surfTy`

### NvMediaImageCreateUsingNvScibuf {#nvmediaimagecreateusingnvscibuf}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `p.h"

NvMediaImage *NvMediaImageCreateUsingNvScibuf(NvMediaDevice      `

### NvMediaImageDestroy {#nvmediaimagedestroy}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `(module);
    }
    NvMediaImageDestroy(image);
    return `

### NvMediaImageFillNvSciBufAttrs {#nvmediaimagefillnvscibufattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `    }

    status = NvMediaImageFillNvSciBufAttrs(device, type, attrs`

### NvMediaImageNvSciBufDeinit {#nvmediaimagenvscibufdeinit}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `FAILURE);
    }
    NvMediaImageNvSciBufDeinit();
}

void cleanupN`

### NvMediaImageNvSciBufInit {#nvmediaimagenvscibufinit}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `tatus;
    status = NvMediaImageNvSciBufInit();
    if (status !`

### NvMediaImageSciBufInit {#nvmediaimagescibufinit}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: NvMediaImageSciBufInit failed\n", __func__`

### NvMediaImageSurfaceMap {#nvmediaimagesurfacemap}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        status;
    NvMediaImageSurfaceMap surfaceMap;

    st`

### NvMediaStatus {#nvmediastatus}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `iError_Success;
    NvMediaStatus             status `

### NvMediaSurfAllocAttr {#nvmediasurfallocattr}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `              const NvMediaSurfAllocAttr *attrs,
           `

### NvMediaSurfFormatAttr {#nvmediasurfformatattr}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `                    NvMediaSurfFormatAttr *surfFormatAttrs,
 `

### NvMediaSurfaceFormatGetType {#nvmediasurfaceformatgettype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `e */
    surfType = NvMediaSurfaceFormatGetType(surfFormatAttrs, NV`

### NvMediaSurfaceType {#nvmediasurfacetype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `                    NvMediaSurfaceType          type,
    `

### NvSciBuf {#nvscibuf}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `& dst image without NvSciBuf
void setupNvMedia(T`

### NvSciBufAccessPerm_ReadWrite {#nvscibufaccesspermreadwrite}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `Perm access_perm  = NvSciBufAccessPerm_ReadWrite;
    NvSciBufAttrKe`

### NvSciBufAttrKeyValuePair {#nvscibufattrkeyvaluepair}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `Perm_ReadWrite;
    NvSciBufAttrKeyValuePair  attr_kvp     = {Nv`

### NvSciBufAttrList {#nvscibufattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `EDIA_STATUS_OK;
    NvSciBufAttrList          attrlist  `

### NvSciBufAttrListCreate {#nvscibufattrlistcreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `p;
    }

    err = NvSciBufAttrListCreate(module, &attrlist);`

### NvSciBufAttrListFree {#nvscibufattrlistfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `cleanup;
    }

    NvSciBufAttrListFree(attrlist);

    if `

### NvSciBufAttrListSetAttrs {#nvscibufattrlistsetattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `p;
    }

    err = NvSciBufAttrListSetAttrs(attrlist, &attr_kvp`

### NvSciBufAttrValAccessPerm {#nvscibufattrvalaccessperm}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `ictlist = NULL;
    NvSciBufAttrValAccessPerm access_perm  = NvSc`

### NvSciBufGeneralAttrKey_RequiredPerm {#nvscibufgeneralattrkeyrequiredperm}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `ir  attr_kvp     = {NvSciBufGeneralAttrKey_RequiredPerm, &access_perm, size`

### NvSciBufModule {#nvscibufmodule}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `cudaDeviceId)
{
    NvSciBufModule            module  `

### NvSciBufModuleClose {#nvscibufmoduleclose}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufModuleClose(module);
    }

   `

### NvSciBufModuleOpen {#nvscibufmoduleopen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: ` = NULL;

    err = NvSciBufModuleOpen(&module);
    if (e`

### NvSciBufObj {#nvscibufobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `                    NvSciBufObj                &buf`

### NvSciBufObjFree {#nvscibufobjfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufObjFree(bufobj);
        bu`

### NvSciBuffModuleOpen {#nvscibuffmoduleopen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: NvSciBuffModuleOpen failed. Error: %d \`

### NvSciError {#nvscierror}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `e       = NULL;
    NvSciError                err `

### NvSciError_Success {#nvscierrorsuccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `     err          = NvSciError_Success;
    NvMediaStatus `

### NvSciSyncFence {#nvscisyncfence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `                    NvSciSyncFence *preSyncFence,
    `

### NvSciSyncFenceClear {#nvscisyncfenceclear}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `;
        }
        NvSciSyncFenceClear(preSyncFence);
    `

### NvSciSyncObj {#nvscisyncobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `                    NvSciSyncObj   &nvMediaSignalerS`


## R

### ReadImage {#readimage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `eMap;

    status = ReadImage(args->inputFileName`


## S

### SciBufAttrListCreate {#scibufattrlistcreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `        printf("%s: SciBufAttrListCreate failed. Error: %d \`


## T

### TestArgs {#testargs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `                    TestArgs       *args,
      `


## W

### WriteImageToAllocatedBuffer {#writeimagetoallocatedbuffer}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `esPerPixel = 1;
    WriteImageToAllocatedBuffer(ctx, ctx->dstImage,`


## B

### blit2DImage {#blit2dimage}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `NvMediaStatus blit2DImage(Blit2DTest     *ctx,
                                 TestArgs       *args`

### blit2DImageNonNvSCI {#blit2dimagenonnvsci}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `NvMediaStatus blit2DImageNonNvSCI(Blit2DTest *ctx, TestArgs *args)
{`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `void cleanup(Blit2DTest *ctx, NvMediaStatus status = NVMEDIA_STATUS_OK)
{`

### cleanupNvMedia {#cleanupnvmedia}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `void cleanupNvMedia(Blit2DTest *ctx)
{`

### createSurface {#createsurface}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `NvMediaStatus createSurface(Blit2DTest            *ctx,
                                   NvMediaSu`

### createSurfaceNonNvSCI {#createsurfacenonnvsci}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `NvMediaStatus createSurfaceNonNvSCI(Blit2DTest            *ctx,
                                    `


## D

### destroySurface {#destroysurface}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `void destroySurface(NvMediaImage *image) {`


## R

### runNvMediaBlit2D {#runnvmediablit2d}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `void runNvMediaBlit2D(TestArgs       *args,
                      Blit2DTest     *ctx,
             `


## S

### setupNvMedia {#setupnvmedia}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp](./nvmedia_producer.cpp_docs.md)
- **Context**: `void setupNvMedia(TestArgs *args, Blit2DTest *ctx)
{`

