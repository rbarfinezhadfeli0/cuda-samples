# Keywords: Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h
---

**Total Keywords**: 26

---

## D

### DisplayMode {#displaymode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `nderer();

    enum DisplayMode { POINTS, SPRITES, `


## F

### FramebufferObject {#framebufferobject}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `htDepthTexture;
    FramebufferObject *m_lightFbo;

    G`


## S

### SMOKE_RENDERER_H {#smokerendererh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `#define SMOKE_RENDERER_H

#include "GLSLProgram.h"
#include "framebufferObject.h"
#include "nvMath.h`

### SmokeRenderer {#smokerenderer}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `class SmokeRenderer`


## G

### getLightPositionEyeSpace {#getlightpositioneyespace}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `vec4f    getLightPositionEyeSpace() {`

### getShadowMatrix {#getshadowmatrix}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `matrix4f getShadowMatrix() {`

### getShadowTexture {#getshadowtexture}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `GLuint getShadowTexture() {`

### getSortVector {#getsortvector}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `vec3f getSortVector() {`


## S

### setAlpha {#setalpha}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setAlpha(float x) {`

### setBlurRadius {#setblurradius}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setBlurRadius(float x) {`

### setColorAttenuation {#setcolorattenuation}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setColorAttenuation(vec3f c) {`

### setColorBuffer {#setcolorbuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setColorBuffer(GLuint vbo) {`

### setDisplayLightBuffer {#setdisplaylightbuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setDisplayLightBuffer(bool b) {`

### setDisplayMode {#setdisplaymode}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setDisplayMode(DisplayMode mode) {`

### setDoBlur {#setdoblur}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setDoBlur(bool b) {`

### setFOV {#setfov}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setFOV(float fov) {`

### setIndexBuffer {#setindexbuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setIndexBuffer(GLuint ib) {`

### setLightPosition {#setlightposition}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setLightPosition(vec3f v) {`

### setLightTarget {#setlighttarget}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setLightTarget(vec3f v) {`

### setNumDisplayedSlices {#setnumdisplayedslices}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setNumDisplayedSlices(int x) {`

### setNumParticles {#setnumparticles}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setNumParticles(unsigned int x) {`

### setNumSlices {#setnumslices}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setNumSlices(int x) {`

### setParticleRadius {#setparticleradius}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setParticleRadius(float x) {`

### setPositionBuffer {#setpositionbuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setPositionBuffer(GLuint vbo) {`

### setShadowAlpha {#setshadowalpha}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setShadowAlpha(float x) {`

### setVelocityBuffer {#setvelocitybuffer}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/smokeParticles/SmokeRenderer.h](./SmokeRenderer.h_docs.md)
- **Context**: `void setVelocityBuffer(GLuint vbo) {`

