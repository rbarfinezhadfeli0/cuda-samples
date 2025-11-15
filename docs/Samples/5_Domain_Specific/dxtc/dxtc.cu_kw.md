# Keywords: Samples/5_Domain_Specific/dxtc/dxtc.cu
---

**Total Keywords**: 23

---

## B

### BlockDXT1 {#blockdxt1}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `struct BlockDXT1`


## C

### CudaMath {#cudamath}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `_math.h>

#include "CudaMath.h"
#include "dds.h"`


## E

### ERROR_THRESHOLD {#errorthreshold}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `#define ERROR_THRESHOLD 0.02f

#define NUM_THREADS 64 // Number of threads per block.

#define __deb`


## I

### INPUT_IMAGE {#inputimage}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `#define INPUT_IMAGE     "teapot512_std.ppm"
#define REFERENCE_IMAGE "teapot512_ref.dds"

#define ERR`


## N

### NUM_THREADS {#numthreads}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `#define NUM_THREADS 64 // Number of threads per block.

#define __debugsync()

template <class T> __`

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `xels, "
           "NumDevsUsed = %i, Workgroup = %`


## R

### REFERENCE_IMAGE {#referenceimage}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `#define REFERENCE_IMAGE "teapot512_ref.dds"

#define ERROR_THRESHOLD 0.02f

#define NUM_THREADS 64 /`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: ` create a timer
    StopWatchInterface *timer = NULL;
    `


## U

### USE_TABLES {#usetables}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `#define USE_TABLES 1

//////////////////////////////////////////////////////////////////////////////`


## _

### __debugsync {#debugsync}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `#define __debugsync()

template <class T> __device__ inline void swap(T &a, T &b)
{
    T tmp = a;
 `


## C

### compareBlock {#compareblock}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `int compareBlock(const BlockDXT1 *b0, const BlockDXT1 *b1)
{`

### compareColors {#comparecolors}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `int compareColors(const Color32 *b0, const Color32 *b1)
{`

### compress {#compress}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `__global__ void compress(`


## E

### evalAllPermutations {#evalallpermutations}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `void evalAllPermutations(const float3    *colors,
                                    const uint    `

### evalPermutation3 {#evalpermutation3}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `float
evalPermutation3(const float3 *colors, uint permutation, ushort *start, ushort *end, float3 co`

### evalPermutation4 {#evalpermutation4}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `float
evalPermutation4(const float3 *colors, uint permutation, ushort *start, ushort *end, float3 co`


## F

### findMinError {#findminerror}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `int findMinError(float *errors, cg::thread_block cta)
{`


## L

### loadColorBlock {#loadcolorblock}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `void loadColorBlock(const uint      *image,
                               float3           colors[1`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### roundAndExpand {#roundandexpand}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `float3 roundAndExpand(float3 v, ushort *w)
{`


## S

### saveBlockDXT1 {#saveblockdxt1}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `void saveBlockDXT1(ushort start, ushort end, uint permutation, int xrefs[16], uint2 *result, int blo`

### sortColors {#sortcolors}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `void sortColors(const float *values, int *ranks, cg::thread_group tile)
{`

### swap {#swap}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/dxtc/dxtc.cu](./dxtc.cu_docs.md)
- **Context**: `void swap(T &a, T &b)
{`

