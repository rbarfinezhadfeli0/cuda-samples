# Keywords: Samples/5_Domain_Specific/binomialOptions_nvrtc/common_gpu_header.h
---

**Total Keywords**: 5

---

## C

### CACHE_DELTA {#cachedelta}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions_nvrtc/common_gpu_header.h](./common_gpu_header.h_docs.md)
- **Context**: `#define CACHE_DELTA (2 * TIME_STEPS)

#define CACHE_SIZE (256)

#define CACHE_STEP (CACHE_SIZE - CAC`

### CACHE_SIZE {#cachesize}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions_nvrtc/common_gpu_header.h](./common_gpu_header.h_docs.md)
- **Context**: `#define CACHE_SIZE (256)

#define CACHE_STEP (CACHE_SIZE - CACHE_DELTA)

#if NUM_STEPS % CACHE_DELTA`

### CACHE_STEP {#cachestep}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions_nvrtc/common_gpu_header.h](./common_gpu_header.h_docs.md)
- **Context**: `#define CACHE_STEP (CACHE_SIZE - CACHE_DELTA)

#if NUM_STEPS % CACHE_DELTA
#error Bad constants
#end`


## T

### TIME_STEPS {#timesteps}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions_nvrtc/common_gpu_header.h](./common_gpu_header.h_docs.md)
- **Context**: `#define TIME_STEPS 16

#define CACHE_DELTA (2 * TIME_STEPS)

#define CACHE_SIZE (256)

#define CACHE`


## _

### __COMMON_GPU_HEADER_H {#commongpuheaderh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions_nvrtc/common_gpu_header.h](./common_gpu_header.h_docs.md)
- **Context**: `#define __COMMON_GPU_HEADER_H

/////////////////////////////////////////////////////////////////////`

