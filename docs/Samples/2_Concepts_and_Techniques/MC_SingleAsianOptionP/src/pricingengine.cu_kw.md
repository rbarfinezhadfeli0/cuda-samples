# Keywords: Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu
---

**Total Keywords**: 10

---

## A

### AsianOption {#asianoption}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `              const AsianOption<Real> *const option`


## P

### PricingEngine {#pricingengine}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `class PricingEngine`


## S

### SharedMemory {#sharedmemory}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `ad_block cta)
{
    SharedMemory<Real> sdata;

    /`


## C

### computeValue {#computevalue}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `__global__ void computeValue(`

### cudaDeviceProp {#cudadeviceprop}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `struct cudaDeviceProp`

### cudaFuncAttributes {#cudafuncattributes}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `struct cudaFuncAttributes`


## G

### generatePaths {#generatepaths}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `__global__ void generatePaths(`

### getPathStep {#getpathstep}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `double getPathStep(double &drift, double &diffusion, curandState &state)
{`


## I

### initRNG {#initrng}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `__global__ void initRNG(`


## R

### reduce_sum {#reducesum}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/MC_SingleAsianOptionP/src/pricingengine.cu](./pricingengine.cu_docs.md)
- **Context**: `Real reduce_sum(Real in, cg::thread_block cta)
{`

