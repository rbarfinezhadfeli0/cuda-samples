# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp
---

**Total Keywords**: 27

---

## N

### NvMedia2DFillNvSciSyncAttrList {#nvmedia2dfillnvscisyncattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `ediaStatus status = NvMedia2DFillNvSciSyncAttrList(ctx->i2d, signalerA`

### NvMediaStatus {#nvmediastatus}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `terAttrList));

    NvMediaStatus status = NvMedia2DF`

### NvSciBufAttrKeyValuePair {#nvscibufattrkeyvaluepair}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `AILURE);
    }

    NvSciBufAttrKeyValuePair attr_gpuid[] = {NvS`

### NvSciBufAttrList {#nvscibufattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `vSciBufObj &bufobj, NvSciBufAttrList &nvmediaAttrlist, i`

### NvSciBufAttrListFree {#nvscibufattrlistfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufAttrListFree(conflictlist);
    `

### NvSciBufAttrListReconcileAndObjAlloc {#nvscibufattrlistreconcileandobjalloc}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciBufAttrListReconcileAndObjAlloc(bufUnreconciledAttr`

### NvSciBufAttrListSetAttrs {#nvscibufattrlistsetattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `vSciErrors(
        NvSciBufAttrListSetAttrs(nvmediaAttrlist, at`

### NvSciBufGeneralAttrKey_GpuId {#nvscibufgeneralattrkeygpuid}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `air attr_gpuid[] = {NvSciBufGeneralAttrKey_GpuId, &devUUID, sizeof(d`

### NvSciBufObj {#nvscibufobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `
void setupNvSciBuf(NvSciBufObj &bufobj, NvSciBufAt`

### NvSciBufObjFree {#nvscibufobjfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufObjFree(Bufobj);
    }
}

v`

### NvSciError {#nvscierror}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `          \
        NvSciError _status = call;    `

### NvSciError_Success {#nvscierrorsuccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `      \
        if (NvSciError_Success != _status) {      `

### NvSciSyncAttrList {#nvscisyncattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `ciSyncModule));
    NvSciSyncAttrList signalerAttrList, w`

### NvSciSyncAttrListCreate {#nvscisyncattrlistcreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncAttrListCreate(sciSyncModule, &sig`

### NvSciSyncAttrListFree {#nvscisyncattrlistfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `t, &syncObj));

    NvSciSyncAttrListFree(signalerAttrList);
`

### NvSciSyncAttrListReconcile {#nvscisyncattrlistreconcile}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncAttrListReconcile(syncUnreconciledLis`

### NvSciSyncModule {#nvscisyncmodule}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `cudaDeviceId)
{
    NvSciSyncModule sciSyncModule;
    `

### NvSciSyncModuleOpen {#nvscisyncmoduleopen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncModuleOpen(&sciSyncModule));
 `

### NvSciSyncObj {#nvscisyncobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `nc(Blit2DTest *ctx, NvSciSyncObj &syncObj, int cudaD`

### NvSciSyncObjAlloc {#nvscisyncobjalloc}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `   checkNvSciErrors(NvSciSyncObjAlloc(syncReconciledList,`

### NvSciSyncObjFree {#nvscisyncobjfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `&syncObj)
{
    if (NvSciSyncObjFree != NULL) {
        `


## C

### checkNvSciErrors {#checknvscierrors}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `#define checkNvSciErrors(call)                                   \
    do {                         `

### cleanupNvSciBuf {#cleanupnvscibuf}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `void cleanupNvSciBuf(NvSciBufObj &Bufobj)
{`

### cleanupNvSciSync {#cleanupnvscisync}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `void cleanupNvSciSync(NvSciSyncObj &syncObj)
{`


## S

### setupCudaSignalerNvSciSync {#setupcudasignalernvscisync}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `void setupCudaSignalerNvSciSync(Blit2DTest *ctx, NvSciSyncObj &syncObj, int cudaDeviceId)
{`

### setupNvMediaSignalerNvSciSync {#setupnvmediasignalernvscisync}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `void setupNvMediaSignalerNvSciSync(Blit2DTest *ctx, NvSciSyncObj &syncObj, int cudaDeviceId)
{`

### setupNvSciBuf {#setupnvscibuf}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvsci_setup.cpp](./nvsci_setup.cpp_docs.md)
- **Context**: `void setupNvSciBuf(NvSciBufObj &bufobj, NvSciBufAttrList &nvmediaAttrlist, int cudaDeviceId)
{`

