# Keywords: Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu
---

**Total Keywords**: 5

---

## E

### ELEMS_PER_THREAD {#elemsperthread}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu](./binomialOptions_kernel.cu_docs.md)
- **Context**: `#define ELEMS_PER_THREAD (NUM_STEPS / THREADBLOCK_SIZE)
#if NUM_STEPS % THREADBLOCK_SIZE
#error Bad `


## T

### THREADBLOCK_SIZE {#threadblocksize}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu](./binomialOptions_kernel.cu_docs.md)
- **Context**: `#define THREADBLOCK_SIZE 128
#define ELEMS_PER_THREAD (NUM_STEPS / THREADBLOCK_SIZE)
#if NUM_STEPS %`


## B

### binomialOptionsGPU {#binomialoptionsgpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu](./binomialOptions_kernel.cu_docs.md)
- **Context**: `void binomialOptionsGPU(real *callValue, TOptionData *optionData, int optN)
{`

### binomialOptionsKernel {#binomialoptionskernel}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu](./binomialOptions_kernel.cu_docs.md)
- **Context**: `__global__ void binomialOptionsKernel(`


## E

### expiryCallValue {#expirycallvalue}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/binomialOptions/binomialOptions_kernel.cu](./binomialOptions_kernel.cu_docs.md)
- **Context**: `double expiryCallValue(double S, double X, double vDt, int i)
{`

