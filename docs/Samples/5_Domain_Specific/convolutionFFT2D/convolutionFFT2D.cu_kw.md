# Keywords: Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu
---

**Total Keywords**: 6

---

## M

### modulateAndNormalize {#modulateandnormalize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu](./convolutionFFT2D.cu_docs.md)
- **Context**: `void modulateAndNormalize(fComplex *d_Dst, fComplex *d_Src, int fftH, int fftW, int padding)
{`


## P

### padDataClampToBorder {#paddataclamptoborder}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu](./convolutionFFT2D.cu_docs.md)
- **Context**: `void padDataClampToBorder(float *d_Dst,
                                     float *d_Src,
         `

### padKernel {#padkernel}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu](./convolutionFFT2D.cu_docs.md)
- **Context**: `void
padKernel(float *d_Dst, float *d_Src, int fftH, int fftW, int kernelH, int kernelW, int kernelY`


## S

### spPostprocess2D {#sppostprocess2d}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu](./convolutionFFT2D.cu_docs.md)
- **Context**: `void spPostprocess2D(void *d_Dst, void *d_Src, uint DY, uint DX, uint padding, int dir)
{`

### spPreprocess2D {#sppreprocess2d}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu](./convolutionFFT2D.cu_docs.md)
- **Context**: `void spPreprocess2D(void *d_Dst, void *d_Src, uint DY, uint DX, uint padding, int dir)
{`

### spProcess2D {#spprocess2d}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/convolutionFFT2D/convolutionFFT2D.cu](./convolutionFFT2D.cu_docs.md)
- **Context**: `void spProcess2D(void *d_Dst, void *d_SrcA, void *d_SrcB, uint DY, uint DX, int dir)
{`

