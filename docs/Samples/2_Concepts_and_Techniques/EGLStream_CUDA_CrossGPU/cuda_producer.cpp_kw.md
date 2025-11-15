# Keywords: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp
---

**Total Keywords**: 9

---

## T

### TestArgs {#testargs}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `er_s *cudaProducer, TestArgs *args)
{
    CUresu`


## C

### cudaDeviceCreateProducer {#cudadevicecreateproducer}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaDeviceCreateProducer(test_cuda_producer_s *cudaProducer)
{`

### cudaProducerDeinit {#cudaproducerdeinit}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerDeinit(test_cuda_producer_s *cudaProducer)
{`

### cudaProducerInit {#cudaproducerinit}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerInit(test_cuda_producer_s *cudaProducer, TestArgs *args)
{`

### cudaProducerPrepareFrame {#cudaproducerprepareframe}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `void cudaProducerPrepareFrame(CUeglFrame *cudaEgl, CUdeviceptr cudaPtr, int bufferSize)
{`

### cudaProducerPresentFrame {#cudaproducerpresentframe}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerPresentFrame(test_cuda_producer_s *cudaProducer, CUeglFrame cudaEgl, int t)
{`

### cudaProducerReturnFrame {#cudaproducerreturnframe}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `CUresult cudaProducerReturnFrame(test_cuda_producer_s *cudaProducer, CUeglFrame cudaEgl, int t)
{`


## P

### presentApiStat {#presentapistat}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `void presentApiStat(void)
{`


## T

### timespec {#timespec}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/cuda_producer.cpp](./cuda_producer.cpp_docs.md)
- **Context**: `struct timespec`

