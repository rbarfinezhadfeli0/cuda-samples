# Keywords: Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp
---

**Total Keywords**: 72

---

## N

### NvSciBufAccessPerm_ReadWrite {#nvscibufaccesspermreadwrite}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `erm perm          = NvSciBufAccessPerm_ReadWrite;
        NvSciBufAt`

### NvSciBufAttrKeyValuePair {#nvscibufattrkeyvaluepair}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `AttrListOut[2];
    NvSciBufAttrKeyValuePair pairArrayOut[10];

`

### NvSciBufAttrList {#nvscibufattrlist}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ce   *m_fence;

    NvSciBufAttrList         m_rawBufAtt`

### NvSciBufAttrListCreate {#nvscibufattrlistcreate}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufAttrListCreate(m_bufModule, &m_raw`

### NvSciBufAttrListGetAttrs {#nvscibufattrlistgetattrs}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufAttrListGetAttrs(m_buffAttrListOut[0`

### NvSciBufAttrListReconcile {#nvscibufattrlistreconcile}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `vSciErrors(
        NvSciBufAttrListReconcile(rawBufUnreconciledL`

### NvSciBufAttrListSetAttrs {#nvscibufattrlistsetattrs}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufAttrListSetAttrs(
            m_rawB`

### NvSciBufAttrValAccessPerm {#nvscibufattrvalaccessperm}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   = false;
        NvSciBufAttrValAccessPerm perm          = NvS`

### NvSciBufAttrValColorFmt {#nvscibufattrvalcolorfmt}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `g = false;

        NvSciBufAttrValColorFmt      planecolorfmts`

### NvSciBufAttrValColorStd {#nvscibufattrvalcolorstd}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `_B8G8R8A8};
        NvSciBufAttrValColorStd      planecolorstds`

### NvSciBufAttrValImageLayoutType {#nvscibufattrvalimagelayouttype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `Type_Image;
        NvSciBufAttrValImageLayoutType layout  = NvSciBufI`

### NvSciBufAttrValImageScanType {#nvscibufattrvalimagescantype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `rStd_SRGB};
        NvSciBufAttrValImageScanType planescantype[]  = `

### NvSciBufGeneralAttrKey_GpuId {#nvscibufgeneralattrkeygpuid}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `erm)},
            {NvSciBufGeneralAttrKey_GpuId, &m_devUUID, sizeof`

### NvSciBufGeneralAttrKey_NeedCpuAccess {#nvscibufgeneralattrkeyneedcpuaccess}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ype)},
            {NvSciBufGeneralAttrKey_NeedCpuAccess, &cpuAccess, sizeof`

### NvSciBufGeneralAttrKey_RequiredPerm {#nvscibufgeneralattrkeyrequiredperm}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ess)},
            {NvSciBufGeneralAttrKey_RequiredPerm, &perm, sizeof(perm`

### NvSciBufGeneralAttrKey_Types {#nvscibufgeneralattrkeytypes}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ize)},
            {NvSciBufGeneralAttrKey_Types, &bufType, sizeof(b`

### NvSciBufImageAttrKey_Alignment {#nvscibufimageattrkeyalignment}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `irArrayOut[1].key = NvSciBufImageAttrKey_Alignment;
        pairArrayO`

### NvSciBufImageAttrKey_BottomPadding {#nvscibufimageattrkeybottompadding}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `pad)},
            {NvSciBufImageAttrKey_BottomPadding, &tbpad, sizeof(tbp`

### NvSciBufImageAttrKey_Layout {#nvscibufimageattrkeylayout}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `unt)},
            {NvSciBufImageAttrKey_Layout, &layout, sizeof(la`

### NvSciBufImageAttrKey_LeftPadding {#nvscibufimageattrkeyleftpadding}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `pad)},
            {NvSciBufImageAttrKey_LeftPadding, &lrpad, sizeof(lrp`

### NvSciBufImageAttrKey_PlaneColorFormat {#nvscibufimageattrkeyplanecolorformat}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `pad)},
            {NvSciBufImageAttrKey_PlaneColorFormat, planecolorfmts, si`

### NvSciBufImageAttrKey_PlaneColorStd {#nvscibufimageattrkeyplanecolorstd}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `mts)},
            {NvSciBufImageAttrKey_PlaneColorStd, planecolorstds, si`

### NvSciBufImageAttrKey_PlaneCount {#nvscibufimageattrkeyplanecount}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ype)},
            {NvSciBufImageAttrKey_PlaneCount, &planeCount, sizeo`

### NvSciBufImageAttrKey_PlaneHeight {#nvscibufimageattrkeyplaneheight}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ths)},
            {NvSciBufImageAttrKey_PlaneHeight, planeHeights, size`

### NvSciBufImageAttrKey_PlaneScanType {#nvscibufimageattrkeyplanescantype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `erm)},
            {NvSciBufImageAttrKey_PlaneScanType, planescantype, siz`

### NvSciBufImageAttrKey_PlaneWidth {#nvscibufimageattrkeyplanewidth}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `tds)},
            {NvSciBufImageAttrKey_PlaneWidth, planeWidths, sizeo`

### NvSciBufImageAttrKey_RightPadding {#nvscibufimageattrkeyrightpadding}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `pad)},
            {NvSciBufImageAttrKey_RightPadding, &lrpad, sizeof(lrp`

### NvSciBufImageAttrKey_Size {#nvscibufimageattrkeysize}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `irArrayOut[0].key = NvSciBufImageAttrKey_Size;
        pairArrayO`

### NvSciBufImageAttrKey_TopPadding {#nvscibufimageattrkeytoppadding}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `out)},
            {NvSciBufImageAttrKey_TopPadding, &tbpad, sizeof(tbp`

### NvSciBufImageObj {#nvscibufimageobj}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `    printf("created NvSciBufImageObj\n");
}

void cudaNv`

### NvSciBufImage_BlockLinearType {#nvscibufimageblocklineartype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `ayoutType layout  = NvSciBufImage_BlockLinearType;
        NvSciBufAt`

### NvSciBufModule {#nvscibufmodule}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `e m_syncModule;
    NvSciBufModule  m_bufModule;

    `

### NvSciBufModuleOpen {#nvscibufmoduleopen}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufModuleOpen(&buffModule));
    `

### NvSciBufObj {#nvscibufobj}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `daImportNvSciRawBuf(NvSciBufObj inputBufObj)
    {
`

### NvSciBufObjAlloc {#nvscibufobjalloc}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufObjAlloc(rawBufReconciledLis`

### NvSciBufObjGetAttrList {#nvscibufobjgetattrlist}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufObjGetAttrList(inputBufObj, &m_buf`

### NvSciBufRawBufferAttrKey_Size {#nvscibufrawbufferattrkeysize}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `[] = {
            {NvSciBufRawBufferAttrKey_Size, &size, sizeof(size`

### NvSciBufScan_InterlaceType {#nvscibufscaninterlacetype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `planescantype[]  = {NvSciBufScan_InterlaceType};

        NvSciBuf`

### NvSciBufType {#nvscibuftype}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `size)
    {
        NvSciBufType              bufTyp`

### NvSciBufType_Image {#nvscibuftypeimage}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `          bufType = NvSciBufType_Image;
        NvSciBufAt`

### NvSciBufType_RawBuffer {#nvscibuftyperawbuffer}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `    bufType       = NvSciBufType_RawBuffer;
        bool      `

### NvSciColorStd_SRGB {#nvscicolorstdsrgb}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `planecolorstds[] = {NvSciColorStd_SRGB};
        NvSciBufA`

### NvSciColor_B8G8R8A8 {#nvscicolorb8g8r8a8}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `planecolorfmts[] = {NvSciColor_B8G8R8A8};
        NvSciBufA`

### NvSciIpc {#nvsciipc}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `cations need to use NvSciIpc
        // and NvSc`

### NvSciSync {#nvscisync}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `iIpc
        // and NvSciSync[Export|Import] util`

### NvSciSyncAttrList {#nvscisyncattrlist}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `  m_bufModule;

    NvSciSyncAttrList m_syncAttrList;
   `

### NvSciSyncAttrListCreate {#nvscisyncattrlistcreate}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncAttrListCreate(m_syncModule, &m_sy`

### NvSciSyncAttrListReconcile {#nvscisyncattrlistreconcile}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `vSciErrors(
        NvSciSyncAttrListReconcile(syncUnreconciledLis`

### NvSciSyncFence {#nvscisyncfence}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `m_syncAttrList;
    NvSciSyncFence   *m_fence;

    Nv`

### NvSciSyncModule {#nvscisyncmodule}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `gnal
{
private:
    NvSciSyncModule m_syncModule;
    N`

### NvSciSyncModuleOpen {#nvscisyncmoduleopen}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncModuleOpen(&syncModule));
    `

### NvSciSyncObj {#nvscisyncobj}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `mportNvSciSemaphore(NvSciSyncObj syncObj)
    {
    `

### NvSciSyncObjAlloc {#nvscisyncobjalloc}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncObjAlloc(syncReconciledList,`


## C

### copyDataToImageArray {#copydatatoimagearray}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void copyDataToImageArray(unsigned char *imageData)
    {`

### createTexture {#createtexture}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void createTexture()
    {`

### cudaImportNvSciImage {#cudaimportnvsciimage}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void cudaImportNvSciImage(NvSciBufObj inputBufObj)
    {`

### cudaImportNvSciRawBuf {#cudaimportnvscirawbuf}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void cudaImportNvSciRawBuf(NvSciBufObj inputBufObj)
    {`

### cudaImportNvSciSemaphore {#cudaimportnvscisemaphore}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void cudaImportNvSciSemaphore(NvSciSyncObj syncObj)
    {`

### cudaNvSciSignal {#cudanvscisignal}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `class cudaNvSciSignal`

### cudaNvSciWait {#cudanvsciwait}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `class cudaNvSciWait`


## G

### getNvSciImageBufAttrList {#getnvsciimagebufattrlist}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `NvSciBufAttrList getNvSciImageBufAttrList() {`

### getNvSciRawBufAttrList {#getnvscirawbufattrlist}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `NvSciBufAttrList getNvSciRawBufAttrList() {`

### getNvSciSyncAttrList {#getnvscisyncattrlist}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `NvSciSyncAttrList getNvSciSyncAttrList() {`


## I

### initCuda {#initcuda}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void initCuda()
    {`


## R

### runImageGrayscale {#runimagegrayscale}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void runImageGrayscale(std::string image_filename, size_t imageWidth, size_t imageHeight)
    {`

### runRotateImageAndSignal {#runrotateimageandsignal}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void runRotateImageAndSignal(unsigned char *imageData)
    {`


## S

### setImageBufAttrList {#setimagebufattrlist}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void setImageBufAttrList(uint32_t width, uint32_t height)
    {`

### setRawBufAttrList {#setrawbufattrlist}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void setRawBufAttrList(uint64_t size)
    {`

### signalExternalSemaphore {#signalexternalsemaphore}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void signalExternalSemaphore()
    {`


## T

### thread_rotateAndSignal {#threadrotateandsignal}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void thread_rotateAndSignal(cudaNvSciSignal *cudaNvSciSignalObj, unsigned char *imageData)
{`

### thread_waitAndGrayscale {#threadwaitandgrayscale}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void thread_waitAndGrayscale(cudaNvSciWait *cudaNvSciWaitObj,
                             std::stri`


## W

### waitExternalSemaphore {#waitexternalsemaphore}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/cudaNvSci/cudaNvSci.cpp](./cudaNvSci.cpp_docs.md)
- **Context**: `void waitExternalSemaphore()
    {`

