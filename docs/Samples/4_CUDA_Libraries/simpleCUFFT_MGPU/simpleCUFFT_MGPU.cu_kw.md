# Keywords: Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu
---

**Total Keywords**: 9

---

## C

### ComplexAdd {#complexadd}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `Complex ComplexAdd(Complex a, Complex b)
{`

### ComplexMul {#complexmul}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `Complex ComplexMul(Complex a, Complex b)
{`

### ComplexPointwiseMulAndScale {#complexpointwisemulandscale}

- **Type**: cuda_kernel
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `__global__ void ComplexPointwiseMulAndScale(`

### ComplexScale {#complexscale}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `Complex ComplexScale(Complex a, float s)
{`

### Convolve {#convolve}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `void Convolve(const Complex *signal,
              int            signal_size,
              const C`


## P

### PadData {#paddata}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `int PadData(const Complex *signal,
            Complex      **padded_signal,
            int        `


## T

### TestResult {#testresult}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: ` printf("\nvalue of TestResult %d\n", bTestResult)`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### multiplyCoefficient {#multiplycoefficient}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_MGPU/simpleCUFFT_MGPU.cu](./simpleCUFFT_MGPU.cu_docs.md)
- **Context**: `void multiplyCoefficient(cudaLibXtDesc *d_signal, cudaLibXtDesc *d_filter_kernel, int new_size, floa`

