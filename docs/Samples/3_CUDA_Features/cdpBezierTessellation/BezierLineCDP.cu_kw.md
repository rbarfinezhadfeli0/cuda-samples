# Keywords: Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu
---

**Total Keywords**: 10

---

## B

### BLOCK_DIM {#blockdim}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `#define BLOCK_DIM 64
int main(int argc, char **argv)
{
    BezierLine *bLines_h = new BezierLine[N_L`

### BezierLine {#bezierline}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `struct BezierLine`


## M

### MAX_TESSELLATION {#maxtessellation}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `#define MAX_TESSELLATION 32
struct BezierLine
{
    float2  CP[3];
    float2 *vertexPos;
    int   `


## N

### N_LINES {#nlines}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `#define N_LINES   256
#define BLOCK_DIM 64
int main(int argc, char **argv)
{
    BezierLine *bLines_`


## C

### checkCapableSM35Device {#checkcapablesm35device}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `int checkCapableSM35Device(int argc, char **argv)
{`

### computeBezierLinePositions {#computebezierlinepositions}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `__global__ void computeBezierLinePositions(`

### computeBezierLinesCDP {#computebezierlinescdp}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `__global__ void computeBezierLinesCDP(`


## F

### freeVertexMem {#freevertexmem}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `__global__ void freeVertexMem(`


## L

### length {#length}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `float length(float2 a) {`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpBezierTessellation/BezierLineCDP.cu](./BezierLineCDP.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

