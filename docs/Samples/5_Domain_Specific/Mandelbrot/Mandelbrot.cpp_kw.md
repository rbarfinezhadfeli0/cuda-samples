# Keywords: Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp
---

**Total Keywords**: 45

---

## B

### BUFFER_DATA {#bufferdata}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define BUFFER_DATA(i) ((char *)0 + i)

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || d`


## E

### ExecutionTime {#executiontime}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `&hTimer);
    float ExecutionTime = sdkGetTimerValue(`


## G

### GetSample {#getsample}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void GetSample(int sampleIndex, float &x, float &y)
{`


## J

### JuliaSet {#juliaset}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `) {
        printf("JuliaSet: params.txt could n`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define MAX(a, b) ((a > b) ? a : b)
#endif
#define BUFFER_DATA(i) ((char *)0 + i)

#if defined(WIN32`

### MAX_EPSILON {#maxepsilon}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define MAX_EPSILON   50
#define REFRESH_DELAY 10 // ms

#ifndef MAX
#define MAX(a, b) ((a > b) ? a `

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 5.0f

// Define the files that are to be save and the reference images for`


## N

### NewTek {#newtek}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `ed by Mark Granger, NewTek

  CUDA 2.0 SDK - u`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `in,
  NVIDIA
*/

// OpenGL Graphics includes
#`


## P

### PixelsPerSecond {#pixelspersecond}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `_timer);

    float PixelsPerSecond = (float)imageW * (`


## R

### RANDOMBITS {#randombits}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define RANDOMBITS(seed, bits) ((unsigned int)RANDOMSEED(seed) >> (32 - (bits)))

// OpenGL PBO and `

### RANDOMSEED {#randomseed}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define RANDOMSEED(seed)       ((seed) = ((seed) * 1103515245 + 12345))
#define RANDOMBITS(seed, bit`

### REFRESH_DELAY {#refreshdelay}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define REFRESH_DELAY 10 // ms

#ifndef MAX
#define MAX(a, b) ((a > b) ? a : b)
#endif
#define BUFFE`

### RUN_CPU {#runcpu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define RUN_CPU 0

// Set to 1 to time frame generation
#define RUN_TIMING 0

// Random number macro`

### RUN_TIMING {#runtiming}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define RUN_TIMING 0

// Random number macros
#define RANDOMSEED(seed)       ((seed) = ((seed) * 110`

### RunMandelbrot0 {#runmandelbrot0}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `                    RunMandelbrot0(d_dst,
            `

### RunMandelbrot1 {#runmandelbrot1}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `                    RunMandelbrot1(d_dst,
            `

### RunMandelbrotDSGold0 {#runmandelbrotdsgold0}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `                    RunMandelbrotDSGold0(h_Src,
            `

### RunMandelbrotDSGold1 {#runmandelbrotdsgold1}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `                    RunMandelbrotDSGold1(h_Src,
            `

### RunMandelbrotGold0 {#runmandelbrotgold0}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `                    RunMandelbrotGold0(h_Src,
            `

### RunMandelbrotGold1 {#runmandelbrotgold1}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `                    RunMandelbrotGold1(h_Src,
            `


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `olors;

// Timer ID
StopWatchInterface *hTimer = NULL;

//`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void cleanup()
{`

### clickFunc {#clickfunc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void clickFunc(int button, int state, int x, int y)
{`

### compileASMShader {#compileasmshader}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `GLuint compileASMShader(GLenum program_type, const char *code)
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void computeFPS()
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### displayFunc {#displayfunc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void displayFunc(void)
{`


## G

### glutCloseFunc {#glutclosefunc}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `#define glutCloseFunc glutWMCloseFunc
#endif
#else
#include <GL/freeglut.h>
#endif

// CUDA runtime
`


## I

### initData {#initdata}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void initData(int argc, char **argv)
{`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void initGL(int *argc, char **argv)
{`

### initMenus {#initmenus}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void initMenus()
{`

### initOpenGLBuffers {#initopenglbuffers}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void initOpenGLBuffers(int w, int h)
{`


## K

### keyboardFunc {#keyboardfunc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void keyboardFunc(unsigned char k, int, int)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### mainMenu {#mainmenu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void mainMenu(int i)
{`

### motionFunc {#motionfunc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void motionFunc(int x, int y)
{`


## P

### printHelp {#printhelp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void printHelp()
{`


## R

### renderImage {#renderimage}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void renderImage(bool bUseOpenGL, bool fp64, int mode)
{`

### reshapeFunc {#reshapefunc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void reshapeFunc(int w, int h)
{`

### runBenchmark {#runbenchmark}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void runBenchmark(int argc, char **argv)
{`

### runSingleTest {#runsingletest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `int runSingleTest(int argc, char **argv)
{`


## S

### setVSync {#setvsync}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void setVSync(int interval)
{`

### startJulia {#startjulia}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void startJulia(const char *path)
{`


## T

### timerEvent {#timerevent}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot.cpp](./Mandelbrot.cpp_docs.md)
- **Context**: `void timerEvent(int value)
{`

