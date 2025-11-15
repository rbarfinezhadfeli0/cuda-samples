# Keywords: Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu
---

**Total Keywords**: 61

---

## A

### AddFloatPlane {#addfloatplane}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `StrideF, Size);
    AddFloatPlane(-128.0f, ImgF1, Str`


## B

### BENCHMARK_SIZE {#benchmarksize}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `#define BENCHMARK_SIZE 10

/**
 *  The PSNR values over this threshold indicate images equality
 */
`

### BmpUtil {#bmputil}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `edup.
*/

#include "BmpUtil.h"
#include "Common`


## C

### CalculatePSNR {#calculatepsnr}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `c_DstGold1        = CalculatePSNR(ImgSrc, ImgDstGold1`

### CopyByte2Float {#copybyte2float}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: ` representation
    CopyByte2Float(ImgSrc, Stride, Img`

### CopyFloat2Byte {#copyfloat2byte}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `StrideF, Size);
    CopyFloat2Byte(ImgF1, StrideF, Img`


## D

### DeviceStride {#devicestride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `c, *dst;
    size_t DeviceStride;
    checkCudaError`

### DstStride {#dststride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `Dst;
    size_t     DstStride;
    checkCudaError`

### DumpBmpAsGray {#dumpbmpasgray}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `FnameResGold1);
    DumpBmpAsGray(SampleImageFnameRes`


## F

### FreePlane {#freeplane}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `e float buffers
    FreePlane(ImgF1);
    FreePla`


## G

### GridFullWarps {#gridfullwarps}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `parameters
    dim3 GridFullWarps(Size.width / KER2_B`

### GridShort {#gridshort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `parameters
    dim3 GridShort(Size.width / KERS_B`

### GridSmallBlocks {#gridsmallblocks}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `OCK_SIZE);
    dim3 GridSmallBlocks(Size.width / BLOCK_`


## I

### ImgDst {#imgdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `mage plane
* \param ImgDst         [IN] - Quan`

### ImgDstCUDA1 {#imgdstcuda1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `gStride);
    byte *ImgDstCUDA1     = MallocPlaneBy`

### ImgDstCUDA2 {#imgdstcuda2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `gStride);
    byte *ImgDstCUDA2     = MallocPlaneBy`

### ImgDstCUDAshort {#imgdstcudashort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `gStride);
    byte *ImgDstCUDAshort = MallocPlaneByte(I`

### ImgDstGold1 {#imgdstgold1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `gStride);
    byte *ImgDstGold1     = MallocPlaneBy`

### ImgDstGold2 {#imgdstgold2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `gStride);
    byte *ImgDstGold2     = MallocPlaneBy`

### ImgF1 {#imgf1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `StrideF;
    float *ImgF1 = MallocPlaneFloat(`

### ImgF2 {#imgf2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `trideF);
    float *ImgF2 = MallocPlaneFloat(`

### ImgHeight {#imgheight}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `)
    int ImgWidth, ImgHeight;
    ROI ImgSize;
 `

### ImgS1 {#imgs1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `StrideS;
    short *ImgS1 = MallocPlaneShort(`

### ImgSize {#imgsize}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: ` ImgHeight;
    ROI ImgSize;
    int res       `

### ImgSrc {#imgsrc}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `ntations
*
* \param ImgSrc         [IN] - Sour`

### ImgSrcF {#imgsrcf}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `FStride;
    float *ImgSrcF = MallocPlaneFloat(`

### ImgSrcFStride {#imgsrcfstride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `entation
    int    ImgSrcFStride;
    float *ImgSrcF`

### ImgStride {#imgstride}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `e buffers
    int   ImgStride;
    byte *ImgSrc  `

### ImgWidth {#imgwidth}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `dimensions)
    int ImgWidth, ImgHeight;
    ROI`


## L

### LoadBmpAsGray {#loadbmpasgray}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `ad sample image
    LoadBmpAsGray(pSampleImageFpath, `


## M

### MallocPlaneByte {#mallocplanebyte}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: ` *ImgSrc          = MallocPlaneByte(ImgWidth, ImgHeight`

### MallocPlaneFloat {#mallocplanefloat}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `
    float *ImgF1 = MallocPlaneFloat(Size.width, Size.he`

### MallocPlaneShort {#mallocplaneshort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `
    short *ImgS1 = MallocPlaneShort(Size.width, Size.he`


## P

### PSNR_THRESHOLD_EQUAL {#psnrthresholdequal}

- **Type**: macro
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `#define PSNR_THRESHOLD_EQUAL 40

// includes kernels
#include "dct8x8_kernel1.cuh"
#include "dct8x8_`

### PreLoadBmp {#preloadbmp}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `   int res        = PreLoadBmp(pSampleImageFpath, `


## S

### SampleImageFname {#sampleimagefname}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: ` filenames
    char SampleImageFname[]             = "te`

### SampleImageFnameResCUDA1 {#sampleimagefnamerescuda1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `old2.bmp";
    char SampleImageFnameResCUDA1[]     = "teapot512_`

### SampleImageFnameResCUDA2 {#sampleimagefnamerescuda2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `uda1.bmp";
    char SampleImageFnameResCUDA2[]     = "teapot512_`

### SampleImageFnameResCUDAshort {#sampleimagefnamerescudashort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `uda2.bmp";
    char SampleImageFnameResCUDAshort[] = "teapot512_cuda`

### SampleImageFnameResGold1 {#sampleimagefnameresgold1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `t512.bmp";
    char SampleImageFnameResGold1[]     = "teapot512_`

### SampleImageFnameResGold2 {#sampleimagefnameresgold2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `old1.bmp";
    char SampleImageFnameResGold2[]     = "teapot512_`

### SrcDst {#srcdst}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `e memory
    short *SrcDst;
    size_t DeviceS`

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `tart CUDA timer
    StopWatchInterface *timerGold = 0;
   `


## T

### TexSrc {#texsrc}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `cudaTextureObject_t TexSrc;
    cudaResourceDe`

### ThreadsFullWarps {#threadsfullwarps}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `EIGHT, 1);
    dim3 ThreadsFullWarps(8, KER2_BLOCK_WIDTH`

### ThreadsShort {#threadsshort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `EIGHT, 1);
    dim3 ThreadsShort(8, KERS_BLOCK_WIDTH`

### ThreadsSmallBlocks {#threadssmallblocks}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `antization
    dim3 ThreadsSmallBlocks(BLOCK_SIZE, BLOCK_S`

### TimeCUDA1 {#timecuda1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `on... ");
    float TimeCUDA1 = WrapperCUDA1(ImgS`

### TimeCUDA2 {#timecuda2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `on... ");
    float TimeCUDA2 = WrapperCUDA2(ImgS`

### TimeCUDAshort {#timecudashort}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `on... ");
    float TimeCUDAshort = WrapperCUDAshort(`

### TimeGold1 {#timegold1}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `on... ");
    float TimeGold1 = WrapperGold1(ImgS`

### TimeGold2 {#timegold2}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `on... ");
    float TimeGold2 = WrapperGold2(ImgS`

### TimerCUDASpan {#timercudaspan}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `UDA timer
    float TimerCUDASpan = sdkGetAverageTime`

### TimerGoldSpan {#timergoldspan}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `UDA timer
    float TimerGoldSpan = sdkGetAverageTime`

### TimerLibJpegSpan16b {#timerlibjpegspan16b}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `UDA timer
    float TimerLibJpegSpan16b = sdkGetAverageTime`


## W

### WrapperCUDA1 {#wrappercuda1}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `float WrapperCUDA1(byte *ImgSrc, byte *ImgDst, int Stride, ROI Size)
{`

### WrapperCUDA2 {#wrappercuda2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `float WrapperCUDA2(byte *ImgSrc, byte *ImgDst, int Stride, ROI Size)
{`

### WrapperCUDAshort {#wrappercudashort}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `float WrapperCUDAshort(byte *ImgSrc, byte *ImgDst, int Stride, ROI Size)
{`

### WrapperGold1 {#wrappergold1}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `float WrapperGold1(byte *ImgSrc, byte *ImgDst, int Stride, ROI Size)
{`

### WrapperGold2 {#wrappergold2}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `float WrapperGold2(byte *ImgSrc, byte *ImgDst, int Stride, ROI Size)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/dct8x8/dct8x8.cu](./dct8x8.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

