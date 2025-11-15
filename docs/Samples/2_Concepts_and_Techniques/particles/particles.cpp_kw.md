# Keywords: Samples/2_Concepts_and_Techniques/particles/particles.cpp
---

**Total Keywords**: 34

---

## A

### AddParam {#addparam}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `");
        params->AddParam(new Param<float>("t`


## D

### DisplayMode {#displaymode}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `;
ParticleRenderer::DisplayMode displayMode        `


## G

### GRID_SIZE {#gridsize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `#define GRID_SIZE     64
#define NUM_PARTICLES 16384

const uint width = 640, height = 480;

// view`


## M

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 5.00f
#define THRESHOLD         0.30f

#define GRID_SIZE     64
#define NU`


## N

### NUM_PARTICLES {#numparticles}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `#define NUM_PARTICLES 16384

const uint width = 640, height = 480;

// view params
int              `

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `        "particles, NumDevsUsed = %u, Workgroup = %`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `rence value.
*/

// OpenGL Graphics includes
#`


## P

### ParamListGL {#paramlistgl}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `oat modelView[16];

ParamListGL *params;

// Auto-V`

### ParticleRenderer {#particlerenderer}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `            = 0.1f;
ParticleRenderer::DisplayMode displa`

### ParticleSystem {#particlesystem}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `Attraction = 0.0f;

ParticleSystem *psystem = 0;

// f`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `      fpsLimit = 1;
StopWatchInterface *timer    = NULL;

`


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `#define THRESHOLD         0.30f

#define GRID_SIZE     64
#define NUM_PARTICLES 16384

const uint wi`


## A

### addSphere {#addsphere}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void addSphere()
{`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void cleanup()
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void computeFPS()
{`


## D

### display {#display}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void display()
{`


## F

### frand {#frand}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `float frand() {`


## G

### glutCloseFunc {#glutclosefunc}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `#define glutCloseFunc glutWMCloseFunc
#endif
#else
#include <GL/freeglut.h>
#endif

// CUDA runtime
`


## I

### idle {#idle}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void idle(void)
{`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void initGL(int *argc, char **argv)
{`

### initMenus {#initmenus}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void initMenus()
{`

### initParams {#initparams}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void initParams()
{`

### initParticleSystem {#initparticlesystem}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void initParticleSystem(int numParticles, uint3 gridSize, bool bUseOpenGL)
{`

### ixform {#ixform}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void ixform(float *v, float *r, GLfloat *m)
{`

### ixformPoint {#ixformpoint}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void ixformPoint(float *v, float *r, GLfloat *m)
{`


## K

### key {#key}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void key(unsigned char key, int /*x*/, int /*y*/)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### mainMenu {#mainmenu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void mainMenu(int i) {`

### motion {#motion}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void motion(int x, int y)
{`

### mouse {#mouse}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void mouse(int button, int state, int x, int y)
{`


## R

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void reshape(int w, int h)
{`

### runBenchmark {#runbenchmark}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void runBenchmark(int iterations, char *exec_path)
{`


## S

### special {#special}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void special(int k, int x, int y)
{`


## X

### xform {#xform}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particles.cpp](./particles.cpp_docs.md)
- **Context**: `void xform(float *v, float *r, GLfloat *m)
{`

