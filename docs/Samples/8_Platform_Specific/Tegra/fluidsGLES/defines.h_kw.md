# Keywords: Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h
---

**Total Keywords**: 11

---

## C

### CPADW {#cpadw}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define CPADW (DIM / 2 + 1)       // Padded width for real->complex in-place FFT
#define RPADW (2 * `


## D

### DEFINES_H {#definesh}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define DEFINES_H

#define DIM   512                 // Square size of solver domain
#define DS    (`

### DIM {#dim}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define DIM   512                 // Square size of solver domain
#define DS    (DIM * DIM)         `


## F

### FORCE {#force}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define FORCE (5.8f * DIM) // Force scale factor
#define FR    4            // Force update radius

`


## P

### PDS {#pds}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define PDS   (DIM * CPADW)       // Padded total domain size

#define DT    0.09f        // Delta T`


## R

### RPADW {#rpadw}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define RPADW (2 * (DIM / 2 + 1)) // Padded width for real->complex in-place FFT
#define PDS   (DIM `


## T

### TIDSX {#tidsx}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define TIDSX 64 // Tids in X
#define TIDSY 4  // Tids in Y

#endif
`

### TIDSY {#tidsy}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define TIDSY 4  // Tids in Y

#endif
`

### TILEX {#tilex}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define TILEX 64 // Tile width
#define TILEY 64 // Tile height
#define TIDSX 64 // Tids in X
#define`

### TILEY {#tiley}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define TILEY 64 // Tile height
#define TIDSX 64 // Tids in X
#define TIDSY 4  // Tids in Y

#endif
`


## V

### VIS {#vis}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/fluidsGLES/defines.h](./defines.h_docs.md)
- **Context**: `#define VIS   0.0025f      // Viscosity constant
#define FORCE (5.8f * DIM) // Force scale factor
#d`

