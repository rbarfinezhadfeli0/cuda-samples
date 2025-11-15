# Keywords: Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h
---

**Total Keywords**: 17

---

## B

### BLOCKDIM_X {#blockdimx}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define BLOCKDIM_X 8
#define BLOCKDIM_Y 8

#ifndef MAX
#define MAX(a, b) ((a < b) ? b : a)
#endif
#i`

### BLOCKDIM_Y {#blockdimy}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define BLOCKDIM_Y 8

#ifndef MAX
#define MAX(a, b) ((a < b) ? b : a)
#endif
#ifndef MIN
#define MIN`


## I

### IMAGE_DENOISING_H {#imagedenoisingh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define IMAGE_DENOISING_H

typedef unsigned int TColor;

///////////////////////////////////////////`

### INV_KNN_WINDOW_AREA {#invknnwindowarea}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define INV_KNN_WINDOW_AREA (1.0f / (float)KNN_WINDOW_AREA)
#define INV_NLM_WINDOW_AREA (1.0f / (flo`

### INV_NLM_WINDOW_AREA {#invnlmwindowarea}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define INV_NLM_WINDOW_AREA (1.0f / (float)NLM_WINDOW_AREA)

#define KNN_WEIGHT_THRESHOLD 0.02f
#def`


## K

### KNN_LERP_THRESHOLD {#knnlerpthreshold}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define KNN_LERP_THRESHOLD   0.79f
#define NLM_WEIGHT_THRESHOLD 0.10f
#define NLM_LERP_THRESHOLD   0`

### KNN_WEIGHT_THRESHOLD {#knnweightthreshold}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define KNN_WEIGHT_THRESHOLD 0.02f
#define KNN_LERP_THRESHOLD   0.79f
#define NLM_WEIGHT_THRESHOLD 0`

### KNN_WINDOW_AREA {#knnwindowarea}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define KNN_WINDOW_AREA     ((2 * KNN_WINDOW_RADIUS + 1) * (2 * KNN_WINDOW_RADIUS + 1))
#define NLM_`

### KNN_WINDOW_RADIUS {#knnwindowradius}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define KNN_WINDOW_RADIUS   3
#define NLM_WINDOW_RADIUS   3
#define NLM_BLOCK_RADIUS    3
#define KN`


## L

### LoadBMPFile {#loadbmpfile}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `ges
extern "C" void LoadBMPFile(uchar4 **dst, int *`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define MAX(a, b) ((a < b) ? b : a)
#endif
#ifndef MIN
#define MIN(a, b) ((a < b) ? a : b)
#endif

/`

### MIN {#min}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define MIN(a, b) ((a < b) ? a : b)
#endif

// functions to load images
extern "C" void LoadBMPFile(`


## N

### NLM_BLOCK_RADIUS {#nlmblockradius}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define NLM_BLOCK_RADIUS    3
#define KNN_WINDOW_AREA     ((2 * KNN_WINDOW_RADIUS + 1) * (2 * KNN_WI`

### NLM_LERP_THRESHOLD {#nlmlerpthreshold}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define NLM_LERP_THRESHOLD   0.10f

#define BLOCKDIM_X 8
#define BLOCKDIM_Y 8

#ifndef MAX
#define M`

### NLM_WEIGHT_THRESHOLD {#nlmweightthreshold}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define NLM_WEIGHT_THRESHOLD 0.10f
#define NLM_LERP_THRESHOLD   0.10f

#define BLOCKDIM_X 8
#define `

### NLM_WINDOW_AREA {#nlmwindowarea}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define NLM_WINDOW_AREA     ((2 * NLM_WINDOW_RADIUS + 1) * (2 * NLM_WINDOW_RADIUS + 1))
#define INV_`

### NLM_WINDOW_RADIUS {#nlmwindowradius}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoising.h](./imageDenoising.h_docs.md)
- **Context**: `#define NLM_WINDOW_RADIUS   3
#define NLM_BLOCK_RADIUS    3
#define KNN_WINDOW_AREA     ((2 * KNN_WI`

