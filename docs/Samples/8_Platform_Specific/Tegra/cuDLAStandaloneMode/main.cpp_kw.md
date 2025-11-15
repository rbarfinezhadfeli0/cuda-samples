# Keywords: Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp
---

**Total Keywords**: 67

---

## C

### CudlaFence {#cudlafence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `tputTensorDesc;
    CudlaFence                  *p`


## D

### DPRINTF {#dprintf}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `#define DPRINTF(...) printf(__VA_ARGS__)

static void printTensorDesc(cudlaModuleTensorDescriptor *t`


## N

### NvSciBuf {#nvscibuf}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `("Error in creating NvSciBuf attribute list\n");`

### NvSciBufAccessPerm_ReadWrite {#nvscibufaccesspermreadwrite}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `erm perm          = NvSciBufAccessPerm_ReadWrite;
    uint32_t      `

### NvSciBufAttrKeyValuePair {#nvscibufattrkeyvaluepair}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `drAlign = 512;

    NvSciBufAttrKeyValuePair setAttrs[] = {
    `

### NvSciBufAttrList {#nvscibufattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `     bufModule;
    NvSciBufAttrList             inputAt`

### NvSciBufAttrListCreate {#nvscibufattrlistcreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `s;

    sciStatus = NvSciBufAttrListCreate(module, attrList);
`

### NvSciBufAttrListFree {#nvscibufattrlistfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufAttrListFree(resourceList->recon`

### NvSciBufAttrListReconcile {#nvscibufattrlistreconcile}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `st;

    sciError = NvSciBufAttrListReconcile(&inputAttrList, 1, `

### NvSciBufAttrListSetAttrs {#nvscibufattrlistsetattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `);

    sciStatus = NvSciBufAttrListSetAttrs(*attrList, setAttrs`

### NvSciBufAttrValAccessPerm {#nvscibufattrvalaccessperm}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `uAccess = true;
    NvSciBufAttrValAccessPerm perm          = NvS`

### NvSciBufGeneralAttrKey_NeedCpuAccess {#nvscibufgeneralattrkeyneedcpuaccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufGeneralAttrKey_NeedCpuAccess, .value = &needCpuA`

### NvSciBufGeneralAttrKey_RequiredPerm {#nvscibufgeneralattrkeyrequiredperm}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufGeneralAttrKey_RequiredPerm, .value = &perm, .l`

### NvSciBufGeneralAttrKey_Types {#nvscibufgeneralattrkeytypes}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `= {
        {.key = NvSciBufGeneralAttrKey_Types, .value = &type, .l`

### NvSciBufModule {#nvscibufmodule}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `  outputBufObj;
    NvSciBufModule               bufMo`

### NvSciBufModuleClose {#nvscibufmoduleclose}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufModuleClose(resourceList->bufMo`

### NvSciBufModuleOpen {#nvscibufmoduleopen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `ss;

    sciError = NvSciBufModuleOpen(&bufModule);
    if`

### NvSciBufObj {#nvscibufobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` *outputBuffer;
    NvSciBufObj                  in`

### NvSciBufObjAlloc {#nvscibufobjalloc}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `Obj;
    sciError = NvSciBufObjAlloc(reconciledInputAttr`

### NvSciBufObjFree {#nvscibufobjfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciBufObjFree(resourceList->input`

### NvSciBufObjGetCpuPtr {#nvscibufobjgetcpuptr}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `  }

    sciError = NvSciBufObjGetCpuPtr(inputBufObj, &input`

### NvSciBufTensorAttrKey_AlignmentPerDim {#nvscibuftensorattrkeyalignmentperdim}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufTensorAttrKey_AlignmentPerDim, .value = &alignmen`

### NvSciBufTensorAttrKey_BaseAddrAlign {#nvscibuftensorattrkeybaseaddralign}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufTensorAttrKey_BaseAddrAlign, .value = &baseAddr`

### NvSciBufTensorAttrKey_DataType {#nvscibuftensorattrkeydatatype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufTensorAttrKey_DataType, .value = &dataType`

### NvSciBufTensorAttrKey_NumDims {#nvscibuftensorattrkeynumdims}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufTensorAttrKey_NumDims, .value = &dimcount`

### NvSciBufTensorAttrKey_SizePerDim {#nvscibuftensorattrkeysizeperdim}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `)},
        {.key = NvSciBufTensorAttrKey_SizePerDim, .value = &sizes, .`

### NvSciBufType {#nvscibuftype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `iDataType_Int8;
    NvSciBufType              type  `

### NvSciBufType_Tensor {#nvscibuftypetensor}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `    type          = NvSciBufType_Tensor;
    uint64_t      `

### NvSciDataType_Int8 {#nvscidatatypeint8}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `    dataType      = NvSciDataType_Int8;
    NvSciBufType  `

### NvSciError {#nvscierror}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `= cudlaSuccess;
    NvSciError  sciStatus = NvSciE`

### NvSciError_Success {#nvscierrorsuccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `iError  sciStatus = NvSciError_Success;

    sciStatus = N`

### NvSciSync {#nvscisync}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `("Error in creating NvSciSync attribute list\n");`

### NvSciSyncAccessPerm {#nvscisyncaccessperm}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `(cpuSignaler);

    NvSciSyncAccessPerm cpuPerm = NvSciSync`

### NvSciSyncAccessPerm_SignalOnly {#nvscisyncaccesspermsignalonly}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `ccessPerm cpuPerm = NvSciSyncAccessPerm_SignalOnly;
    keyValue[1].at`

### NvSciSyncAccessPerm_WaitOnly {#nvscisyncaccesspermwaitonly}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `ccessPerm cpuPerm = NvSciSyncAccessPerm_WaitOnly;
    keyValue[1].at`

### NvSciSyncAttrKeyValuePair {#nvscisyncattrkeyvaluepair}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `ignaler = true;
    NvSciSyncAttrKeyValuePair keyValue[2];
    me`

### NvSciSyncAttrKey_NeedCpuAccess {#nvscisyncattrkeyneedcpuaccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `yValue[0].attrKey = NvSciSyncAttrKey_NeedCpuAccess;
    keyValue[0].va`

### NvSciSyncAttrKey_RequiredPerm {#nvscisyncattrkeyrequiredperm}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `].attrKey         = NvSciSyncAttrKey_RequiredPerm;
    keyValue[1].va`

### NvSciSyncAttrList {#nvscisyncattrlist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `      nvSciCtx;
    NvSciSyncAttrList            waiterAt`

### NvSciSyncAttrListCreate {#nvscisyncattrlistcreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `le;

    sciError = NvSciSyncAttrListCreate(syncModule, &signal`

### NvSciSyncAttrListFree {#nvscisyncattrlistfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciSyncAttrListFree(resourceList->nvSci`

### NvSciSyncAttrListReconcile {#nvscisyncattrlistreconcile}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` sciError =
        NvSciSyncAttrListReconcile(syncAttrListObj1, 2`

### NvSciSyncAttrListSetAttrs {#nvscisyncattrlistsetattrs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `uPerm);

    return NvSciSyncAttrListSetAttrs(list, keyValue, 2);`

### NvSciSyncCpuWaitContext {#nvscisynccpuwaitcontext}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `      eofFence;
    NvSciSyncCpuWaitContext      nvSciCtx;
    `

### NvSciSyncCpuWaitContextAlloc {#nvscisynccpuwaitcontextalloc}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `j1;

    sciError = NvSciSyncCpuWaitContextAlloc(syncModule, &nvSciC`

### NvSciSyncCpuWaitContextFree {#nvscisynccpuwaitcontextfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciSyncCpuWaitContextFree(resourceList->nvSci`

### NvSciSyncFence {#nvscisyncfence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `    syncModule;
    NvSciSyncFence               preFe`

### NvSciSyncFenceClear {#nvscisyncfenceclear}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` = NULL;
    }

    NvSciSyncFenceClear(&(resourceList->pre`

### NvSciSyncFenceInitializer {#nvscisyncfenceinitializer}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `yncFence preFence = NvSciSyncFenceInitializer;
    sciError      `

### NvSciSyncFenceWait {#nvscisyncfencewait}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `CPU.
    sciError = NvSciSyncFenceWait(reinterpret_cast<Nv`

### NvSciSyncModule {#nvscisyncmodule}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `      syncObj2;
    NvSciSyncModule              syncMo`

### NvSciSyncModuleClose {#nvscisyncmoduleclose}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciSyncModuleClose(resourceList->syncM`

### NvSciSyncModuleOpen {#nvscisyncmoduleopen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `j2;

    sciError = NvSciSyncModuleOpen(&syncModule);
    i`

### NvSciSyncObj {#nvscisyncobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `utConflictList;
    NvSciSyncObj                 syn`

### NvSciSyncObjAlloc {#nvscisyncobjalloc}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `j1;

    sciError = NvSciSyncObjAlloc(nvSciSyncReconciled`

### NvSciSyncObjFree {#nvscisyncobjfree}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvSciSyncObjFree(resourceList->syncO`

### NvSciSyncObjGenerateFence {#nvscisyncobjgeneratefence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `or                = NvSciSyncObjGenerateFence(syncObj1, &preFence`

### NvSciSyncObjSignal {#nvscisyncobjsignal}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `nal wait events
    NvSciSyncObjSignal(syncObj1);
    DPRI`


## R

### ResourceList {#resourcelist}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `   *signalEvents;
} ResourceList;

void cleanUp(Reso`


## C

### cleanUp {#cleanup}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `void cleanUp(ResourceList *resourceList)
{`

### createAndSetAttrList {#createandsetattrlist}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `cudlaStatus createAndSetAttrList(NvSciBufModule module, uint64_t bufSize, NvSciBufAttrList *attrList`


## F

### fillCpuSignalerAttrList {#fillcpusignalerattrlist}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `NvSciError fillCpuSignalerAttrList(NvSciSyncAttrList list)
{`

### fillCpuWaiterAttrList {#fillcpuwaiterattrlist}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `NvSciError fillCpuWaiterAttrList(NvSciSyncAttrList list)
{`


## I

### initializeInputBuffers {#initializeinputbuffers}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `int initializeInputBuffers(char *filePath, cudlaModuleTensorDescriptor *tensorDesc, unsigned char *b`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## P

### printTensorDesc {#printtensordesc}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `void printTensorDesc(cudlaModuleTensorDescriptor *tensorDesc)
{`


## S

### stat {#stat}

- **Type**: type
- **File**: [Samples/8_Platform_Specific/Tegra/cuDLAStandaloneMode/main.cpp](./main.cpp_docs.md)
- **Context**: `struct stat`

