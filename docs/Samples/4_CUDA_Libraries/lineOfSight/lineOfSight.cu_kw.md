# Keywords: Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu
---

**Total Keywords**: 12

---

## H

### HeightField {#heightfield}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `struct HeightField`

### HeightFieldTex {#heightfieldtex}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `cudaTextureObject_t HeightFieldTex)
{
    uint i = blo`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `#define NOMINMAX
#endif

// includes, system
#include <float.h>
#include <math.h>
#include <stdio.h>`


## R

### Ray {#ray}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `struct Ray`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `

    // Create
    StopWatchInterface *timer;
    sdkCrea`


## C

### computeAngles_kernel {#computeangleskernel}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `__global__ void computeAngles_kernel(`

### computeVisibilities_kernel {#computevisibilitieskernel}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `__global__ void
computeVisibilities_kernel(`


## G

### getAngle {#getangle}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `float getAngle(const Ray ray, float2 location, float height)
{`

### getLocation {#getlocation}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `float2 getLocation(const Ray ray, int i)
{`


## L

### lineOfSight_gold {#lineofsightgold}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `void lineOfSight_gold(const HeightField heightField, const Ray ray, Bool *visibilities)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/lineOfSight/lineOfSight.cu](./lineOfSight.cu_docs.md)
- **Context**: `int runTest(int argc, char **argv)
{`

