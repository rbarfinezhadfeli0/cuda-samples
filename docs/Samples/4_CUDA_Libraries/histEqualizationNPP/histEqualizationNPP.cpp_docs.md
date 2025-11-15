# Documentation for Samples/4_CUDA_Libraries/histEqualizationNPP/histEqualizationNPP.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/histEqualizationNPP/histEqualizationNPP.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/histEqualizationNPP
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

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
#pragma warning(disable : 4819)
#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#endif

#include <Exceptions.h>
#include <ImageIO.h>
#include <ImagesCPU.h>
#include <ImagesNPP.h>
#include <fstream>
#include <helper_cuda.h>
#include <iostream>
#include <npp.h>
#include <string.h>
#include <string>

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
#define STRCASECMP  _stricmp
#define STRNCASECMP _strnicmp
#else
#define STRCASECMP  strcasecmp
#define STRNCASECMP strncasecmp
#endif

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

int main(int argc, char *argv[])
{
    printf("%s Starting...\n\n", argv[0]);

    try {
        std::string sFilename;
        char       *filePath;

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
        // sFilename.
        int           file_errors = 0;
        std::ifstream infile(sFilename.data(), std::ifstream::in);

        if (infile.good()) {
            std::cout << "histEqualizationNPP opened: <" << sFilename.data() << "> successfully!" << std::endl;
            file_errors = 0;
            infile.close();
        }
        else {
            std::cout << "histEqualizationNPP unable to open: <" << sFilename.data() << ">" << std::endl;
            file_errors++;
            infile.close();
        }

        if (file_errors > 0) {
            exit(EXIT_FAILURE);
        }

        std::string dstFileName = sFilename;

        std::string::size_type dot = dstFileName.rfind('.');

        if (dot != std::string::npos) {
            dstFileName = dstFileName.substr(0, dot);
        }

        dstFileName += "_histEqualization.pgm";

        if (checkCmdLineFlag(argc, (const char **)argv, "output")) {
            char *outputFilePath;
            getCmdLineArgumentString(argc, (const char **)argv, "output", &outputFilePath);
            dstFileName = outputFilePath;
        }

        npp::ImageCPU_8u_C1 oHostSrc;
        npp::loadImage(sFilename, oHostSrc);
        npp::ImageNPP_8u_C1 oDeviceSrc(oHostSrc);

        //
        // allocate arrays for histogram and levels
        //

        const int binCount   = 255;
        const int levelCount = binCount + 1; // levels array has one more element

        Npp32s *histDevice   = 0;
        Npp32s *levelsDevice = 0;

        NPP_CHECK_CUDA(cudaMalloc((void **)&histDevice, binCount * sizeof(Npp32s)));
        NPP_CHECK_CUDA(cudaMalloc((void **)&levelsDevice, levelCount * sizeof(Npp32s)));

        //
        // compute histogram
        //

        NppiSize oSizeROI = {(int)oDeviceSrc.width(), (int)oDeviceSrc.height()}; // full image
        // create device scratch buffer for nppiHistogram
        size_t nDeviceBufferSize;
        nppiHistogramEvenGetBufferSize_8u_C1R_Ctx(oSizeROI, levelCount, &nDeviceBufferSize, nppStreamCtx);
        Npp8u *pDeviceBuffer;
        NPP_CHECK_CUDA(cudaMalloc((void **)&pDeviceBuffer, nDeviceBufferSize));

        // compute levels values on host
        Npp32s levelsHost[levelCount];
        NPP_CHECK_NPP(nppiEvenLevelsHost_32s(levelsHost, levelCount, 0, binCount));
        // compute the histogram
        NPP_CHECK_NPP(nppiHistogramEven_8u_C1R_Ctx(oDeviceSrc.data(),
                                                   oDeviceSrc.pitch(),
                                                   oSizeROI,
                                                   histDevice,
                                                   levelCount,
                                                   0,
                                                   binCount,
                                                   pDeviceBuffer,
                                                   nppStreamCtx));
        // copy histogram and levels to host memory
        Npp32s histHost[binCount];
        NPP_CHECK_CUDA(cudaMemcpy(histHost, histDevice, binCount * sizeof(Npp32s), cudaMemcpyDeviceToHost));

        Npp32s lutHost[levelCount];

        // fill LUT
        {
            Npp32s *pHostHistogram = histHost;
            Npp32s  totalSum       = 0;

            for (; pHostHistogram < histHost + binCount; ++pHostHistogram) {
                totalSum += *pHostHistogram;
            }

            NPP_ASSERT(totalSum <= oSizeROI.width * oSizeROI.height);

            if (totalSum == 0) {
                totalSum = 1;
            }

            float multiplier = 1.0f / float(oSizeROI.width * oSizeROI.height) * 0xFF;

            Npp32s  runningSum   = 0;
            Npp32s *pLookupTable = lutHost;

            for (pHostHistogram = histHost; pHostHistogram < histHost + binCount; ++pHostHistogram) {
                *pLookupTable = (Npp32s)(runningSum * multiplier + 0.5f);
                pLookupTable++;
                runningSum += *pHostHistogram;
            }

            lutHost[binCount] = 0xFF; // last element is always 1
        }

        //
        // apply LUT transformation to the image
        //
        // Create a device image for the result.
        npp::ImageNPP_8u_C1 oDeviceDst(oDeviceSrc.size());

#if CUDART_VERSION >= 5000
        // Note for CUDA 5.0, that nppiLUT_Linear_8u_C1R requires these pointers to
        // be in GPU device memory
        Npp32s *lutDevice  = 0;
        Npp32s *lvlsDevice = 0;

        NPP_CHECK_CUDA(cudaMalloc((void **)&lutDevice, sizeof(Npp32s) * (levelCount)));
        NPP_CHECK_CUDA(cudaMalloc((void **)&lvlsDevice, sizeof(Npp32s) * (levelCount)));

        NPP_CHECK_CUDA(cudaMemcpy(lutDevice, lutHost, sizeof(Npp32s) * (levelCount), cudaMemcpyHostToDevice));
        NPP_CHECK_CUDA(cudaMemcpy(lvlsDevice, levelsHost, sizeof(Npp32s) * (levelCount), cudaMemcpyHostToDevice));

        NPP_CHECK_NPP(nppiLUT_Linear_8u_C1R_Ctx(oDeviceSrc.data(),
                                                oDeviceSrc.pitch(),
                                                oDeviceDst.data(),
                                                oDeviceDst.pitch(),
                                                oSizeROI,
                                                lutDevice, // value and level arrays are in host memory
                                                lvlsDevice,
                                                levelCount,
                                                nppStreamCtx));

        NPP_CHECK_CUDA(cudaFree(lutDevice));
        NPP_CHECK_CUDA(cudaFree(lvlsDevice));
#else
        NPP_CHECK_NPP(nppiLUT_Linear_8u_C1R_Ctx(oDeviceSrc.data(),
                                                oDeviceSrc.pitch(),
                                                oDeviceDst.data(),
                                                oDeviceDst.pitch(),
                                                oSizeROI,
                                                lutHost, // value and level arrays are in host memory
                                                levelsHost,
                                                levelCount,
                                                nppStreamCtx));
#endif

        // copy the result image back into the storage that contained the
        // input image
        npp::ImageCPU_8u_C1 oHostDst(oDeviceDst.size());
        oDeviceDst.copyTo(oHostDst.data(), oHostDst.pitch());

        cudaFree(histDevice);
        cudaFree(levelsDevice);
        cudaFree(pDeviceBuffer);
        nppiFree(oDeviceSrc.data());
        nppiFree(oDeviceDst.data());

        // save the result
        npp::saveImage(dstFileName.c_str(), oHostDst);
        std::cout << "Saved image file " << dstFileName << std::endl;
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

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/histEqualizationNPP/histEqualizationNPP.cpp`.

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

- **Total Lines**: 327
- **Approximate Size**: 12799 bytes

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
