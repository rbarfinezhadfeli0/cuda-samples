# Keywords: Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu
---

**Total Keywords**: 32

---

## B

### ButtonPress {#buttonpress}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `  if (event.type == ButtonPress) {
                `

### ButtonRelease {#buttonrelease}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `  if (event.type == ButtonRelease) {
                `


## F

### FOPEN {#fopen}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define FOPEN(fHandle, filename, mode) (fHandle = fopen(filename, mode))
#endif
#endif

void sdkDump`


## G

### GUI_IDLE {#guiidle}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define GUI_IDLE      0x100
#define GUI_ROTATE    0x101
#define GUI_TRANSLATE 0x102

int gui_mode;

`

### GUI_ROTATE {#guirotate}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define GUI_ROTATE    0x101
#define GUI_TRANSLATE 0x102

int gui_mode;

////////////////////////////`

### GUI_TRANSLATE {#guitranslate}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define GUI_TRANSLATE 0x102

int gui_mode;

////////////////////////////////////////////////////////`


## I

### InitGraphicsState {#initgraphicsstate}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void InitGraphicsState(void)
{`


## K

### KeyPress {#keypress}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `  if (event.type == KeyPress && XLookupString(&e`

### KeySym {#keysym}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `vent event;
        KeySym key;
        char  `


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define MAX(a, b) ((a > b) ? a : b)

///////////////////////////////////////////////////////////////`

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 0.0f
#define THRESHOLD         0.0f
#define REFRESH_DELAY     1 // ms

#de`

### MotionNotify {#motionnotify}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `  if (event.type == MotionNotify) {
                `


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#endif

// includes, cuda
#include <cuda_gl_interop.h>
#includ`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: ` CUDA C bindings to OpenGL ES to
    dynamical`


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define REFRESH_DELAY     1 // ms

#define GUI_IDLE      0x100
#define GUI_ROTATE    0x101
#define G`


## S

### ShaderCreate {#shadercreate}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `GLuint ShaderCreate(const char *vshader_filename, const char *fshader_filename)
{`

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `ranslate_z = -3.0;

StopWatchInterface *timer = NULL;

// `


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define THRESHOLD         0.0f
#define REFRESH_DELAY     1 // ms

#define GUI_IDLE      0x100
#defin`


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

// includes, cuda
#inclu`


## C

### checkResultCuda {#checkresultcuda}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void checkResultCuda(int argc, char **argv, const GLuint &vbo)
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void computeFPS()
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### display_thisframe {#displaythisframe}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void display_thisframe(float time_delta)
{`


## E

### error_exit {#errorexit}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void error_exit(const char *format, ...)
{`


## L

### launch_kernel {#launchkernel}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void launch_kernel(float4 *pos, unsigned int mesh_width, unsigned int mesh_height, float time)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### readAndCompileShaderFromGLSLFile {#readandcompileshaderfromglslfile}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void readAndCompileShaderFromGLSLFile(GLuint new_shaderprogram, const char *filename, GLenum shaderT`

### runAutoTest {#runautotest}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void runAutoTest(int devID, char **argv, char *ref_file)
{`

### runCuda {#runcuda}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void runCuda(struct cudaGraphicsResource **vbo_resource)
{`

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `bool runTest(int argc, char **argv, char *ref_file)
{`


## S

### sdkDumpBin2 {#sdkdumpbin2}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `void sdkDumpBin2(void *data, unsigned int bytes, const char *filename)
{`

### simple_vbo_kernel {#simplevbokernel}

- **Type**: cuda_kernel
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/simpleGLES.cu](./simpleGLES.cu_docs.md)
- **Context**: `__global__ void simple_vbo_kernel(`

