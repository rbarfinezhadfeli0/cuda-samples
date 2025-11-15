# Keywords: Samples/0_Introduction/simpleCUDA2GL/main.cpp
---

**Total Keywords**: 36

---

## B

### BackBuffer {#backbuffer}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `s = 0;

// CheckFBO/BackBuffer class objects
Check`


## C

### CheckBackBuffer {#checkbackbuffer}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `g_CheckRender = new CheckBackBuffer(window_width, windo`

### CheckFBO {#checkfbo}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `otalErrors = 0;

// CheckFBO/BackBuffer class ob`

### CheckRender {#checkrender}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `uffer class objects
CheckRender *g_CheckRender = NU`

### Cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void Cleanup(int iExitCode)
{`


## E

### EnableQAReadback {#enableqareadback}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `     g_CheckRender->EnableQAReadback(true);
    }

    p`


## F

### FragColor {#fragcolor}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `0\n"
    "out uvec4 FragColor;\n"
    "void main(`

### FreeResource {#freeresource}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void FreeResource()
{`


## I

### IsQAReadback {#isqareadback}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `r && g_CheckRender->IsQAReadback()) {
        static`


## M

### MAX_EPSILON {#maxepsilon}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `#define MAX_EPSILON   10
#define REFRESH_DELAY 10 // ms

const char *sSDKname = "simpleCUDA2GL";

un`

### MacOSX {#macosx}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `c4 output...
// but MacOSX complains about not`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#pragma warning(disable : 4996)
#endif

// OpenGL Graphics inc`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: ` : 4996)
#endif

// OpenGL Graphics includes
#`


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `#define REFRESH_DELAY 10 // ms

const char *sSDKname = "simpleCUDA2GL";

unsigned int g_TotalErrors `


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `      fpsLimit = 1;
StopWatchInterface *timer    = NULL;

`


## U

### USE_TEXSUBIMAGE2D {#usetexsubimage2d}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `#define USE_TEXSUBIMAGE2D
#else
#include <GL/freeglut.h>
#endif

// CUDA includes
#include <cuda_gl_`


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#pragma warning(disable : 4996)
`


## C

### compileGLSLprogram {#compileglslprogram}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `GLuint compileGLSLprogram(const char *vertex_shader_src, const char *fragment_shader_src)
{`

### createPBO {#createpbo}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void createPBO(GLuint *pbo, struct cudaGraphicsResource **pbo_resource)
{`

### createTextureDst {#createtexturedst}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void createTextureDst(GLuint *tex_cudaResult, unsigned int size_x, unsigned int size_y)
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### deletePBO {#deletepbo}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void deletePBO(GLuint *pbo)
{`

### deleteTexture {#deletetexture}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void deleteTexture(GLuint *tex)
{`

### display {#display}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void display()
{`

### displayImage {#displayimage}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void displayImage(GLuint texture)
{`


## G

### generateCUDAImage {#generatecudaimage}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void generateCUDAImage()
{`


## I

### initCUDABuffers {#initcudabuffers}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void initCUDABuffers()
{`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `bool initGL(int *argc, char **argv)
{`

### initGLBuffers {#initglbuffers}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void initGLBuffers()
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void keyboard(unsigned char key, int /*x*/, int /*y*/)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### mainMenu {#mainmenu}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void mainMenu(int i) {`


## O

### objects {#objects}

- **Type**: type
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `class objects`


## R

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void reshape(int w, int h)
{`

### runStdProgram {#runstdprogram}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void runStdProgram(int argc, char **argv)
{`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/0_Introduction/simpleCUDA2GL/main.cpp](./main.cpp_docs.md)
- **Context**: `void timerEvent(int value)
{`

