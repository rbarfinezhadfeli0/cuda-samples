# Keywords: Samples/2_Concepts_and_Techniques/interval/cuda_interval.h
---

**Total Keywords**: 14

---

## C

### CUDA_INTERVAL_H {#cudaintervalh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `#define CUDA_INTERVAL_H

#include "cuda_interval_lib.h"
#include "interval.h"

// Stack in local mem`


## E

### empty {#empty}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `bool empty() {`


## F

### full {#full}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `bool full() {`


## G

### global_stack {#globalstack}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `class global_stack`


## I

### is_minimal {#isminimal}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `bool is_minimal(interval_gpu<T> const &x, int thread_id)
{`


## L

### local_stack {#localstack}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `class local_stack`


## N

### newton_interval {#newtoninterval}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `void
newton_interval(global_stack<interval_gpu<T>, DEPTH_RESULT, THREADS> &result, interval_gpu<T> c`

### newton_interval_naive {#newtonintervalnaive}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `void newton_interval_naive(global_stack<interval_gpu<T>, DEPTH_RESULT, THREADS> &result,
           `

### newton_interval_rec {#newtonintervalrec}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `void newton_interval_rec(global_stack<interval_gpu<T>, DEPTH_RESULT, THREADS> &result,
             `


## P

### pop {#pop}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `T pop()
    {`

### push {#push}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `void push(T const &v)
    {`


## S

### should_bisect {#shouldbisect}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `bool should_bisect(interval_gpu<T> const &x, interval_gpu<T> const &x1, interval_gpu<T> const &x2, T`

### size {#size}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `int  size() {`


## T

### test_interval_newton {#testintervalnewton}

- **Type**: cuda_kernel
- **File**: [Samples/2_Concepts_and_Techniques/interval/cuda_interval.h](./cuda_interval.h_docs.md)
- **Context**: `__global__ void
test_interval_newton(`

