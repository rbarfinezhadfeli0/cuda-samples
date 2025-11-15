# Keywords: Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp
---

**Total Keywords**: 36

---

## F

### FilterKernel_init {#filterkernelinit}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void FilterKernel_init()
{`

### FilterKernel_update {#filterkernelupdate}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void FilterKernel_update(float blurfactor)
{`


## M

### MAX {#max}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `#define MAX(a, b) ((a > b) ? a : b)

///////////////////////////////////////////////////////////////`

### MAX_EPSILON_ERROR {#maxepsilonerror}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `#define MAX_EPSILON_ERROR 5.00f
#define THRESHOLD         0.30f

const char *sSDKsample = "CUDA 3D V`


## N

### NumDevsUsed {#numdevsused}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `, Size = %u Texels, NumDevsUsed = %u, Workgroup "
 `


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `k-to-front.
 */

// OpenGL Graphics includes
#`


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `ntegrated   = true;
StopWatchInterface *animationTimer  = `


## T

### THRESHOLD {#threshold}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `#define THRESHOLD         0.30f

const char *sSDKsample = "CUDA 3D Volume Filtering";


#include "vo`


## V

### VolumeFilter_runFilter {#volumefilterrunfilter}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `ume *volumeRender = VolumeFilter_runFilter(
        &volumeOri`

### VolumeRender {#volumerender}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: ` rendering path for VolumeRender
        glutDisplay`

### VolumeRender_copyInvViewMatrix {#volumerendercopyinvviewmatrix}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `oid render()
{

    VolumeRender_copyInvViewMatrix(invViewMatrix, size`

### VolumeRender_deinit {#volumerenderdeinit}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `volumeFilter1);
    VolumeRender_deinit();

    if (pbo) {
`

### VolumeRender_init {#volumerenderinit}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `Size, NULL, 1);
    VolumeRender_init();
    VolumeRender`

### VolumeRender_render {#volumerenderrender}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: ` results to PBO
    VolumeRender_render(gridSize,
         `

### VolumeRender_setPreIntegrated {#volumerendersetpreintegrated}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `Integrated;
        VolumeRender_setPreIntegrated(preIntegrated);
   `

### VolumeRender_setTextureFilterMode {#volumerendersettexturefiltermode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `rFiltering;
        VolumeRender_setTextureFilterMode(linearFiltering, &v`

### VolumeType {#volumetype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `Size.depth * sizeof(VolumeType);
    void  *h_volu`


## C

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void cleanup()
{`

### computeFPS {#computefps}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void computeFPS()
{`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `struct cudaGraphicsResource`


## D

### display {#display}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void display()
{`


## F

### filter {#filter}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void filter()
{`


## G

### glutCloseFunc {#glutclosefunc}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `#define glutCloseFunc glutWMCloseFunc
#endif
#else
#include <GL/freeglut.h>
#endif

// CUDA Runtime `


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `int iDivUp(int a, int b) {`

### idle {#idle}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void idle() {`

### initData {#initdata}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void initData(int argc, char **argv)
{`

### initGL {#initgl}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void initGL(int *argc, char **argv)
{`

### initPixelBuffer {#initpixelbuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void initPixelBuffer()
{`


## K

### keyboard {#keyboard}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void keyboard(unsigned char key, int x, int y)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### motion {#motion}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void motion(int x, int y)
{`

### mouse {#mouse}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void mouse(int button, int state, int x, int y)
{`


## P

### printHelp {#printhelp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void printHelp()
{`


## R

### render {#render}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void render()
{`

### reshape {#reshape}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void       reshape(int w, int h)
{`

### runSingleTest {#runsingletest}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/volumeFiltering/volumeFiltering.cpp](./volumeFiltering.cpp_docs.md)
- **Context**: `void runSingleTest(const char *ref_file, const char *exec_path)
{`

