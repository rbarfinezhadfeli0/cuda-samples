# Keywords: Samples/2_Concepts_and_Techniques/scan/scan.cu
---

**Total Keywords**: 14

---

## T

### THREADBLOCK_SIZE {#threadblocksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `#define THREADBLOCK_SIZE 256

//////////////////////////////////////////////////////////////////////`


## C

### closeScan {#closescan}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `void closeScan(void) {`


## F

### factorRadix2 {#factorradix2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `uint factorRadix2(uint &log2L, uint L)
{`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `uint iDivUp(uint dividend, uint divisor)
{`

### initScan {#initscan}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `void initScan(void)
{`


## S

### scan1Exclusive {#scan1exclusive}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `uint scan1Exclusive(uint idata, volatile uint *s_Data, uint size, cg::thread_block cta)
{`

### scan1Inclusive {#scan1inclusive}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `uint scan1Inclusive(uint idata, volatile uint *s_Data, uint size, cg::thread_block cta)
{`

### scan4Exclusive {#scan4exclusive}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `uint4 scan4Exclusive(uint4 idata4, volatile uint *s_Data, uint size, cg::thread_block cta)
{`

### scan4Inclusive {#scan4inclusive}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `uint4 scan4Inclusive(uint4 idata4, volatile uint *s_Data, uint size, cg::thread_block cta)
{`

### scanExclusiveLarge {#scanexclusivelarge}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `size_t scanExclusiveLarge(uint *d_Dst, uint *d_Src, uint batchSize, uint arrayLength)
{`

### scanExclusiveShared {#scanexclusiveshared}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `__global__ void scanExclusiveShared(`

### scanExclusiveShared2 {#scanexclusiveshared2}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `__global__ void scanExclusiveShared2(`

### scanExclusiveShort {#scanexclusiveshort}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `size_t scanExclusiveShort(uint *d_Dst, uint *d_Src, uint batchSize, uint arrayLength)
{`


## U

### uniformUpdate {#uniformupdate}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/scan/scan.cu](./scan.cu_docs.md)
- **Context**: `__global__ void uniformUpdate(`

