# Keywords: Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu
---

**Total Keywords**: 19

---

## S

### SimParams {#simparams}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: ` void setParameters(SimParams *hostParams)
    {
`


## A

### allocateArray {#allocatearray}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void allocateArray(void **devPtr, size_t size) {`


## C

### calcHash {#calchash}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void calcHash(uint *gridParticleHash, uint *gridParticleIndex, float *pos, int numParticles)
    {`

### collide {#collide}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void collide(float *newVel,
                 float *sortedPos,
                 float *sortedVel,
  `

### computeGridSize {#computegridsize}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void computeGridSize(uint n, uint blockSize, uint &numBlocks, uint &numThreads)
    {`

### copyArrayFromDevice {#copyarrayfromdevice}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void copyArrayFromDevice(void *host, const void *device, struct cudaGraphicsResource **cuda_vbo_reso`

### copyArrayToDevice {#copyarraytodevice}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void copyArrayToDevice(void *device, const void *host, int offset, int size)
    {`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `struct cudaGraphicsResource`

### cudaInit {#cudainit}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void cudaInit(int argc, char **argv)
    {`


## F

### freeArray {#freearray}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void freeArray(void *devPtr) {`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `uint iDivUp(uint a, uint b) {`

### integrateSystem {#integratesystem}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void integrateSystem(float *pos, float *vel, float deltaTime, uint numParticles)
    {`


## R

### registerGLBufferObject {#registerglbufferobject}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void registerGLBufferObject(uint vbo, struct cudaGraphicsResource **cuda_vbo_resource)
    {`

### reorderDataAndFindCellStart {#reorderdataandfindcellstart}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void reorderDataAndFindCellStart(uint  *cellStart,
                                     uint  *cellE`


## S

### setParameters {#setparameters}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void setParameters(SimParams *hostParams)
    {`

### sortParticles {#sortparticles}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void sortParticles(uint *dGridParticleHash, uint *dGridParticleIndex, uint numParticles)
    {`


## T

### threadSync {#threadsync}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void threadSync() {`


## U

### unmapGLBufferObject {#unmapglbufferobject}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void unmapGLBufferObject(struct cudaGraphicsResource *cuda_vbo_resource)
    {`

### unregisterGLBufferObject {#unregisterglbufferobject}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem_cuda.cu](./particleSystem_cuda.cu_docs.md)
- **Context**: `void unregisterGLBufferObject(struct cudaGraphicsResource *cuda_vbo_resource)
    {`

