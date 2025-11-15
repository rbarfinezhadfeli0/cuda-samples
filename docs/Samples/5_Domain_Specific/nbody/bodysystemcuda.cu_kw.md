# Keywords: Samples/5_Domain_Specific/nbody/bodysystemcuda.cu
---

**Total Keywords**: 10

---

## D

### DeviceData {#devicedata}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `struct DeviceData`


## S

### SX_SUM {#sxsum}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `#define SX_SUM(i, j) sharedPos[i + blockDim.x * j]

template <typename T> __device__ T getSofteningS`

### SharedMemory {#sharedmemory}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `struct SharedMemory`


## B

### bodyBodyInteraction {#bodybodyinteraction}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `Type
bodyBodyInteraction(typename vec3<T>::Type ai, typename vec4<T>::Type bi, typename vec4<T>::Typ`


## C

### computeBodyAccel {#computebodyaccel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `Type
computeBodyAccel(typename vec4<T>::Type bodyPos, typename vec4<T>::Type *positions, int numTile`


## G

### getSofteningSquared {#getsofteningsquared}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `T getSofteningSquared() {`


## I

### integrateBodies {#integratebodies}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `__global__ void integrateBodies(`

### integrateNbodySystem {#integratenbodysystem}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `void integrateNbodySystem(DeviceData<T>         *deviceData,
                          cudaGraphicsR`


## R

### rsqrt_T {#rsqrtt}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `T rsqrt_T(T x) {`


## S

### setSofteningSquared {#setsofteningsquared}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/bodysystemcuda.cu](./bodysystemcuda.cu_docs.md)
- **Context**: `cudaError_t setSofteningSquared(double softeningSq)
{`

