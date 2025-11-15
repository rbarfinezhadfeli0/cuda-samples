# Documentation for Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/freeImageInteropNPP
- **Binary**: No

## Purpose and Role

This is a C/C++ source file containing host-side implementation code.

## Original Source Content

```cpp
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

// A simple CUDA Sample demonstrates how to use FreeImage library with NPP.
// Detailed description of this example can be found as comments in the source
// code.

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
#pragma warning(disable : 4819)
#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

// Common Helpers

#include "Exceptions.h"
#include "FreeImage.h"

// Other Includes
#include <cuda_runtime.h>
#include <fstream>
#include <iostream>
#include <npp.h> // CUDA NPP Definitions
#include <string.h>

// Other helpers
#include <helper_cuda.h>   // helper for CUDA Error handling and initialization
#include <helper_string.h> // helper for string parsing

inline int cudaDeviceInit(int argc, const char **argv)
{
    int deviceCount;
    checkCudaErrors(cudaGetDeviceCount(&deviceCount));

    if (deviceCount == 0) {
        std::cerr << "CUDA error: no devices supporting CUDA." << std::endl;
        exit(EXIT_FAILURE);
    }

    int dev = findCudaDevice(argc, argv);

    cudaDeviceProp deviceProp;
    cudaGetDeviceProperties(&deviceProp, dev);
    std::cerr << "cudaSetDevice GPU" << dev << " = " << deviceProp.name << std::endl;

    checkCudaErrors(cudaSetDevice(dev));

    return dev;
}

// Error handler for FreeImage library.
//  In case this handler is invoked, it throws an NPP exception.
extern "C" void FreeImageErrorHandler(FREE_IMAGE_FORMAT oFif, const char *zMessage) { throw npp::Exception(zMessage); }

std::ostream &operator<<(std::ostream &rOutputStream, const FIBITMAP &rBitmap)
{
    unsigned int nImageWidth  = FreeImage_GetWidth(const_cast<FIBITMAP *>(&rBitmap));
    unsigned int nImageHeight = FreeImage_GetHeight(const_cast<FIBITMAP *>(&rBitmap));
    unsigned int nPitch       = FreeImage_GetPitch(const_cast<FIBITMAP *>(&rBitmap));
    unsigned int nBPP         = FreeImage_GetBPP(const_cast<FIBITMAP *>(&rBitmap));

    FREE_IMAGE_COLOR_TYPE eType = FreeImage_GetColorType(const_cast<FIBITMAP *>(&rBitmap));

    rOutputStream << "Size  (" << nImageWidth << ", " << nImageHeight << ")\n";
    rOutputStream << "Pitch " << nPitch << "\n";
    rOutputStream << "Type  ";

    switch (eType) {
    case FIC_MINISWHITE:
        rOutputStream << "FIC_MINISWHITE\n";
        break;

    case FIC_MINISBLACK:
        rOutputStream << "FIC_MINISBLACK\n";
        break;

    case FIC_RGB:
        rOutputStream << "FIC_RGB\n";
        break;

    case FIC_PALETTE:
        rOutputStream << "FIC_PALETTE\n";
        break;

    case FIC_RGBALPHA:
        rOutputStream << "FIC_RGBALPHA\n";
        break;

    case FIC_CMYK:
        rOutputStream << "FIC_CMYK\n";
        break;

    default:
        rOutputStream << "Unknown pixel format.\n";
    }

    rOutputStream << "BPP   " << nBPP << std::endl;

    return rOutputStream;
}

int main(int argc, char *argv[])
{
    printf("%s Starting...\n\n", argv[0]);

    try {
        std::string sFilename;
        char       *filePath;

        // set your own FreeImage error handler
        FreeImage_SetOutputMessage(FreeImageErrorHandler);

        cudaDeviceInit(argc, (const char **)argv);

        NppStreamContext nppStreamCtx;
        nppStreamCtx.hStream =
            0; // The NULL stream by default, set this to whatever your stream ID is if not the NULL stream.

        cudaError_t cudaError = cudaGetDevice(&nppStreamCtx.nCudaDeviceId);
        if (cudaError != cudaSuccess) {
            printf("CUDA error: no devices supporting CUDA.\n");
            return NPP_NOT_SUFFICIENT_COMPUTE_CAPABILITY;
        }

        const NppLibraryVersion *libVer = nppGetLibVersion();

        printf("NPP Library Version %d.%d.%d\n", libVer->major, libVer->minor, libVer->build);

        int driverVersion, runtimeVersion;
        cudaDriverGetVersion(&driverVersion);
        cudaRuntimeGetVersion(&runtimeVersion);

        printf("CUDA Driver  Version: %d.%d\n", driverVersion / 1000, (driverVersion % 100) / 10);
        printf("CUDA Runtime Version: %d.%d\n\n", runtimeVersion / 1000, (runtimeVersion % 100) / 10);

        cudaError = cudaDeviceGetAttribute(&nppStreamCtx.nCudaDevAttrComputeCapabilityMajor,
                                           cudaDevAttrComputeCapabilityMajor,
                                           nppStreamCtx.nCudaDeviceId);
        if (cudaError != cudaSuccess)
            return NPP_NOT_SUFFICIENT_COMPUTE_CAPABILITY;

        cudaError = cudaDeviceGetAttribute(&nppStreamCtx.nCudaDevAttrComputeCapabilityMinor,
                                           cudaDevAttrComputeCapabilityMinor,
                                           nppStreamCtx.nCudaDeviceId);
        if (cudaError != cudaSuccess)
            return NPP_NOT_SUFFICIENT_COMPUTE_CAPABILITY;

        cudaError = cudaStreamGetFlags(nppStreamCtx.hStream, &nppStreamCtx.nStreamFlags);

        cudaDeviceProp oDeviceProperties;

        cudaError = cudaGetDeviceProperties(&oDeviceProperties, nppStreamCtx.nCudaDeviceId);

        nppStreamCtx.nMultiProcessorCount         = oDeviceProperties.multiProcessorCount;
        nppStreamCtx.nMaxThreadsPerMultiProcessor = oDeviceProperties.maxThreadsPerMultiProcessor;
        nppStreamCtx.nMaxThreadsPerBlock          = oDeviceProperties.maxThreadsPerBlock;
        nppStreamCtx.nSharedMemPerBlock           = oDeviceProperties.sharedMemPerBlock;

        if (checkCmdLineFlag(argc, (const char **)argv, "input")) {
            getCmdLineArgumentString(argc, (const char **)argv, "input", &filePath);
        }
        else {
            filePath = sdkFindFilePath("teapot512.pgm", argv[0]);
        }

        if (filePath) {
            sFilename = filePath;
        }
        else {
            sFilename = "teapot512.pgm";
        }

        // if we specify the filename at the command line, then we only test
        // sFilename otherwise we will check both sFilename[0,1]
        int           file_errors = 0;
        std::ifstream infile(sFilename.data(), std::ifstream::in);

        if (infile.good()) {
            std::cout << "freeImageInteropNPP opened: <" << sFilename.data() << "> successfully!" << std::endl;
            file_errors = 0;
            infile.close();
        }
        else {
            std::cout << "freeImageInteropNPP unable to open: <" << sFilename.data() << ">" << std::endl;
            file_errors++;
            infile.close();
        }

        if (file_errors > 0) {
            exit(EXIT_FAILURE);
        }

        std::string sResultFilename = sFilename;

        std::string::size_type dot = sResultFilename.rfind('.');

        if (dot != std::string::npos) {
            sResultFilename = sResultFilename.substr(0, dot);
        }

        sResultFilename += "_boxFilterFII.pgm";

        if (checkCmdLineFlag(argc, (const char **)argv, "output")) {
            char *outputFilePath;
            getCmdLineArgumentString(argc, (const char **)argv, "output", &outputFilePath);
            sResultFilename = outputFilePath;
        }

        FREE_IMAGE_FORMAT eFormat = FreeImage_GetFileType(sFilename.c_str());

        // no signature? try to guess the file format from the file extension
        if (eFormat == FIF_UNKNOWN) {
            eFormat = FreeImage_GetFIFFromFilename(sFilename.c_str());
        }

        NPP_ASSERT(eFormat != FIF_UNKNOWN);
        // check that the plugin has reading capabilities ...
        FIBITMAP *pBitmap;

        if (FreeImage_FIFSupportsReading(eFormat)) {
            pBitmap = FreeImage_Load(eFormat, sFilename.c_str());
        }

        NPP_ASSERT(pBitmap != 0);
        // Dump the bitmap information to the console
        std::cout << (*pBitmap) << std::endl;
        // make sure this is an 8-bit single channel image
        NPP_ASSERT(FreeImage_GetColorType(pBitmap) == FIC_MINISBLACK);
        NPP_ASSERT(FreeImage_GetBPP(pBitmap) == 8);

        unsigned int   nImageWidth  = FreeImage_GetWidth(pBitmap);
        unsigned int   nImageHeight = FreeImage_GetHeight(pBitmap);
        unsigned int   nSrcPitch    = FreeImage_GetPitch(pBitmap);
        unsigned char *pSrcData     = FreeImage_GetBits(pBitmap);

        int    nSrcPitchCUDA;
        Npp8u *pSrcImageCUDA = nppiMalloc_8u_C1(nImageWidth, nImageHeight, &nSrcPitchCUDA);
        NPP_ASSERT_NOT_NULL(pSrcImageCUDA);
        // copy image loaded via FreeImage to into CUDA device memory, i.e.
        // transfer the image-data up to the GPU's video-memory
        NPP_CHECK_CUDA(cudaMemcpy2D(
            pSrcImageCUDA, nSrcPitchCUDA, pSrcData, nSrcPitch, nImageWidth, nImageHeight, cudaMemcpyHostToDevice));

        // define size of the box filter
        const NppiSize  oMaskSize   = {7, 7};
        const NppiPoint oMaskAchnor = {0, 0};
        // compute maximal result image size
        const NppiSize oSizeROI = {(int)nImageWidth - (oMaskSize.width - 1),
                                   (int)nImageHeight - (oMaskSize.height - 1)};
        // allocate result image memory
        int    nDstPitchCUDA;
        Npp8u *pDstImageCUDA = nppiMalloc_8u_C1(oSizeROI.width, oSizeROI.height, &nDstPitchCUDA);
        NPP_ASSERT_NOT_NULL(pDstImageCUDA);
        NPP_CHECK_NPP(nppiFilterBox_8u_C1R_Ctx(pSrcImageCUDA,
                                               nSrcPitchCUDA,
                                               pDstImageCUDA,
                                               nDstPitchCUDA,
                                               oSizeROI,
                                               oMaskSize,
                                               oMaskAchnor,
                                               nppStreamCtx));
        // create the result image storage using FreeImage so we can easily
        // save
        FIBITMAP *pResultBitmap = FreeImage_Allocate(oSizeROI.width, oSizeROI.height, 8 /* bits per pixel */);
        NPP_ASSERT_NOT_NULL(pResultBitmap);
        unsigned int   nResultPitch = FreeImage_GetPitch(pResultBitmap);
        unsigned char *pResultData  = FreeImage_GetBits(pResultBitmap);

        NPP_CHECK_CUDA(cudaMemcpy2D(pResultData,
                                    nResultPitch,
                                    pDstImageCUDA,
                                    nDstPitchCUDA,
                                    oSizeROI.width,
                                    oSizeROI.height,
                                    cudaMemcpyDeviceToHost));
        // now save the result image
        bool bSuccess;
        bSuccess = FreeImage_Save(FIF_PGM, pResultBitmap, sResultFilename.c_str(), 0) == TRUE;
        NPP_ASSERT_MSG(bSuccess, "Failed to save result image.");

        // free nppiImage
        nppiFree(pSrcImageCUDA);
        nppiFree(pDstImageCUDA);

        exit(EXIT_SUCCESS);
    }
    catch (npp::Exception &rException) {
        std::cerr << "Program error! The following exception occurred: \n";
        std::cerr << rException << std::endl;
        std::cerr << "Aborting." << std::endl;
        exit(EXIT_FAILURE);
    }
    catch (...) {
        std::cerr << "Program error! An unknow type of exception occurred. \n";
        std::cerr << "Aborting." << std::endl;
        exit(EXIT_FAILURE);
    }

    exit(EXIT_SUCCESS);
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/freeImageInteropNPP/freeImageInteropNPP.cpp`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 326
- **Approximate Size**: 12717 bytes

