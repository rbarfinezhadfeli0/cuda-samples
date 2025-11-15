# Keywords: Samples/2_Concepts_and_Techniques/interval/cpu_interval.h
---

**Total Keywords**: 18

---

## C

### CPU_INTERVAL_H {#cpuintervalh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `#define CPU_INTERVAL_H

#ifndef __USE_ISOC99
#define __USE_ISOC99
#endif

#include <boost/numeric/in`


## U

### UNPROTECTED {#unprotected}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `#define UNPROTECTED       0
#define USE_RECURSION_CPU 1

using boost::numeric::interval;
using names`

### USE_RECURSION_CPU {#userecursioncpu}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `#define USE_RECURSION_CPU 1

using boost::numeric::interval;
using namespace boost::numeric;

templa`


## _

### __USE_ISOC99 {#useisoc99}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `#define __USE_ISOC99
#endif

#include <boost/numeric/interval.hpp>
#include <iostream>
#include <vec`


## C

### checkAgainstHost {#checkagainsthost}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `bool checkAgainstHost(int *h_nresults, int *h_nresults_cpu, I_CPU *h_result, I_CPU *h_result_cpu)
{`


## E

### empty {#empty}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `bool empty() {`


## F

### f_cpu {#fcpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `I f_cpu(I const &x, int thread_id)
{`

### fd_cpu {#fdcpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `I fd_cpu(I const &x, int thread_id)
{`

### full {#full}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `bool full() {`


## G

### global_stack_cpu {#globalstackcpu}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `class global_stack_cpu`


## I

### is_minimal_cpu {#isminimalcpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `bool is_minimal_cpu(I const &x, int thread_id)
{`


## N

### newton_interval_cpu {#newtonintervalcpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `void newton_interval_cpu(global_stack_cpu<I, DEPTH_RESULT, THREADS> &result, I const &ix0, int threa`

### newton_interval_rec_cpu {#newtonintervalreccpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `void newton_interval_rec_cpu(global_stack_cpu<I, DEPTH_RESULT, THREADS> &result, I const &ix, int th`


## P

### pop {#pop}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `T pop()
    {`

### push {#push}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `void push(T const &v)
    {`


## S

### should_bisect_cpu {#shouldbisectcpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `bool should_bisect_cpu(I const &x, I const &x1, I const &x2, typename I::base_type alpha)
{`

### size {#size}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `int  size() {`


## T

### test_interval_newton_cpu {#testintervalnewtoncpu}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/interval/cpu_interval.h](./cpu_interval.h_docs.md)
- **Context**: `void test_interval_newton_cpu(I *buffer, int *nresults, I i)
{`

