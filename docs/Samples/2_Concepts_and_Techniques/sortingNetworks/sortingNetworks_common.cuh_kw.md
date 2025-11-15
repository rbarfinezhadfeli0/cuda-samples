# Keywords: Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh
---

**Total Keywords**: 5

---

## C

### Comparator {#comparator}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh](./sortingNetworks_common.cuh_docs.md)
- **Context**: `void Comparator(uint &keyA, uint &valA, uint &keyB, uint &valB, uint dir)
{`


## S

### SHARED_SIZE_LIMIT {#sharedsizelimit}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh](./sortingNetworks_common.cuh_docs.md)
- **Context**: `#define SHARED_SIZE_LIMIT 1024U

// Map to single instructions on G8x / G9x / G100
#define UMUL(a, b`

### SORTINGNETWORKS_COMMON_CUH {#sortingnetworkscommoncuh}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh](./sortingNetworks_common.cuh_docs.md)
- **Context**: `#define SORTINGNETWORKS_COMMON_CUH

#include "sortingNetworks_common.h"

// Enables maximum occupanc`


## U

### UMAD {#umad}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh](./sortingNetworks_common.cuh_docs.md)
- **Context**: `#define UMAD(a, b, c) (UMUL((a), (b)) + (c))

__device__ inline void Comparator(uint &keyA, uint &va`

### UMUL {#umul}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/sortingNetworks/sortingNetworks_common.cuh](./sortingNetworks_common.cuh_docs.md)
- **Context**: `#define UMUL(a, b)    __umul24((a), (b))
#define UMAD(a, b, c) (UMUL((a), (b)) + (c))

__device__ in`

