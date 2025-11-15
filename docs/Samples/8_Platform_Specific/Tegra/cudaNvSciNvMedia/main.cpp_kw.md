# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp
---

**Total Keywords**: 22

---

## G

### GetTimeMicroSec {#gettimemicrosec}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `ces cudaResObj;
    GetTimeMicroSec(&startTime);
    se`


## N

### NvMedia {#nvmedia}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `   }

    // Create NvMedia device
    ctx.devi`

### NvMedia2DCreate {#nvmedia2dcreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `itter
    ctx.i2d = NvMedia2DCreate(ctx.device);
    if`

### NvMedia2DDestroy {#nvmedia2ddestroy}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvMedia2DDestroy(ctx->i2d);
    }

 `

### NvMedia2DGetVersion {#nvmedia2dgetversion}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `rsion;
    status = NvMedia2DGetVersion(&version);
    if (`

### NvMediaDeviceCreate {#nvmediadevicecreate}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `ce
    ctx.device = NvMediaDeviceCreate();
    if (!ctx.dev`

### NvMediaDeviceDestroy {#nvmediadevicedestroy}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: ` != NULL) {
        NvMediaDeviceDestroy(ctx->device);
    }`

### NvMediaStatus {#nvmediastatus}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `up(Blit2DTest *ctx, NvMediaStatus status)
{
    if (c`

### NvMediaVersion {#nvmediaversion}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `heck version */
    NvMediaVersion version;
    status`

### NvSCI {#nvsci}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: ` operations without NvSCI APIs starts
    cud`

### NvSciBufObj {#nvscibufobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `s.iterations);

    NvSciBufObj            dstNvSci`

### NvSciError {#nvscierror}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `          \
        NvSciError _status = call;    `

### NvSciError_Success {#nvscierrorsuccess}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `      \
        if (NvSciError_Success != _status) {      `

### NvSciSyncFence {#nvscisyncfence}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `A_STATUS_ERROR;
    NvSciSyncFence nvMediaSignalerFenc`

### NvSciSyncFenceInitializer {#nvscisyncfenceinitializer}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `ediaSignalerFence = NvSciSyncFenceInitializer;
    NvSciSyncFence`

### NvSciSyncObj {#nvscisyncobj}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `srcNvSciBufobj;
    NvSciSyncObj           nvMediaSi`


## P

### ParseArgs {#parseargs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `it2DTest));

    /* ParseArgs parses the command `

### PrintUsage {#printusage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `, &args)) {
        PrintUsage();
        return -`


## T

### TestArgs {#testargs}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `char *argv[])
{
    TestArgs       args;
    Bli`


## C

### checkNvSciErrors {#checknvscierrors}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `#define checkNvSciErrors(call)                                   \
    do {                         `

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `void cleanup(Blit2DTest *ctx, NvMediaStatus status)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/main.cpp](./main.cpp_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`

