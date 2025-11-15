# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp
---

**Total Keywords**: 16

---

## B

### BmpUtil {#bmputil}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `uded.
*/

#include "BmpUtil.h"
#include "Common`


## F

### FirstIn {#firstin}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `lements.
*
* \param FirstIn        [IN] - Point`

### FirstOut {#firstout}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `
*elements
* \param FirstOut       [OUT] - Point`


## M

### MresStride {#mresstride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `ult matrix
* \param MresStride     [IN] - Stride o`


## S

### SrcDst {#srcdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `   [IN] - Stride of SrcDst
* \param Size      `

### StepIn {#stepin}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `put vector
* \param StepIn         [IN] - Valu`

### StepOut {#stepout}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `put vector
* \param StepOut        [IN] - Value`

### SubroutineDCTvector {#subroutinedctvector}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void SubroutineDCTvector(float *FirstIn, int StepIn, float *FirstOut, int StepOut)
{`

### SubroutineIDCTvector {#subroutineidctvector}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void SubroutineIDCTvector(float *FirstIn, int StepIn, float *FirstOut, int StepOut)
{`


## C

### computeDCT8x8Gold1 {#computedct8x8gold1}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void computeDCT8x8Gold1(const float *fSrc, float *fDst, int Stride, ROI Size)
{`

### computeDCT8x8Gold2 {#computedct8x8gold2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void computeDCT8x8Gold2(const float *fSrc, float *fDst, int Stride, ROI Size)
{`

### computeIDCT8x8Gold1 {#computeidct8x8gold1}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void computeIDCT8x8Gold1(const float *fSrc, float *fDst, int Stride, ROI Size)
{`

### computeIDCT8x8Gold2 {#computeidct8x8gold2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void computeIDCT8x8Gold2(const float *fSrc, float *fDst, int Stride, ROI Size)
{`


## M

### mult8x8 {#mult8x8}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void mult8x8(const float *M1, int M1Stride, const float *M2, int M2Stride, float *Mres, int MresStri`


## Q

### quantizeGoldFloat {#quantizegoldfloat}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void quantizeGoldFloat(float *fSrcDst, int Stride, ROI Size)
{`

### quantizeGoldShort {#quantizegoldshort}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/DCT8x8_Gold.cpp](./DCT8x8_Gold.cpp_docs.md)
- **Context**: `void quantizeGoldShort(short *fSrcDst, int Stride, ROI Size)
{`

