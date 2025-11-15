# Keywords: Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu
---

**Total Keywords**: 9

---

## N

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `      "%.0f Texels, NumDevsUsed = %d, Workgroup = %`


## S

### SMEM {#smem}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `#define SMEM(X, Y) sdata[(Y) * tilew + (X)]

/*
    2D convolution using shared memory
    - operate`

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `f GPU_PROFILING
    StopWatchInterface *timer = 0;
    sdk`


## C

### clamp {#clamp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `int clamp(int x, int a, int b) {`

### cudaChannelFormatDesc {#cudachannelformatdesc}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `struct cudaChannelFormatDesc`

### cudaProcess {#cudaprocess}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `__global__ void cudaProcess(`


## G

### getPixel {#getpixel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `uchar4 getPixel(int x, int y, cudaTextureObject_t inTex)
{`


## L

### launch_cudaProcess {#launchcudaprocess}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `void launch_cudaProcess(dim3          grid,
                                   dim3          block,
`


## R

### rgbToInt {#rgbtoint}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/postProcessGL/postProcessGL.cu](./postProcessGL.cu_docs.md)
- **Context**: `int rgbToInt(float r, float g, float b)
{`

