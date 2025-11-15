# Keywords: Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh
---

**Total Keywords**: 10

---

## S

### SimParams {#simparams}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `memory
__constant__ SimParams cudaParams;

struct`


## _

### _PARTICLES_KERNEL_H_ {#particleskernelh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `#define _PARTICLES_KERNEL_H_

#include <cooperative_groups.h>
#include <math.h>
#include <stdio.h>

`


## C

### calcGridHash {#calcgridhash}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `uint calcGridHash(int3 gridPos)
{`

### calcGridPos {#calcgridpos}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `int3 calcGridPos(float3 p)
{`

### calcHashD {#calchashd}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `__global__ void calcHashD(`

### collideCell {#collidecell}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `float3 collideCell(int3    gridPos,
                              uint    index,
                   `

### collideD {#collided}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `__global__ void collideD(`

### collideSpheres {#collidespheres}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `float3
collideSpheres(float3 posA, float3 posB, float3 velA, float3 velB, float radiusA, float radiu`


## I

### integrate_functor {#integratefunctor}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `struct integrate_functor`


## R

### reorderDataAndFindCellStartD {#reorderdataandfindcellstartd}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles_kernel_impl.cuh](./particles_kernel_impl.cuh_docs.md)
- **Context**: `__global__ void reorderDataAndFindCellStartD(`

