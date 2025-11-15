# Keywords: Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh
---

**Total Keywords**: 8

---

## R

### RAD {#rad}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `#define RAD 8
// STEPS is the number of loads we must perform to initialize the shared memory
// are`


## S

### STEPS {#steps}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `#define STEPS 3

#include <cooperative_groups.h>

namespace cg = cooperative_groups;

//////////////`


## _

### _STEREODISPARITY_KERNEL_H_ {#stereodisparitykernelh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `#define _STEREODISPARITY_KERNEL_H_

#define blockSize_x 32
#define blockSize_y 8

// RAD is the radi`

### __usad4 {#usad4}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `int __usad4(unsigned int A, unsigned int B, unsigned int C = 0)
{`


## B

### blockSize_x {#blocksizex}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `#define blockSize_x 32
#define blockSize_y 8

// RAD is the radius of the region of support for the `

### blockSize_y {#blocksizey}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `#define blockSize_y 8

// RAD is the radius of the region of support for the search
#define RAD 8
//`


## C

### cpu_gold_stereo {#cpugoldstereo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `void cpu_gold_stereo(unsigned int *img0,
                     unsigned int *img1,
                  `


## S

### stereoDisparityKernel {#stereodisparitykernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/stereoDisparity/stereoDisparity_kernel.cuh](./stereoDisparity_kernel.cuh_docs.md)
- **Context**: `__global__ void stereoDisparityKernel(`

