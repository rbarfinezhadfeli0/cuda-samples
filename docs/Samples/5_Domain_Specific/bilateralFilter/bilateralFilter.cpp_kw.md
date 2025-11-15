# Keywords: Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp
---

**Total Keywords**: 31

---

## B

### BilateralFilterGPU {#bilateralfiltergpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `  printf("\nRunning BilateralFilterGPU for %d cycles...\n\`


## G

### GL_TEXTURE_TYPE {#gltexturetype}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define GL_TEXTURE_TYPE GL_TEXTURE_2D

extern "C" void loadImageData(int argc, char **argv);

// The`


## L

### LoadBMPFile {#loadbmpfile}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `;
extern "C" void   LoadBMPFile(uchar4 **dst, unsig`


## M

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 5.0f
#define REFRESH_DELAY     10 // ms
#define MIN_EUCLIDEAN_D   0.01f
#d`

### MAX_EUCLIDEAN_D {#maxeuclideand}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define MAX_EUCLIDEAN_D   5.f
#define MAX_FILTER_RADIUS 25

const static char *sSDKsample = "CUDA Bi`

### MAX_FILTER_RADIUS {#maxfilterradius}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define MAX_FILTER_RADIUS 25

const static char *sSDKsample = "CUDA Bilateral Filter";

const char *`

### MIN_EUCLIDEAN_D {#mineuclideand}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define MIN_EUCLIDEAN_D   0.01f
#define MAX_EUCLIDEAN_D   5.f
#define MAX_FILTER_RADIUS 25

const st`


## N

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `e = %u RGBA Pixels, NumDevsUsed = %u\n",
          `


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `an image
  and uses OpenGL to display the resu`


## R

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define REFRESH_DELAY     10 // ms
#define MIN_EUCLIDEAN_D   0.01f
#define MAX_EUCLIDEAN_D   5.f
#de`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `ar **pArgv = NULL;

StopWatchInterface *timer        = NUL`


## C

### checkCUDAProfile {#checkcudaprofile}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `bool checkCUDAProfile(int dev, int min_runtime, int min_compute)
{`

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void cleanup()
{`

### compileASMShader {#compileasmshader}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `GLuint compileASMShader(GLenum program_type, const char *code)
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void computeFPS()
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### display {#display}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void display()
{`


## G

### glutCloseFunc {#glutclosefunc}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `#define glutCloseFunc glutWMCloseFunc
#endif
#else
#include <GL/freeglut.h>
#endif

// CUDA utilitie`


## I

### image {#image}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `load image (needed so we can get the width and height before we create the
    // window
    char *i`

### initCuda {#initcuda}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void initCuda()
{`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void initGL(int argc, char **argv)
{`

### initGLResources {#initglresources}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void initGLResources()
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void keyboard(unsigned char key, int /*x*/, int /*y*/)
{`


## L

### loadImageData {#loadimagedata}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void loadImageData(int argc, char **argv)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## P

### printHelp {#printhelp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void printHelp()
{`


## R

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void reshape(int x, int y)
{`

### runBenchmark {#runbenchmark}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `int runBenchmark(int argc, char **argv)
{`

### runSingleTest {#runsingletest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `int runSingleTest(char *ref_file, char *exec_path)
{`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void timerEvent(int value)
{`


## V

### varyEuclidean {#varyeuclidean}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/bilateralFilter/bilateralFilter.cpp](./bilateralFilter.cpp_docs.md)
- **Context**: `void varyEuclidean()
{`

