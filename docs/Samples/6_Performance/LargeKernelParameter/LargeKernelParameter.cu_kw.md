# Keywords: Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu
---

**Total Keywords**: 8

---

## C

### CONST_COPIED_PARAMS {#constcopiedparams}

- **Type**: macro
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `#define CONST_COPIED_PARAMS (TOTAL_PARAMS - KERNEL_PARAM_LIMIT)

__constant__ int excess_params[CONS`


## K

### KERNEL_PARAM_LIMIT {#kernelparamlimit}

- **Type**: macro
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `#define KERNEL_PARAM_LIMIT  (1024) // ints
#define CONST_COPIED_PARAMS (TOTAL_PARAMS - KERNEL_PARAM_`


## T

### TEST_ITERATIONS {#testiterations}

- **Type**: macro
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `#define TEST_ITERATIONS     (1000)
#define TOTAL_PARAMS        (8000) // ints
#define KERNEL_PARAM_L`

### TOTAL_PARAMS {#totalparams}

- **Type**: macro
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `#define TOTAL_PARAMS        (8000) // ints
#define KERNEL_PARAM_LIMIT  (1024) // ints
#define CONST_`


## K

### kernelDefault {#kerneldefault}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `__global__ void kernelDefault(`

### kernelLargeParam {#kernellargeparam}

- **Type**: cuda_kernel
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `__global__ void kernelLargeParam(`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `int main()
{`


## R

### report_time {#reporttime}

- **Type**: function
- **File**: [Samples/6_Performance/LargeKernelParameter/LargeKernelParameter.cu](./LargeKernelParameter.cu_docs.md)
- **Context**: `void report_time(std::chrono::time_point<std::chrono::steady_clock> start,
                        s`

