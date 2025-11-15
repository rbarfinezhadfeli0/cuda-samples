# Keywords: Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu
---

**Total Keywords**: 11

---

## M

### MAX_OPTIONS {#maxoptions}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `#define MAX_OPTIONS (1024 * 1024)

// Preprocessed input option data
typedef struct
{
    real S;
  `

### MonteCarloGPU {#montecarlogpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `void MonteCarloGPU(TOptionPlan *plan, cudaStream_t stream)
{`

### MonteCarloOneBlockPerOption {#montecarlooneblockperoption}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `__global__ void MonteCarloOneBlockPerOption(`

### MonteCarlo_common {#montecarlocommon}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `_cuda.h>

#include "MonteCarlo_common.h"

///////////////`

### MonteCarlo_reduction {#montecarloreduction}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `/////////
#include "MonteCarlo_reduction.cuh"

/////////////`

### MuByT {#mubyt}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `   real X;
    real MuByT;
    real VBySqrtT;`


## T

### THREAD_N {#threadn}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `#define THREAD_N 256

//////////////////////////////////////////////////////////////////////////////`


## C

### closeMonteCarloGPU {#closemontecarlogpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `void closeMonteCarloGPU(TOptionPlan *plan)
{`


## E

### endCallValue {#endcallvalue}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `double endCallValue(double S, double X, double r, double MuByT, double VBySqrtT)
{`


## I

### initMonteCarloGPU {#initmontecarlogpu}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `void initMonteCarloGPU(TOptionPlan *plan)
{`


## R

### rngSetupStates {#rngsetupstates}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.cu](./MonteCarlo_kernel.cu_docs.md)
- **Context**: `__global__ void rngSetupStates(`

