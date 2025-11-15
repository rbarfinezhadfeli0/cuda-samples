# Keywords: Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu
---

**Total Keywords**: 12

---

## _

### _BISECT_UTIL_H_ {#bisectutilh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `#define _BISECT_UTIL_H_

#include <cooperative_groups.h>

namespace cg = cooperative_groups;

// inc`


## C

### ceilPow2 {#ceilpow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `int ceilPow2(int n)
{`

### compactIntervals {#compactintervals}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `void compactIntervals(float       *s_left,
                                 float       *s_right,
  `

### computeMidpoint {#computemidpoint}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `float computeMidpoint(const float left, const float right)
{`

### computeNumSmallerEigenvals {#computenumsmallereigenvals}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `int computeNumSmallerEigenvals(float             *g_d,
                                             `

### computeNumSmallerEigenvalsLarge {#computenumsmallereigenvalslarge}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `int computeNumSmallerEigenvalsLarge(float             *g_d,
                                        `

### createIndicesCompaction {#createindicescompaction}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `void
createIndicesCompaction(T *s_compaction_list_exc, unsigned int num_threads_compaction, cg::thre`


## F

### floorPow2 {#floorpow2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `int floorPow2(int n)
{`


## S

### storeInterval {#storeinterval}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `void storeInterval(unsigned int addr,
                              float       *s_left,
           `

### storeIntervalConverged {#storeintervalconverged}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `void storeIntervalConverged(float             *s_left,
                                       float `

### storeNonEmptyIntervals {#storenonemptyintervals}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `void storeNonEmptyIntervals(unsigned int       addr,
                                       const un`

### subdivideActiveInterval {#subdivideactiveinterval}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/eigenvalues/bisect_util.cu](./bisect_util.cu_docs.md)
- **Context**: `void subdivideActiveInterval(const unsigned int tid,
                                        float  `

