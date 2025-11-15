# Keywords: Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp
---

**Total Keywords**: 31

---

## M

### MAX_EPSILON {#maxepsilon}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `#define MAX_EPSILON   0.10f
#define THRESHOLD     0.15f
#define REFRESH_DELAY 10 // ms

////////////`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#endif

// includes for OpenGL
#include <helper_gl.h>

// incl`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `if

// includes for OpenGL
#include <helper_gl`


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `#define REFRESH_DELAY 10 // ms

////////////////////////////////////////////////////////////////////`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `dirDepend = 0.07f;

StopWatchInterface *timer         = NU`


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `#define THRESHOLD     0.15f
#define REFRESH_DELAY 10 // ms

////////////////////////////////////////`


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

// includes for OpenGL
#`


## A

### attachShader {#attachshader}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `int attachShader(GLuint prg, GLenum type, const char *name)
{`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void cleanup()
{`

### createMeshIndexBuffer {#createmeshindexbuffer}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void createMeshIndexBuffer(GLuint *id, int w, int h)
{`

### createMeshPositionVBO {#createmeshpositionvbo}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void createMeshPositionVBO(GLuint *id, int w, int h)
{`

### createVBO {#createvbo}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void createVBO(GLuint *vbo, int size)
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### deleteVBO {#deletevbo}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void deleteVBO(GLuint *vbo)
{`

### display {#display}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void display()
{`


## G

### gauss {#gauss}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `float gauss()
{`

### generate_h0 {#generateh0}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void generate_h0(float2 *h0)
{`


## I

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `bool initGL(int *argc, char **argv)
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void keyboard(unsigned char key, int /*x*/, int /*y*/)
{`


## L

### loadGLSLProgram {#loadglslprogram}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `GLuint loadGLSLProgram(const char *vertFileName, const char *fragFileName)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### motion {#motion}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void motion(int x, int y)
{`

### mouse {#mouse}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void mouse(int button, int state, int x, int y)
{`


## P

### phillips {#phillips}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `float phillips(float Kx, float Ky, float Vdir, float V, float A, float dir_depend)
{`


## R

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void reshape(int w, int h)
{`

### runAutoTest {#runautotest}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void runAutoTest(int argc, char **argv)
{`

### runCuda {#runcuda}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void runCuda()
{`

### runCudaTest {#runcudatest}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void runCudaTest(char *exec_path)
{`

### runGraphicsTest {#rungraphicstest}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void runGraphicsTest(int argc, char **argv)
{`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `void timerEvent(int value)
{`


## U

### urand {#urand}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/oceanFFT/oceanFFT.cpp](./oceanFFT.cpp_docs.md)
- **Context**: `float urand() {`

