# Keywords: Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu
---

**Total Keywords**: 9

---

## C

### checkConsumerDataGPU {#checkconsumerdatagpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `void checkConsumerDataGPU(char *data, int size, char expectedVal, int frameNumber)
{`

### checkProducerDataGPU {#checkproducerdatagpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `void                checkProducerDataGPU(char *data, int size, char expectedVal, int frameNumber)
{`

### cudaConsumer_filter {#cudaconsumerfilter}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `cudaError_t cudaConsumer_filter(cudaStream_t cStream,
                                char        *p`

### cudaGetValueMismatch {#cudagetvaluemismatch}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `cudaError_t cudaGetValueMismatch()
{`

### cudaProducer_filter {#cudaproducerfilter}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `cudaError_t cudaProducer_filter(cudaStream_t pStream,
                                char        *p`


## G

### getNumErrors {#getnumerrors}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `__global__ void getNumErrors(`


## T

### testKernelConsumer {#testkernelconsumer}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `__global__ void testKernelConsumer(`

### testKernelProducer {#testkernelproducer}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `__global__ void testKernelProducer(`


## W

### writeDataToBuffer {#writedatatobuffer}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/EGLStream_CUDA_CrossGPU/kernel.cu](./kernel.cu_docs.md)
- **Context**: `__global__ void writeDataToBuffer(`

