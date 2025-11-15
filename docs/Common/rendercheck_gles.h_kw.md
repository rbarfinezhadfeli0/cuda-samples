# Keywords: Common/rendercheck_gles.h
---

**Total Keywords**: 47

---

## B

### BUFFER_OFFSET {#bufferoffset}

- **Type**: macro
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `#define BUFFER_OFFSET(i) ((char *)NULL + (i))

#if _DEBUG
#define CHECK_FBO     checkStatus(__FILE__`

### BackBuffer {#backbuffer}

- **Type**: identifier
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `e readback BLT from BackBuffer->PBO->membuf
      `


## C

### CFrameBufferObject {#cframebufferobject}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `class CFrameBufferObject`

### CHECK_FBO {#checkfbo}

- **Type**: macro
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `#define CHECK_FBO     true
#endif

class CheckRender {
 public:
  CheckRender(unsigned int width, un`

### CheckBackBuffer {#checkbackbuffer}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `class CheckBackBuffer`

### CheckFBO {#checkfbo}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `class CheckFBO`

### CheckRender {#checkrender}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `class CheckRender`


## E

### EnableQAReadback {#enableqareadback}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void EnableQAReadback(bool bStatus) {`


## F

### FrameBuffer {#framebuffer}

- **Type**: identifier
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `}

  // bind to the FrameBuffer Object
  void bindR`


## I

### IsFBO {#isfbo}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool IsFBO() {`

### IsPBO {#ispbo}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool IsPBO() {`

### IsQAReadback {#isqareadback}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool IsQAReadback() {`


## P

### PGMvsPGM {#pgmvspgm}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool PGMvsPGM(const char *src_file, const char *ref_file,
                        const float epsilo`

### PPMvsPPM {#ppmvsppm}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool PPMvsPPM(const char *src_file, const char *ref_file,
                        const float epsilo`


## _

### _RENDERCHECK_GLES_H_ {#rendercheckglesh}

- **Type**: macro
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `#define _RENDERCHECK_GLES_H_

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <a`


## A

### allocateMemory {#allocatememory}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void allocateMemory(unsigned int width, unsigned int height,
                              unsigned `

### attachTexture {#attachtexture}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void attachTexture(GLenum texTarget, GLuint texId,
                     GLenum attachment = GL_COLOR`


## B

### bindFragmentProgram {#bindfragmentprogram}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void bindFragmentProgram(){`

### bindReadback {#bindreadback}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void bindReadback() {`

### bindRenderPath {#bindrenderpath}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void bindRenderPath() {`

### bindTexture {#bindtexture}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void bindTexture() {`

### bufferConfig {#bufferconfig}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `struct bufferConfig`


## C

### checkStatus {#checkstatus}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool checkStatus(const char *zfile, int line, bool silent) {`

### check_gl_error {#checkglerror}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void check_gl_error(const char *file, int line) {`

### compareBin2BinFloat {#comparebin2binfloat}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool compareBin2BinFloat(const char *src_file, const char *ref_file,
                               `

### compareBin2BinUint {#comparebin2binuint}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool compareBin2BinUint(const char *src_file, const char *ref_file,
                                `

### create {#create}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool create(GLuint width, GLuint height, fboConfig &config, fboData &data) {`

### createTexture {#createtexture}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `GLuint createTexture(GLenum target, int w, int h, GLint internalformat,
                       GLenu`


## D

### dumpBin {#dumpbin}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void dumpBin(void *data, unsigned int bytes, const char *filename) {`


## F

### fboConfig {#fboconfig}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `struct fboConfig`

### fboData {#fbodata}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `struct fboData`

### freeResources {#freeresources}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void freeResources() {`

### functions {#functions}

- **Type**: type
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `class functions`


## G

### getDepthTex {#getdepthtex}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `GLuint getDepthTex() {`

### getFbo {#getfbo}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `GLuint getFbo() {`

### getPixelFormat {#getpixelformat}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `int getPixelFormat() {`

### getTex {#gettex}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `GLuint getTex() {`


## I

### initialize {#initialize}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool initialize(unsigned width, unsigned height, fboConfig &rConfigFBO,
                  fboData &r`


## R

### readback {#readback}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `bool readback(GLuint width, GLuint height, unsigned char *memBuf) {`


## S

### savePGM {#savepgm}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void savePGM(const char *zfilename, bool bInvert, void **ppReadBuf) {`

### savePPM {#saveppm}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void savePPM(const char *zfilename, bool bInvert, void **ppReadBuf) {`

### setExecPath {#setexecpath}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void setExecPath(char *path) {`

### setPixelFormat {#setpixelformat}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void setPixelFormat(GLenum format) {`

### setThresholdCompare {#setthresholdcompare}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void setThresholdCompare(float value) {`


## U

### unbindReadback {#unbindreadback}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void unbindReadback() {`

### unbindRenderPath {#unbindrenderpath}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void unbindRenderPath() {`

### unbindTexture {#unbindtexture}

- **Type**: function
- **File**: [Common/rendercheck_gles.h](./rendercheck_gles.h_docs.md)
- **Context**: `void unbindTexture() {`

