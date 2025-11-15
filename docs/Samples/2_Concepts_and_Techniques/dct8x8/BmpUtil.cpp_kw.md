# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp
---

**Total Keywords**: 28

---

## A

### AddFloatPlane {#addfloatplane}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void AddFloatPlane(float Value, float *ImgSrcDst, int StrideF, ROI Size)
{`


## B

### BmpUtil {#bmputil}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `***********
* \file BmpUtil.cpp
* \brief Contai`


## C

### CalculateMSE {#calculatemse}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `float CalculateMSE(byte *Img1, byte *Img2, int Stride, ROI Size)
{`

### CalculatePSNR {#calculatepsnr}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `float CalculatePSNR(byte *Img1, byte *Img2, int Stride, ROI Size)
{`

### CopyByte2Float {#copybyte2float}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void CopyByte2Float(byte *ImgSrc, int StrideB, float *ImgDst, int StrideF, ROI Size)
{`

### CopyFloat2Byte {#copyfloat2byte}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void CopyFloat2Byte(float *ImgSrc, int StrideF, byte *ImgDst, int StrideB, ROI Size)
{`


## D

### DumpBlock {#dumpblock}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void DumpBlock(byte *Plane, int Stride, char *Fname)
{`

### DumpBlockF {#dumpblockf}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void DumpBlockF(float *PlaneF, int StrideF, char *Fname)
{`

### DumpBmpAsGray {#dumpbmpasgray}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void DumpBmpAsGray(char *FileName, byte *Img, int Stride, ROI ImSize)
{`


## F

### FileHeader {#fileheader}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `{
    BMPFileHeader FileHeader;
    BMPInfoHeader `

### FileName {#filename}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `mensions
*
* \param FileName       [IN] - Image `

### FreePlane {#freeplane}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void FreePlane(void *ptr)
{`


## I

### ImSize {#imsize}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `age stride
* \param ImSize         [IN] - Imag`

### ImgDst {#imgdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `ane stride
* \param ImgDst             [OUT] -`

### ImgSrc {#imgsrc}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `at plane
*
* \param ImgSrc             [IN] - `

### ImgSrcDst {#imgsrcdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `lue to add
* \param ImgSrcDst          [IN/OUT] -`

### InfoHeader {#infoheader}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `;
    BMPInfoHeader InfoHeader;
    FILE         *`


## L

### LoadBmpAsGray {#loadbmpasgray}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void LoadBmpAsGray(char *FileName, int Stride, ROI ImSize, byte *Img)
{`


## M

### MallocPlaneByte {#mallocplanebyte}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `ated plane
*/
byte *MallocPlaneByte(int width, int heig`

### MallocPlaneFloat {#mallocplanefloat}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `ted plane
*/
float *MallocPlaneFloat(int width, int heig`

### MallocPlaneShort {#mallocplaneshort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `ted plane
*/
short *MallocPlaneShort(int width, int heig`

### MulFloatPlane {#mulfloatplane}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `void MulFloatPlane(float Value, float *ImgSrcDst, int StrideF, ROI Size)
{`


## N

### NumAbs {#numabs}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `at num)
{
    float NumAbs  = fabs(num);
    i`

### NumAbsI {#numabsi}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `abs(num);
    int   NumAbsI = (int)(NumAbs + 0.`


## P

### PreLoadBmp {#preloadbmp}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `int PreLoadBmp(char *FileName, int *Width, int *Height)
{`


## T

### TmpDiff {#tmpdiff}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `) {
            int TmpDiff = Img1[i * Stride +`


## C

### clamp_0_255 {#clamp0255}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `int clamp_0_255(int x) {`


## R

### round_f {#roundf}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/BmpUtil.cpp](./BmpUtil.cpp_docs.md)
- **Context**: `float round_f(float num)
{`

