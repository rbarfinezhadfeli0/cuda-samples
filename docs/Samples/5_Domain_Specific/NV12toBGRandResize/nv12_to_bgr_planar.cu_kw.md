# Keywords: Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu
---

**Total Keywords**: 5

---

## C

### CONV_THREADS_X {#convthreadsx}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu](./nv12_to_bgr_planar.cu_docs.md)
- **Context**: `#define CONV_THREADS_X 64
#define CONV_THREADS_Y 10

__forceinline__ __device__ static float clampF(`

### CONV_THREADS_Y {#convthreadsy}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu](./nv12_to_bgr_planar.cu_docs.md)
- **Context**: `#define CONV_THREADS_Y 10

__forceinline__ __device__ static float clampF(float x, float lower, floa`

### clampF {#clampf}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu](./nv12_to_bgr_planar.cu_docs.md)
- **Context**: `float clampF(float x, float lower, float upper)
{`


## N

### nv12ToBGRplanarBatch {#nv12tobgrplanarbatch}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu](./nv12_to_bgr_planar.cu_docs.md)
- **Context**: `void nv12ToBGRplanarBatch(uint8_t     *pNv12,
                          int          nNv12Pitch,
   `

### nv12ToBGRplanarBatchKernel {#nv12tobgrplanarbatchkernel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/NV12toBGRandResize/nv12_to_bgr_planar.cu](./nv12_to_bgr_planar.cu_docs.md)
- **Context**: `void nv12ToBGRplanarBatchKernel(const uint8_t *pNv12,
                                              `

