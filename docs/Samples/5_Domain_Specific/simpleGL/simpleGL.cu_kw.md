# Keywords: Samples/5_Domain_Specific/simpleGL/simpleGL.cu
---

**Total Keywords**: 29

---

## F

### FOPEN {#fopen}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define FOPEN(fHandle, filename, mode) (fHandle = fopen(filename, mode))
#endif
#endif

void sdkDump`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define MAX(a, b) ((a > b) ? a : b)

///////////////////////////////////////////////////////////////`

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 10.0f
#define THRESHOLD         0.30f
#define REFRESH_DELAY     10 // ms

`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#endif

// OpenGL Graphics includes
#include <helper_gl.h>
#if`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `how to use the Cuda OpenGL bindings to
    dyn`


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define REFRESH_DELAY     10 // ms

////////////////////////////////////////////////////////////////`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `ranslate_z = -3.0;

StopWatchInterface *timer = NULL;

// `


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define THRESHOLD         0.30f
#define REFRESH_DELAY     10 // ms

////////////////////////////////`


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

// OpenGL Graphics inclu`


## C

### checkResultCuda {#checkresultcuda}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void checkResultCuda(int argc, char **argv, const GLuint &vbo)
{`

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void cleanup()
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void computeFPS()
{`

### createVBO {#createvbo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void createVBO(GLuint *vbo, struct cudaGraphicsResource **vbo_res, unsigned int vbo_res_flags)
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### deleteVBO {#deletevbo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void deleteVBO(GLuint *vbo, struct cudaGraphicsResource *vbo_res)
{`

### display {#display}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void display()
{`


## G

### glutCloseFunc {#glutclosefunc}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `#define glutCloseFunc glutWMCloseFunc
#endif
#else
#include <GL/freeglut.h>
#endif

// includes, cud`


## I

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `bool initGL(int *argc, char **argv)
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void keyboard(unsigned char key, int /*x*/, int /*y*/)
{`


## L

### launch_kernel {#launchkernel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void launch_kernel(float4 *pos, unsigned int mesh_width, unsigned int mesh_height, float time)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### motion {#motion}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void motion(int x, int y)
{`

### mouse {#mouse}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void mouse(int button, int state, int x, int y)
{`


## R

### runAutoTest {#runautotest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void runAutoTest(int devID, char **argv, char *ref_file)
{`

### runCuda {#runcuda}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void runCuda(struct cudaGraphicsResource **vbo_resource)
{`

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `bool runTest(int argc, char **argv, char *ref_file)
{`


## S

### sdkDumpBin2 {#sdkdumpbin2}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void sdkDumpBin2(void *data, unsigned int bytes, const char *filename)
{`

### simple_vbo_kernel {#simplevbokernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `__global__ void simple_vbo_kernel(`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleGL/simpleGL.cu](./simpleGL.cu_docs.md)
- **Context**: `void timerEvent(int value)
{`

