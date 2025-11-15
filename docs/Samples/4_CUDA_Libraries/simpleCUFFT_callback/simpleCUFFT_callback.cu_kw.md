# Keywords: Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu
---

**Total Keywords**: 12

---

## C

### ComplexAdd {#complexadd}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `Complex ComplexAdd(Complex a, Complex b)
{`

### ComplexMul {#complexmul}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `Complex ComplexMul(Complex a, Complex b)
{`

### ComplexPointwiseMulAndScale {#complexpointwisemulandscale}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `cufftComplex ComplexPointwiseMulAndScale(void *a, size_t index, void *cb_info, void *sharedmem)
{`

### ComplexScale {#complexscale}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `Complex ComplexScale(Complex a, float s)
{`

### Convolve {#convolve}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `void Convolve(const Complex *signal,
              int            signal_size,
              const C`


## F

### FILTER_KERNEL_SIZE {#filterkernelsize}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `#define FILTER_KERNEL_SIZE 11

/////////////////////////////////////////////////////////////////////`


## P

### PadData {#paddata}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `int PadData(const Complex *signal,
            Complex      **padded_signal,
            int        `


## S

### SIGNAL_SIZE {#signalsize}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `#define SIGNAL_SIZE        50
#define FILTER_KERNEL_SIZE 11

///////////////////////////////////////`


## _

### _cb_params {#cbparams}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `struct _cb_params`


## C

### cudaDeviceProp {#cudadeviceprop}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `struct cudaDeviceProp`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## R

### runTest {#runtest}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/simpleCUFFT_callback/simpleCUFFT_callback.cu](./simpleCUFFT_callback.cu_docs.md)
- **Context**: `int runTest(int argc, char **argv)
{`