### Content Structure

#### Functions and Kernels

This file contains function definitions and potentially CUDA kernel launches.
Functions in this file handle:

- **Initialization**: Setting up CUDA context and allocating resources
- **Computation**: Core algorithmic implementations
- **Cleanup**: Freeing resources and error checking

#### Error Handling

The code implements error handling through:

- CUDA error checking macros
- Return code validation
- Exception handling where appropriate

#### Memory Management

Memory operations include:

- Device memory allocation (cudaMalloc)
- Host memory allocation
- Memory transfers (cudaMemcpy)
- Proper cleanup and deallocation

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

### Computational Complexity

The algorithms in this file are designed with performance in mind:

- **GPU Parallelism**: Leveraging thousands of CUDA cores
- **Memory Bandwidth**: Optimizing data transfer patterns
- **Occupancy**: Maximizing GPU utilization
- **Latency Hiding**: Using asynchronous operations where beneficial

### Optimization Opportunities

Potential areas for optimization:

1. Kernel launch configuration tuning
2. Shared memory usage
3. Coalesced memory access
4. Reduction of host-device transfers

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

To test this file:

1. Build the sample using CMake
2. Run the executable with appropriate parameters
3. Verify output against expected results
4. Check for memory leaks using cuda-memcheck
5. Profile performance using NVIDIA profiling tools

### Integration Tests

This file is tested as part of the overall sample application, ensuring:

- Correct functionality
- Expected performance characteristics
- Compatibility across different GPU architectures

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Running

```bash
./{executable_name} [options]
```

Refer to the sample's README for specific command-line options and usage patterns.

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
