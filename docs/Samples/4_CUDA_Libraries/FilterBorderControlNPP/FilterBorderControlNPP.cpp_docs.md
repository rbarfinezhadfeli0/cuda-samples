# Documentation for Samples/4_CUDA_Libraries/FilterBorderControlNPP/FilterBorderControlNPP.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/FilterBorderControlNPP/FilterBorderControlNPP.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/FilterBorderControlNPP
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

// USING NPP IMAGE FILTER SOURCE IMAGE BORDER CONTROL
// This sample demonstrates how any border version of an NPP filtering function
// can be used in the most common mode, with border control enabled. Mentioned
// functions can be used to duplicate the results of the equivalent non-border
// version of the NPP functions. They can be also used for enabling and
// disabling border control on various source image edges depending on what
// portion of the source image is being used as input.

#if defined(WIN32) || defined(_WIN32) || defined(WIN64) || defined(_WIN64)
#define WINDOWS_LEAN_AND_MEAN
#define NOMINMAX
#include <windows.h>
#pragma warning(disable : 4819)
#endif

#include <Exceptions.h>
#include <ImageIO.h>
#include <ImagesCPU.h>
#include <ImagesNPP.h>
#include <cuda_runtime.h>
#include <fstream>
#include <helper_cuda.h>
#include <helper_string.h>
#include <iostream>
#include <npp.h>
#include <string.h>

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
        const char *inputFile  = "teapot512.pgm";
        std::string sFilename  = inputFile;
        std::string sOutputDir = "./";

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

        char *filePath;

        if (checkCmdLineFlag(argc, (const char **)argv, "input")) {
            getCmdLineArgumentString(argc, (const char **)argv, "input", &filePath);
        }
        else {
            filePath = sdkFindFilePath(inputFile, argv[0]);
        }

        if (!filePath) {
            std::cerr << "Couldn't find input file " << sFilename << std::endl;
            exit(1);
        }

        sFilename = filePath;

        // if we specify the filename at the command line, then we only test
        // sFilename[0].
        int           file_errors = 0;
        std::ifstream infile(sFilename.data(), std::ifstream::in);

        if (infile.good()) {
            std::cout << "gradientFilterBorderNPP opened <" << sFilename.data() << "> successfully!" << std::endl;
            file_errors = 0;
            infile.close();
        }
        else {
            std::cout << "gradientFilterBorderNPP unable to open <" << sFilename.data() << ">" << std::endl;
            file_errors++;
            infile.close();
        }

        if (file_errors > 0) {
            cudaDeviceReset();
            exit(EXIT_FAILURE);
        }

        std::string sResultBaseFilename = sFilename;

        std::string::size_type dot = sResultBaseFilename.rfind('.');

        if (dot != std::string::npos) {
            sResultBaseFilename = sResultBaseFilename.substr(0, dot);
        }

        std::string sResultXFilename = sOutputDir + sFilename + "_gradientVectorPrewittBorderX_Vertical.pgm";
        std::string sResultYFilename = sResultBaseFilename;

        //        sResultXFilename += "_gradientVectorPrewittBorderX_Vertical.pgm";
        sResultYFilename += "_gradientVectorPrewittBorderY_Horizontal.pgm";

        //        if (checkCmdLineFlag(argc, (const char **)argv, "output"))
        //        {
        //           char *outputFilePath;
        //            getCmdLineArgumentString(argc, (const char **)argv, "output",
        //            &outputFilePath); sResultBaseFilename = outputFilePath;
        //        }

        // declare a host image object for an 8-bit grayscale image
        npp::ImageCPU_8u_C1 oHostSrc;
        // load gray-scale image from disk
        npp::loadImage(sFilename, oHostSrc);
        // declare a device image and copy construct from the host image,
        // i.e. upload host to device
        npp::ImageNPP_8u_C1 oDeviceSrc(oHostSrc);

        NppiSize  oSrcSize   = {(int)oDeviceSrc.width(), (int)oDeviceSrc.height()};
        NppiPoint oSrcOffset = {0, 0};

        // create struct with ROI size
        NppiSize oSizeROI = {(int)oDeviceSrc.width(), (int)oDeviceSrc.height()};
        // allocate device destination images of appropriatedly size
        npp::ImageNPP_16s_C1 oDeviceDstX(oSizeROI.width, oSizeROI.height);
        npp::ImageNPP_16s_C1 oDeviceDstY(oSizeROI.width, oSizeROI.height);

        // run Prewitt edge detection gradient vector filter
        NPP_CHECK_NPP(nppiGradientVectorPrewittBorder_8u16s_C1R_Ctx(oDeviceSrc.data(),
                                                                    oDeviceSrc.pitch(),
                                                                    oSrcSize,
                                                                    oSrcOffset,
                                                                    oDeviceDstX.data(),
                                                                    oDeviceDstX.pitch(),
                                                                    oDeviceDstY.data(),
                                                                    oDeviceDstY.pitch(),
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    oSizeROI,
                                                                    NPP_MASK_SIZE_3_X_3,
                                                                    nppiNormL1,
                                                                    NPP_BORDER_REPLICATE,
                                                                    nppStreamCtx));

        // allocate device destination images of appropriatedly size
        npp::ImageNPP_8u_C1 oDeviceDstOutX(oSizeROI.width, oSizeROI.height);
        npp::ImageNPP_8u_C1 oDeviceDstOutY(oSizeROI.width, oSizeROI.height);

        // convert 16s_C1 result images to binary 8u_C1 output images using constant
        // value to adjust amount of visible detail
        NPP_CHECK_NPP(nppiCompareC_16s_C1R_Ctx(oDeviceDstX.data(),
                                               oDeviceDstX.pitch(),
                                               32,
                                               oDeviceDstOutX.data(),
                                               oDeviceDstOutX.pitch(),
                                               oSizeROI,
                                               NPP_CMP_GREATER_EQ,
                                               nppStreamCtx));

        NPP_CHECK_NPP(nppiCompareC_16s_C1R_Ctx(oDeviceDstY.data(),
                                               oDeviceDstY.pitch(),
                                               32,
                                               oDeviceDstOutY.data(),
                                               oDeviceDstOutY.pitch(),
                                               oSizeROI,
                                               NPP_CMP_GREATER_EQ,
                                               nppStreamCtx));

        // create host images for the results
        npp::ImageCPU_8u_C1 oHostDstX(oDeviceDstOutX.size());
        npp::ImageCPU_8u_C1 oHostDstY(oDeviceDstOutY.size());
        // and copy the device result data into them
        oDeviceDstOutX.copyTo(oHostDstX.data(), oHostDstX.pitch());
        oDeviceDstOutY.copyTo(oHostDstY.data(), oHostDstY.pitch());

        saveImage(sResultXFilename, oHostDstX);
        std::cout << "Saved image: " << sResultXFilename << std::endl;
        saveImage(sResultYFilename, oHostDstY);
        std::cout << "Saved image: " << sResultYFilename << std::endl;

        // now use the Prewitt gradient border filter function in such a way that no
        // border replication operations will be applied

        // create a Prewitt filter mask size object, Prewitt uses a 3x3 filter
        // kernel
        NppiSize oMaskSize = {3, 3};
        // create a size object for the enlarged source image
        NppiSize oEnlargedSrcSize = {oSrcSize.width + oMaskSize.width - 1, oSrcSize.height + oMaskSize.height - 1};

        // create an enlarged device source image
        npp::ImageNPP_8u_C1 oEnlargedDeviceSrc(oEnlargedSrcSize.width, oEnlargedSrcSize.height);

        // copy and enlarge the original device source image and surround it with a
        // white edge (border)
        NPP_CHECK_NPP(nppiCopyConstBorder_8u_C1R_Ctx(oDeviceSrc.data(),
                                                     oDeviceSrc.pitch(),
                                                     oSrcSize,
                                                     oEnlargedDeviceSrc.data(),
                                                     oEnlargedDeviceSrc.pitch(),
                                                     oEnlargedSrcSize,
                                                     oMaskSize.width / 2,
                                                     oMaskSize.height / 2,
                                                     255,
                                                     nppStreamCtx));

        // adjust oEnlargedDeviceSrc pixel pointer to point to the first pixel of
        // the original source image in the enlarged source image
        const Npp8u *pTemp = reinterpret_cast<const Npp8u *>(oEnlargedDeviceSrc.data());
        pTemp += (oMaskSize.height / 2) * oEnlargedDeviceSrc.pitch();
        const Npp8u *pAdjustedSrc = reinterpret_cast<const Npp8u *>((void *)(pTemp));
        pAdjustedSrc += oMaskSize.width / 2;

        // create device output images for the no source border results
        npp::ImageNPP_8u_C1 oDeviceDstOutXNoBorders(oSizeROI.width, oSizeROI.height);
        npp::ImageNPP_8u_C1 oDeviceDstOutYNoBorders(oSizeROI.width, oSizeROI.height);

        // tell the filter function what cartesian pixel position pAdjustedSrc is
        // pointing to within the enlarged source image
        oSrcOffset.x += oMaskSize.width / 2;
        oSrcOffset.y += oMaskSize.height / 2;

        // run Prewitt edge detection gradient vector filter bypassing border
        // control due to enlarged source image
        NPP_CHECK_NPP(nppiGradientVectorPrewittBorder_8u16s_C1R_Ctx(pAdjustedSrc,
                                                                    oEnlargedDeviceSrc.pitch(),
                                                                    oEnlargedSrcSize,
                                                                    oSrcOffset,
                                                                    oDeviceDstX.data(),
                                                                    oDeviceDstX.pitch(),
                                                                    oDeviceDstY.data(),
                                                                    oDeviceDstY.pitch(),
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    oSizeROI,
                                                                    NPP_MASK_SIZE_3_X_3,
                                                                    nppiNormL1,
                                                                    NPP_BORDER_REPLICATE,
                                                                    nppStreamCtx));

        // convert 16s_C1 result images to binary 8u_C1 output images using constant
        // value to adjust amount of visible detail
        NPP_CHECK_NPP(nppiCompareC_16s_C1R_Ctx(oDeviceDstX.data(),
                                               oDeviceDstX.pitch(),
                                               32,
                                               oDeviceDstOutXNoBorders.data(),
                                               oDeviceDstOutXNoBorders.pitch(),
                                               oSizeROI,
                                               NPP_CMP_GREATER_EQ,
                                               nppStreamCtx));

        NPP_CHECK_NPP(nppiCompareC_16s_C1R_Ctx(oDeviceDstY.data(),
                                               oDeviceDstY.pitch(),
                                               32,
                                               oDeviceDstOutYNoBorders.data(),
                                               oDeviceDstOutYNoBorders.pitch(),
                                               oSizeROI,
                                               NPP_CMP_GREATER_EQ,
                                               nppStreamCtx));
        // create additional output files
        std::string sResultXNoBordersFilename = sResultBaseFilename;
        std::string sResultYNoBordersFilename = sResultBaseFilename;

        sResultXNoBordersFilename += "_gradientVectorPrewittBorderX_Vertical_WithNoSourceBorders.pgm";
        sResultYNoBordersFilename += "_gradientVectorPrewittBorderY_Horizontal_WithNoSourceBorders.pgm";

        // copy the device result data into the host output images
        oDeviceDstOutXNoBorders.copyTo(oHostDstX.data(), oHostDstX.pitch());
        oDeviceDstOutYNoBorders.copyTo(oHostDstY.data(), oHostDstY.pitch());

        saveImage(sResultXNoBordersFilename, oHostDstX);
        std::cout << "Saved image: " << sResultXNoBordersFilename << std::endl;
        saveImage(sResultYNoBordersFilename, oHostDstY);
        std::cout << "Saved image: " << sResultYNoBordersFilename << std::endl;

        // now diff the two output images, one using border control and one
        // bypassing border control

        // create device output images for the diff results
        npp::ImageNPP_8u_C1 oDeviceDstOutXDiff(oSizeROI.width, oSizeROI.height);
        npp::ImageNPP_8u_C1 oDeviceDstOutYDiff(oSizeROI.width, oSizeROI.height);

        // diff the two 8u_C1 result images one with and one without border control

        NPP_CHECK_NPP(nppiAbsDiff_8u_C1R_Ctx(oDeviceDstOutXNoBorders.data(),
                                             oDeviceDstOutXNoBorders.pitch(),
                                             oDeviceDstOutX.data(),
                                             oDeviceDstOutX.pitch(),
                                             oDeviceDstOutXDiff.data(),
                                             oDeviceDstOutXDiff.pitch(),
                                             oSizeROI,
                                             nppStreamCtx));

        NPP_CHECK_NPP(nppiAbsDiff_8u_C1R_Ctx(oDeviceDstOutYNoBorders.data(),
                                             oDeviceDstOutYNoBorders.pitch(),
                                             oDeviceDstOutY.data(),
                                             oDeviceDstOutY.pitch(),
                                             oDeviceDstOutYDiff.data(),
                                             oDeviceDstOutYDiff.pitch(),
                                             oSizeROI,
                                             nppStreamCtx));

        // create additional output files
        std::string sResultXDiffFilename = sResultBaseFilename;
        std::string sResultYDiffFilename = sResultBaseFilename;

        sResultXDiffFilename += "_gradientVectorPrewittBorderX_Vertical_BorderDiffs.pgm";
        sResultYDiffFilename += "_gradientVectorPrewittBorderY_Horizontal_BorderDiffs.pgm";

        // copy the device result data into the host output images
        oDeviceDstOutXDiff.copyTo(oHostDstX.data(), oHostDstX.pitch());
        oDeviceDstOutYDiff.copyTo(oHostDstY.data(), oHostDstY.pitch());

        saveImage(sResultXDiffFilename, oHostDstX);
        std::cout << "Saved image: " << sResultXDiffFilename << std::endl;
        saveImage(sResultYDiffFilename, oHostDstY);
        std::cout << "Saved image: " << sResultYDiffFilename << std::endl;

        // if you closely examine the above difference files (recommend using GIMP
        // for viewing using scaling with no interpolation) you will see several
        // single pixel differences (white pixels) along the right and bottom edges
        // of the default vs. borderless images this happens because border pixels
        // in the original source image are duplicated when the filter kernels
        // overlap the edge of the source image when using the first version of the
        // filter call but are actually sampled from the enlarged source image when
        // using the second version of the filter call the technique used in the
        // second filter call can be used with any filter border function in NPP to
        // duplicate results that would be generated from a non-border filter
        // function call by filling the border pixel outside the embedded source
        // image with the appropriate border pixel values

        // here is how to use border control to process a source image in multiple
        // calls and get correct output in the destination image

        // since the source image pointer already points to the beginning of the
        // source image in the enlarged source image it doesn't need changed

        // tighten up the top and left source image borders - this will enable
        // border replication on the left and top borders of the original source
        // image
        oSrcOffset.x = 0;
        oSrcOffset.y = 0;
        // tighten up the right and bottom side source image borders - this will
        // enable border replication on the right and bottom borders of the original
        // source image
        oEnlargedSrcSize.width  = oSrcSize.width;
        oEnlargedSrcSize.height = oSrcSize.height;

        // create device output images for the mixed edge results
        npp::ImageNPP_8u_C1 oDeviceDstOutXMixedBorders(oSizeROI.width, oSizeROI.height);
        npp::ImageNPP_8u_C1 oDeviceDstOutYMixedBorders(oSizeROI.width, oSizeROI.height);

        // shrink output ROI width so that only the left half of the destination
        // image will be generated however since oEnlargedSrcSize.width is still set
        // to oSrcSize.width then border control will be disabled when the filter
        // needs to access source pixels beyond the right side of the left half of
        // the source image
        int nLeftWidth  = oSizeROI.width / 2;
        int nRightWidth = oSizeROI.width - nLeftWidth;
        oSizeROI.width  = nLeftWidth;

        // run Prewitt edge detection gradient vector filter to generate the left
        // side of the output image
        NPP_CHECK_NPP(nppiGradientVectorPrewittBorder_8u16s_C1R_Ctx(pAdjustedSrc,
                                                                    oEnlargedDeviceSrc.pitch(),
                                                                    oEnlargedSrcSize,
                                                                    oSrcOffset,
                                                                    oDeviceDstX.data(),
                                                                    oDeviceDstX.pitch(),
                                                                    oDeviceDstY.data(),
                                                                    oDeviceDstY.pitch(),
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    oSizeROI,
                                                                    NPP_MASK_SIZE_3_X_3,
                                                                    nppiNormL1,
                                                                    NPP_BORDER_REPLICATE,
                                                                    nppStreamCtx));

        // now move the enlarged source pointer to the horizontal middle of the
        // enlarged source image and tell the function where it was moved to
        pAdjustedSrc += nLeftWidth;
        // and adjust the source offset parameter accordingly - this will in effect
        // turn off border control for the left border allowing the necessary source
        // pixels to be used
        oSrcOffset.x += nLeftWidth;

        // update oSizeROI.width so that only enough destination pixels will be
        // produced to fill the right half of the destination image
        oSizeROI.width = nRightWidth;

        // run Prewitt edge detection gradient vector filter to generate the right
        // side of the output image adjusting the destination image pointers
        // appropriately
        NPP_CHECK_NPP(nppiGradientVectorPrewittBorder_8u16s_C1R_Ctx(pAdjustedSrc,
                                                                    oEnlargedDeviceSrc.pitch(),
                                                                    oEnlargedSrcSize,
                                                                    oSrcOffset,
                                                                    oDeviceDstX.data() + nLeftWidth,
                                                                    oDeviceDstX.pitch(),
                                                                    oDeviceDstY.data() + nLeftWidth,
                                                                    oDeviceDstY.pitch(),
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    0,
                                                                    oSizeROI,
                                                                    NPP_MASK_SIZE_3_X_3,
                                                                    nppiNormL1,
                                                                    NPP_BORDER_REPLICATE,
                                                                    nppStreamCtx));

        // convert 16s_C1 result images to binary 8u_C1 output images using constant
        // value to adjust amount of visible detail
        NPP_CHECK_NPP(nppiCompareC_16s_C1R_Ctx(oDeviceDstX.data(),
                                               oDeviceDstX.pitch(),
                                               32,
                                               oDeviceDstOutXMixedBorders.data(),
                                               oDeviceDstOutXMixedBorders.pitch(),
                                               oSizeROI,
                                               NPP_CMP_GREATER_EQ,
                                               nppStreamCtx));

        NPP_CHECK_NPP(nppiCompareC_16s_C1R_Ctx(oDeviceDstY.data(),
                                               oDeviceDstY.pitch(),
                                               32,
                                               oDeviceDstOutYMixedBorders.data(),
                                               oDeviceDstOutYMixedBorders.pitch(),
                                               oSizeROI,
                                               NPP_CMP_GREATER_EQ,
                                               nppStreamCtx));
        // create additional output files
        std::string sResultXMixedBordersFilename = sResultBaseFilename;
        std::string sResultYMixedBordersFilename = sResultBaseFilename;

        sResultXMixedBordersFilename += "_gradientVectorPrewittBorderX_Vertical_WithMixedBorders.pgm";
        sResultYMixedBordersFilename += "_gradientVectorPrewittBorderY_Horizontal_WithMixedBorders.pgm";

        // copy the device result data into the host output images
        oDeviceDstOutXMixedBorders.copyTo(oHostDstX.data(), oHostDstX.pitch());
        oDeviceDstOutYMixedBorders.copyTo(oHostDstY.data(), oHostDstY.pitch());

        saveImage(sResultXMixedBordersFilename, oHostDstX);
        std::cout << "Saved image: " << sResultXMixedBordersFilename << std::endl;
        saveImage(sResultYMixedBordersFilename, oHostDstY);
        std::cout << "Saved image: " << sResultYMixedBordersFilename << std::endl;

        // diff the original 8u_C1 result images with border control and the mixed
        // border control images, they should match (diff image will be all black)

        NPP_CHECK_NPP(nppiAbsDiff_8u_C1R_Ctx(oDeviceDstOutXMixedBorders.data(),
                                             oDeviceDstOutXMixedBorders.pitch(),
                                             oDeviceDstOutX.data(),
                                             oDeviceDstOutX.pitch(),
                                             oDeviceDstOutXDiff.data(),
                                             oDeviceDstOutXDiff.pitch(),
                                             oSizeROI,
                                             nppStreamCtx));

        NPP_CHECK_NPP(nppiAbsDiff_8u_C1R_Ctx(oDeviceDstOutYMixedBorders.data(),
                                             oDeviceDstOutYMixedBorders.pitch(),
                                             oDeviceDstOutY.data(),
                                             oDeviceDstOutY.pitch(),
                                             oDeviceDstOutYDiff.data(),
                                             oDeviceDstOutYDiff.pitch(),
                                             oSizeROI,
                                             nppStreamCtx));

        // create additional output files
        std::string sResultXMixedDiffFilename = sResultBaseFilename;
        std::string sResultYMixedDiffFilename = sResultBaseFilename;

        sResultXMixedDiffFilename += "_gradientVectorPrewittBorderX_Vertical_MixedBorderDiffs.pgm";
        sResultYMixedDiffFilename += "_gradientVectorPrewittBorderY_Horizontal_MixedBorderDiffs.pgm";

        // copy the device result data into the host output images
        oDeviceDstOutXDiff.copyTo(oHostDstX.data(), oHostDstX.pitch());
        oDeviceDstOutYDiff.copyTo(oHostDstY.data(), oHostDstY.pitch());

        saveImage(sResultXMixedDiffFilename, oHostDstX);
        std::cout << "Saved image: " << sResultXMixedDiffFilename << std::endl;
        saveImage(sResultYMixedDiffFilename, oHostDstY);
        std::cout << "Saved image: " << sResultYMixedDiffFilename << std::endl;

        nppiFree(oDeviceSrc.data());
        nppiFree(oDeviceDstX.data());
        nppiFree(oDeviceDstY.data());
        nppiFree(oDeviceDstOutX.data());
        nppiFree(oDeviceDstOutY.data());
        nppiFree(oDeviceDstOutXNoBorders.data());
        nppiFree(oDeviceDstOutYNoBorders.data());
        nppiFree(oDeviceDstOutXDiff.data());
        nppiFree(oDeviceDstOutYDiff.data());
        nppiFree(oDeviceDstOutXMixedBorders.data());
        nppiFree(oDeviceDstOutYMixedBorders.data());
        nppiFree(oEnlargedDeviceSrc.data());

        cudaDeviceReset();
        exit(EXIT_SUCCESS);
    }
    catch (npp::Exception &rException) {
        std::cerr << "Program error! The following exception occurred: \n";
        std::cerr << rException << std::endl;
        std::cerr << "Aborting." << std::endl;

        cudaDeviceReset();
        exit(EXIT_FAILURE);
    }
    catch (...) {
        std::cerr << "Program error! An unknow type of exception occurred. \n";
        std::cerr << "Aborting." << std::endl;

        cudaDeviceReset();
        exit(EXIT_FAILURE);
        return -1;
    }

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/FilterBorderControlNPP/FilterBorderControlNPP.cpp`.

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

- **Total Lines**: 603
- **Approximate Size**: 31837 bytes

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
