# Keywords: Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_reduction.cuh
---

**Total Keywords**: 2

---

## M

### MONTECARLO_REDUCTION_CUH {#montecarloreductioncuh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_reduction.cuh](./MonteCarlo_reduction.cuh_docs.md)
- **Context**: `#define MONTECARLO_REDUCTION_CUH

#include <cooperative_groups.h>

namespace cg = cooperative_groups`


## S

### sumReduce {#sumreduce}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_reduction.cuh](./MonteCarlo_reduction.cuh_docs.md)
- **Context**: `void
sumReduce(T *sum, T *sum2, cg::thread_block &cta, cg::thread_block_tile<32> &tile32, __TOptionV`

