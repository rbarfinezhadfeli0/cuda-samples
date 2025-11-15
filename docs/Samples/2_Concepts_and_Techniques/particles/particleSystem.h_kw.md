# Keywords: Samples/2_Concepts_and_Techniques/particles/particleSystem.h
---

**Total Keywords**: 25

---

## D

### DEBUG_GRID {#debuggrid}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `#define DEBUG_GRID 0
#define DO_TIMING  0

#include <helper_functions.h>

#include "particles_kernel`

### DO_TIMING {#dotiming}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `#define DO_TIMING  0

#include <helper_functions.h>

#include "particles_kernel.cuh"
#include "vecto`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `ource;   // handles OpenGL-CUDA exchange
    s`


## P

### ParticleArray {#particlearray}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `ONFIGS };

    enum ParticleArray {
        POSITION,`

### ParticleConfig {#particleconfig}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `System();

    enum ParticleConfig { CONFIG_RANDOM, CO`

### ParticleSystem {#particlesystem}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `methods
    ParticleSystem() {`


## S

### SimParams {#simparams}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `

    // params
    SimParams m_params;
    uint3`

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `_numGridCells;

    StopWatchInterface *m_timer;

    uint`


## _

### __PARTICLESYSTEM_H__ {#particlesystemh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `#define __PARTICLESYSTEM_H__

#define DEBUG_GRID 0
#define DO_TIMING  0

#include <helper_functions.`


## C

### class {#class}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `class
class`

### cudaGraphicsResource {#cudagraphicsresource}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `struct cudaGraphicsResource`


## G

### getCellSize {#getcellsize}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `float3 getCellSize() {`

### getColliderPos {#getcolliderpos}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `float3 getColliderPos() {`

### getColliderRadius {#getcolliderradius}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `float  getColliderRadius() {`

### getGridSize {#getgridsize}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `uint3  getGridSize() {`

### getParticleRadius {#getparticleradius}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `float  getParticleRadius() {`

### getWorldOrigin {#getworldorigin}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `float3 getWorldOrigin() {`


## S

### setCollideAttraction {#setcollideattraction}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setCollideAttraction(float x) {`

### setCollideDamping {#setcollidedamping}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setCollideDamping(float x) {`

### setCollideShear {#setcollideshear}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setCollideShear(float x) {`

### setCollideSpring {#setcollidespring}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setCollideSpring(float x) {`

### setColliderPos {#setcolliderpos}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setColliderPos(float3 x) {`

### setDamping {#setdamping}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setDamping(float x) {`

### setGravity {#setgravity}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setGravity(float x) {`

### setIterations {#setiterations}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/particles/particleSystem.h](./particleSystem.h_docs.md)
- **Context**: `void setIterations(int i) {`

