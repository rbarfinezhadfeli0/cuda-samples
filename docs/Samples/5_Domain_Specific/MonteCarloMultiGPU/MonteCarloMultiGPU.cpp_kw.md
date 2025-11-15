# Keywords: Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp
---

**Total Keywords**: 18

---

## B

### BlackScholesCall {#blackscholescall}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `ons
extern "C" void BlackScholesCall(float &CallResult, `


## C

### CallResult {#callresult}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `kScholesCall(float &CallResult, TOptionData option`


## D

### DO_CPU {#docpu}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `#define DO_CPU
#undef DO_CPU

#define PRINT_RESULTS
#undef PRINT_RESULTS

void usage()
{
    printf(`


## M

### MonteCarlo {#montecarlo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `main(): running CPU MonteCarlo...\n");
    TOption`

### MonteCarloCPU {#montecarlocpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `///
extern "C" void MonteCarloCPU(TOptionValue &callV`

### MonteCarloGPU {#montecarlogpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `ain computation
    MonteCarloGPU(plan);

    checkCu`

### MonteCarloMultiGPU {#montecarlomultigpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `serve;

    printf("MonteCarloMultiGPU\n");
    printf("==`

### MonteCarlo_common {#montecarlocommon}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `ading.h>

#include "MonteCarlo_common.h"

int   *pArgc = `


## P

### PRINT_RESULTS {#printresults}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `#define PRINT_RESULTS
#undef PRINT_RESULTS

void usage()
{
    printf("--method=[threaded,streamed] `


## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `//////////
// Timer
StopWatchInterface **hTimer = NULL;

s`


## A

### adjustGridSize {#adjustgridsize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `int adjustGridSize(int GPUIndex, int defaultGridSize)
{`

### adjustProblemSize {#adjustproblemsize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `int adjustProblemSize(int GPU_N, int default_nOptions)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### multiSolver {#multisolver}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `void multiSolver(TOptionPlan *plan, int nPlans)
{`


## R

### randFloat {#randfloat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `float randFloat(float low, float high)
{`


## S

### solverThread {#solverthread}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `CUT_THREADPROC solverThread(TOptionPlan *plan)
{`

### strcasecmp {#strcasecmp}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `#define strcasecmp _strcmpi
#endif

////////////////////////////////////////////////////////////////`


## U

### usage {#usage}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp](./MonteCarloMultiGPU.cpp_docs.md)
- **Context**: `void usage()
{`

