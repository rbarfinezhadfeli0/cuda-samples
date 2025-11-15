# Keywords: Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu
---

**Total Keywords**: 21

---

## T

### ThrustScanWrapper {#thrustscanwrapper}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void ThrustScanWrapper(unsigned int *output, unsigned int *input, unsigned int numElements)
{`


## _

### _MARCHING_CUBES_KERNEL_CU_ {#marchingcubeskernelcu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `#define _MARCHING_CUBES_KERNEL_CU_

#include <cuda_runtime_api.h>
#include <helper_cuda.h> // includ`


## A

### allocateTextures {#allocatetextures}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void allocateTextures(uint **d_edgeTable, uint **d_triTable, uint **d_numVertsTable)
{`


## C

### calcGridPos {#calcgridpos}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `uint3 calcGridPos(uint i, uint3 gridSizeShift, uint3 gridSizeMask)
{`

### calcNormal {#calcnormal}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `float3 calcNormal(float3 *v0, float3 *v1, float3 *v2)
{`

### classifyVoxel {#classifyvoxel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `__global__ void classifyVoxel(`

### compactVoxels {#compactvoxels}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `__global__ void compactVoxels(`

### createVolumeTexture {#createvolumetexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void createVolumeTexture(uchar *d_volume, size_t buffSize)
{`


## D

### destroyAllTextureObjects {#destroyalltextureobjects}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void destroyAllTextureObjects()
{`


## F

### fieldFunc {#fieldfunc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `float fieldFunc(float3 p) {`

### fieldFunc4 {#fieldfunc4}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `float4 fieldFunc4(float3 p)
{`


## G

### generateTriangles {#generatetriangles}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `__global__ void generateTriangles(`

### generateTriangles2 {#generatetriangles2}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `__global__ void generateTriangles2(`


## L

### launch_classifyVoxel {#launchclassifyvoxel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void launch_classifyVoxel(dim3   grid,
                                     dim3   threads,
        `

### launch_compactVoxels {#launchcompactvoxels}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void launch_compactVoxels(dim3  grid,
                                     dim3  threads,
          `

### launch_generateTriangles {#launchgeneratetriangles}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void launch_generateTriangles(dim3    grid,
                                         dim3    threads`

### launch_generateTriangles2 {#launchgeneratetriangles2}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void launch_generateTriangles2(dim3    grid,
                                          dim3    threa`


## S

### sampleVolume {#samplevolume}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `float sampleVolume(cudaTextureObject_t volumeTex, uchar *data, uint3 p, uint3 gridSize)
{`


## T

### tangle {#tangle}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `float tangle(float x, float y, float z)
{`


## V

### vertexInterp {#vertexinterp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `float3 vertexInterp(float isolevel, float3 p0, float3 p1, float f0, float f1)
{`

### vertexInterp2 {#vertexinterp2}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes_kernel.cu](./marchingCubes_kernel.cu_docs.md)
- **Context**: `void vertexInterp2(float isolevel, float3 p0, float3 p1, float4 f0, float4 f1, float3 &p, float3 &n)`

