# Keywords: Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp
---

**Total Keywords**: 21

---

## A

### AutoTest {#autotest}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `m;
        printf("[AutoTest]: %s <%s>\n", sSDKs`


## B

### BUFFER_DATA {#bufferdata}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `#define BUFFER_DATA(i) ((char *)0 + i)

// Auto-Verification Code
const int    frameCheckNumber = 4;`


## I

### ImageDenoising {#imagedenoising}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `*sSDKsample = "CUDA ImageDenoising";

const char *filt`


## L

### LoadBMPFile {#loadbmpfile}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `AILURE);
    }

    LoadBMPFile(&h_Src, &imageW, &i`


## M

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 5
#define REFRESH_DELAY     10 // ms

void cleanup();

void computeFPS()
{`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `planations.
 */

// OpenGL Graphics includes
#`

### OutputDebugString {#outputdebugstring}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `gl_error));
        OutputDebugString(tmpStr);
#endif
   `


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `#define REFRESH_DELAY     10 // ms

void cleanup();

void computeFPS()
{
    frameCount++;
    fpsCo`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `  g_Diag   = false;
StopWatchInterface *timer    = NULL;

`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void cleanup()
{`

### compileASMShader {#compileasmshader}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `GLuint compileASMShader(GLenum program_type, const char *code)
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void computeFPS()
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### displayFunc {#displayfunc}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void displayFunc(void)
{`


## I

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `int initGL(int *argc, char **argv)
{`

### initOpenGLBuffers {#initopenglbuffers}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void initOpenGLBuffers()
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void keyboard(unsigned char k, int /*x*/, int /*y*/)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### runAutoTest {#runautotest}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void runAutoTest(int argc, char **argv, const char *filename, int kernel_param)
{`

### runImageFilters {#runimagefilters}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void runImageFilters(TColor *d_dst)
{`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/imageDenoising/imageDenoisingGL.cpp](./imageDenoisingGL.cpp_docs.md)
- **Context**: `void timerEvent(int value)
{`

