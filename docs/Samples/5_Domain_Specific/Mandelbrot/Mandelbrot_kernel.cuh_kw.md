# Keywords: Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh
---

**Total Keywords**: 12

---

## A

### ABS {#abs}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `#define ABS(n) ((n) < 0 ? -(n) : (n))

// Double single functions based on DSFUN90 package:
// http:`


## B

### BLOCKDIM_X {#blockdimx}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `#define BLOCKDIM_X 16
#define BLOCKDIM_Y 16

#define ABS(n) ((n) < 0 ? -(n) : (n))

// Double single`

### BLOCKDIM_Y {#blockdimy}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `#define BLOCKDIM_Y 16

#define ABS(n) ((n) < 0 ? -(n) : (n))

// Double single functions based on DS`


## C

### CalcMandelbrot {#calcmandelbrot}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `int
CalcMandelbrot(const T xPos, const T yPos, const T xJParam, const T yJParam, const int crunch, c`

### CalcMandelbrotDS {#calcmandelbrotds}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `int CalcMandelbrotDS(const float xPos0,
                                       const float xPos1,
  `

### CheckColors {#checkcolors}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `int CheckColors(const uchar4 &color0, const uchar4 &color1)
{`


## D

### dsadd {#dsadd}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `void dsadd(float &c0, float &c1, const float a0, const float a1, const float b0, const float b1)
{`

### dsdeq {#dsdeq}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `void dsdeq(float &a0, float &a1, double b)
{`

### dsfeq {#dsfeq}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `void dsfeq(float &a0, float &a1, float b)
{`

### dsmul {#dsmul}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `void dsmul(float &c0, float &c1, const float a0, const float a1, const float b0, const float b1)
{`

### dssub {#dssub}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `void dssub(float &c0, float &c1, const float a0, const float a1, const float b0, const float b1)
{`


## I

### iDivUp {#idivup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/Mandelbrot/Mandelbrot_kernel.cuh](./Mandelbrot_kernel.cuh_docs.md)
- **Context**: `int iDivUp(int a, int b) {`

