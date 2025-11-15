# Keywords: Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp
---

**Total Keywords**: 36

---

## D

### DEBUG_BUFFERS {#debugbuffers}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define DEBUG_BUFFERS 0

///////////////////////////////////////////////////////////////////////////`


## E

### EPSILON {#epsilon}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define EPSILON   5.0f
#define THRESHOLD 0.30f

void animation()
{
    if (animate) {
        isoVal`


## M

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 5.0f
#define REFRESH_DELAY     10 // ms

// Define the files that are to b`

### MarchingCubes {#marchingcubes}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `argv)
{
    printf("MarchingCubes\n");

    if (check`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#endif

// includes for OpenGL
#include <helper_gl.h>

// incl`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `if

// includes for OpenGL
#include <helper_gl`


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define REFRESH_DELAY     10 // ms

// Define the files that are to be save and the reference images`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `cubes.ppm", NULL};

StopWatchInterface *timer = 0;

// Aut`


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define THRESHOLD 0.30f

void animation()
{
    if (animate) {
        isoValue += dIsoValue;

     `

### ThrustScanWrapper {#thrustscanwrapper}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `();
extern "C" void ThrustScanWrapper(unsigned int *outpu`


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

// includes for OpenGL
#`


## A

### animation {#animation}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void animation()
{`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void cleanup()
{`

### compileASMShader {#compileasmshader}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `GLuint compileASMShader(GLenum program_type, const char *code)
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void computeFPS()
{`

### computeIsosurface {#computeisosurface}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void computeIsosurface()
{`

### createVBO {#createvbo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void createVBO(GLuint *vbo, unsigned int size)
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### deleteVBO {#deletevbo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void deleteVBO(GLuint *vbo, struct cudaGraphicsResource **cuda_resource)
{`

### display {#display}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void display()
{`

### dumpBuffer {#dumpbuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void dumpBuffer(T *d_buffer, int nelements, int size_element)
{`

### dumpFile {#dumpfile}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void dumpFile(void *dData, int data_bytes, const char *file_name)
{`


## I

### idle {#idle}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void idle()
{`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `bool initGL(int *argc, char **argv)
{`

### initMC {#initmc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void initMC(int argc, char **argv)
{`

### initMenus {#initmenus}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void initMenus()
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void keyboard(unsigned char key, int /*x*/, int /*y*/)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### mainMenu {#mainmenu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void mainMenu(int i) {`

### motion {#motion}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void motion(int x, int y)
{`

### mouse {#mouse}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void mouse(int button, int state, int x, int y)
{`


## R

### renderIsosurface {#renderisosurface}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void renderIsosurface()
{`

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void reshape(int w, int h)
{`

### runAutoTest {#runautotest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void runAutoTest(int argc, char **argv)
{`

### runGraphicsTest {#rungraphicstest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void runGraphicsTest(int argc, char **argv)
{`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/marchingCubes/marchingCubes.cpp](./marchingCubes.cpp_docs.md)
- **Context**: `void timerEvent(int value)
{`

