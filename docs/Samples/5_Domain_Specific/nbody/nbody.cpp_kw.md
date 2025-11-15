# Keywords: Samples/5_Domain_Specific/nbody/nbody.cpp
---

**Total Keywords**: 48

---

## A

### AddParam {#addparam}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `Size
    paramlist->AddParam(
        new Param<`


## B

### BodySystem {#bodysystem}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: ` *m_singleton;

    BodySystem<T>     *m_nbody;
  `

### BodySystemCPU {#bodysystemcpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `> *m_nbodyCuda;
    BodySystemCPU<T>  *m_nbodyCpu;

 `

### BodySystemCUDA {#bodysystemcuda}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `>     *m_nbody;
    BodySystemCUDA<T> *m_nbodyCuda;
  `


## C

### Create {#create}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void Create() {`


## D

### Destroy {#destroy}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void Destroy() {`

### DisplayMode {#displaymode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `

ParticleRenderer::DisplayMode displayMode = Parti`


## M

### MultiGPU {#multigpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `            printf("MultiGPU n-body requires CUD`


## N

### NBodyDemo {#nbodydemo}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `class NBodyDemo`

### NBodyParams {#nbodyparams}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `struct NBodyParams`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `el;
}

// check for OpenGL errors
inline void `

### OpenMP {#openmp}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `tion with CPU using OpenMP\n");
#else
        `


## P

### ParamListGL {#paramlistgl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `eDemo];

// The UI.
ParamListGL *paramlist; // para`

### ParticleRenderer {#particlerenderer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `           = 0.1f;

ParticleRenderer::DisplayMode displa`


## S

### SetBarColorInner {#setbarcolorinner}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `s");
    paramlist->SetBarColorInner(0.8f, 0.8f, 0.0f);
`

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `  = 10000.0f; // ms
StopWatchInterface *demoTimer = NULL, `


## _

### _compareResults {#compareresults}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `bool _compareResults(int numBodies)
    {`

### _init {#init}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void _init(int  numBodies,
               int  numDevices,
               int  blockSize,
          `

### _reset {#reset}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void _reset(int numBodies, NBodyConfig config)
    {`

### _resetRenderer {#resetrenderer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void _resetRenderer()
    {`

### _runBenchmark {#runbenchmark}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void _runBenchmark(int iterations)
    {`

### _selectDemo {#selectdemo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void _selectDemo(int index)
    {`


## C

### checkGLErrors {#checkglerrors}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void checkGLErrors(const char *s)
{`

### compareResults {#compareresults}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `bool compareResults(int numBodies) {`

### computePerfStats {#computeperfstats}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void computePerfStats(double &interactionsPerSecond, double &gflops, float milliseconds, int iterati`


## D

### display {#display}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void display()
{`

### displayNBodySystem {#displaynbodysystem}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void displayNBodySystem()
{`


## F

### finalize {#finalize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void finalize()
{`


## G

### getArrays {#getarrays}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void getArrays(T *pos, T *vel)
    {`


## I

### idle {#idle}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void idle(void) {`

### init {#init}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void init(int  numBodies,
                     int  numDevices,
                     int  blockSize,`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void initGL(int *argc, char **argv)
{`

### initParameters {#initparameters}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void initParameters()
{`


## K

### key {#key}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void key(unsigned char key, int /*x*/, int /*y*/)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### motion {#motion}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void motion(int x, int y)
{`

### mouse {#mouse}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void mouse(int button, int state, int x, int y)
{`


## P

### print {#print}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void print()
    {`


## R

### reset {#reset}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void reset(int numBodies, NBodyConfig config) {`

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void reshape(int w, int h)
{`

### runBenchmark {#runbenchmark}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void runBenchmark(int iterations) {`


## S

### selectDemo {#selectdemo}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void selectDemo(int activeDemo)
{`

### setArrays {#setarrays}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void setArrays(const T *pos, const T *vel)
    {`

### showHelp {#showhelp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void showHelp()
{`

### special {#special}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void special(int key, int x, int y)
{`

### switchDemoPrecision {#switchdemoprecision}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void switchDemoPrecision()
{`


## U

### updateParams {#updateparams}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void updateParams()
{`

### updateSimulation {#updatesimulation}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/nbody/nbody.cpp](./nbody.cpp_docs.md)
- **Context**: `void updateSimulation()
{`

