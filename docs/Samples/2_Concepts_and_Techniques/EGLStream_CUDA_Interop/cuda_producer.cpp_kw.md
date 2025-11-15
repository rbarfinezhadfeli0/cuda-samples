# Keywords: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp
---

**Total Keywords**: 12

---

## C

### CudaProducer {#cudaproducer}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `) {
        printf("CudaProducer: Error opening file`


## N

### NumChannels {#numchannels}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `FACE_LDST;
    desc.NumChannels = 4;
    desc.Width`


## R

### ReadARGBFrame {#readargbframe}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `            printf("ReadARGBFrame: file read to the e`

### ReadYUVFrame {#readyuvframe}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `) {
        printf("ReadYUVFrame: Error seeking file`


## T

### TestArgs {#testargs}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `treamKHR eglStream, TestArgs *args)
{
    cudaPr`


## W

### WidthInBytes {#widthinbytes}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `
            cpdesc.WidthInBytes                    `


## C

### cudaDeviceCreateProducer {#cudadevicecreateproducer}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaDeviceCreateProducer(test_cuda_producer_s *cudaProducer, CUdevice device)
{`

### cudaProducerDeinit {#cudaproducerdeinit}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerDeinit(test_cuda_producer_s *cudaProducer)
{`

### cudaProducerInit {#cudaproducerinit}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `void cudaProducerInit(test_cuda_producer_s *cudaProducer, EGLDisplay eglDisplay, EGLStreamKHR eglStr`

### cudaProducerReadARGBFrame {#cudaproducerreadargbframe}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerReadARGBFrame(FILE          *file,
                                          un`

### cudaProducerReadYUVFrame {#cudaproducerreadyuvframe}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerReadYUVFrame(FILE          *file,
                                         unsi`

### cudaProducerTest {#cudaproducertest}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_Interop/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerTest(test_cuda_producer_s *cudaProducer, char *file)
{`

