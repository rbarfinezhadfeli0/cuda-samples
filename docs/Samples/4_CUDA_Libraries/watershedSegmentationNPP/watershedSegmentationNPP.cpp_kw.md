# Keywords: Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp
---

**Total Keywords**: 23

---

## C

### CompressedSegmentLabelsOutputFile0 {#compressedsegmentlabelsoutputfile0}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &CompressedSegmentLabelsOutputFile0 = "teapot_Compresse`

### CompressedSegmentLabelsOutputFile1 {#compressedsegmentlabelsoutputfile1}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &CompressedSegmentLabelsOutputFile1 = "CT_skull_Compres`

### CompressedSegmentLabelsOutputFile2 {#compressedsegmentlabelsoutputfile2}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &CompressedSegmentLabelsOutputFile2 = "Rocks_Compressed`


## I

### InputFile {#inputfile}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `        const char *InputFile = sdkFindFilePath(f`


## N

### NOMINMAX {#nominmax}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `#define NOMINMAX
#include <windows.h>
#pragma warning(disable : 4819)
#endif

#include <fstream>
#in`

### NUMBER_OF_IMAGES {#numberofimages}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `#define NUMBER_OF_IMAGES 3

Npp8u  *pInputImageDev[NUMBER_OF_IMAGES];
Npp8u  *pInputImageHost[NUMBER`

### NppLibraryVersion {#npplibraryversion}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `Y;
    }

    const NppLibraryVersion *libVer = nppGetLib`

### NppStatus {#nppstatus}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `     cudaError;
    NppStatus        nppStatus;
 `

### NppStreamContext {#nppstreamcontext}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `     nppStatus;
    NppStreamContext nppStreamCtx;
    F`

### NppiNorm {#nppinorm}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `      *bmpFile;
    NppiNorm         eNorm = npp`

### NppiSize {#nppisize}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `edMemPerBlock;

    NppiSize oSizeROI[NUMBER_OF_`


## S

### SegmentBoundariesOutputFile0 {#segmentboundariesoutputfile0}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentBoundariesOutputFile0 = "teapot_SegmentBo`

### SegmentBoundariesOutputFile1 {#segmentboundariesoutputfile1}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentBoundariesOutputFile1 = "CT_skull_Segment`

### SegmentBoundariesOutputFile2 {#segmentboundariesoutputfile2}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentBoundariesOutputFile2 = "Rocks_SegmentBou`

### SegmentsOutputFile0 {#segmentsoutputfile0}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentsOutputFile0 = "teapot_Segments_`

### SegmentsOutputFile1 {#segmentsoutputfile1}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentsOutputFile1 = "CT_skull_Segment`

### SegmentsOutputFile2 {#segmentsoutputfile2}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentsOutputFile2 = "Rocks_Segments_8`

### SegmentsWithContrastingBoundariesOutputFile0 {#segmentswithcontrastingboundariesoutputfile0}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentsWithContrastingBoundariesOutputFile0 =
    "teapot_Segme`

### SegmentsWithContrastingBoundariesOutputFile1 {#segmentswithcontrastingboundariesoutputfile1}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentsWithContrastingBoundariesOutputFile1 =
    "CT_skull_Seg`

### SegmentsWithContrastingBoundariesOutputFile2 {#segmentswithcontrastingboundariesoutputfile2}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `
const std::string &SegmentsWithContrastingBoundariesOutputFile2 =
    "Rocks_Segmen`


## W

### WINDOWS_LEAN_AND_MEAN {#windowsleanandmean}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#pragma warning(disable : 4819)
`


## L

### loadRaw8BitImage {#loadraw8bitimage}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `int loadRaw8BitImage(Npp8u *pImage, int nWidth, int nHeight, int nImage)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/watershedSegmentationNPP/watershedSegmentationNPP.cpp](./watershedSegmentationNPP.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

