# Keywords: Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu
---

**Total Keywords**: 14

---

## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `#define MAX(a, b) (a > b ? a : b)
#endif

static const char *sSDKsample = "[simpleVoteIntrinsics]\0"`


## V

### VOTE_DATA_GROUP {#votedatagroup}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `#define VOTE_DATA_GROUP 4

/////////////////////////////////////////////////////////////////////////`

### VoteAllKernel {#voteallkernel}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `  getLastCudaError("VoteAllKernel() execution failed\`

### VoteAllKernel2 {#voteallkernel2}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `p_size, 1);
        VoteAllKernel2<<<gridBlock, thread`

### VoteAnyKernel {#voteanykernel}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `  getLastCudaError("VoteAnyKernel() execution failed\`

### VoteAnyKernel1 {#voteanykernel1}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `p_size, 1);
        VoteAnyKernel1<<<gridBlock, thread`

### VoteAnyKernel3 {#voteanykernel3}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `hronize());
        VoteAnyKernel3<<<1, warp_size * 3>`


## C

### checkErrors1 {#checkerrors1}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `int checkErrors1(unsigned int *h_result, int start, int end, int warp_size, const char *voteType)
{`

### checkErrors2 {#checkerrors2}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `int checkErrors2(unsigned int *h_result, int start, int end, int warp_size, const char *voteType)
{`

### checkResultsVoteAllKernel2 {#checkresultsvoteallkernel2}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `int checkResultsVoteAllKernel2(unsigned int *h_result, int size, int warp_size)
{`

### checkResultsVoteAnyKernel1 {#checkresultsvoteanykernel1}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `int checkResultsVoteAnyKernel1(unsigned int *h_result, int size, int warp_size)
{`

### checkResultsVoteAnyKernel3 {#checkresultsvoteanykernel3}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `int checkResultsVoteAnyKernel3(bool *hinfo, int size)
{`


## G

### genVoteTestPattern {#genvotetestpattern}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `void genVoteTestPattern(unsigned int *VOTE_PATTERN, int size)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleVoteIntrinsics/simpleVoteIntrinsics.cu](./simpleVoteIntrinsics.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

