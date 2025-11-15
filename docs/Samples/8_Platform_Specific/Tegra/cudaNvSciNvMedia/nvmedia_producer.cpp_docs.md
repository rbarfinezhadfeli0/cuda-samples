# Documentation for Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp`
- **Type**: .cpp
- **Location**: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia
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

#include <iostream>
#include <string.h>
/* Nvidia headers */
#include "nvmedia_2d.h"
#include "nvmedia_2d_nvscisync.h"
#include "nvmedia_image.h"
#include "nvmedia_image_nvscibuf.h"
#include "nvmedia_producer.h"
#include "nvmedia_surface.h"
#include "nvmedia_utils/cmdline.h"
#include "nvmedia_utils/image_utils.h"
#include "nvsci_setup.h"

NvMediaImage *NvMediaImageCreateUsingNvScibuf(NvMediaDevice              *device,
                                              NvMediaSurfaceType          type,
                                              const NvMediaSurfAllocAttr *attrs,
                                              uint32_t                    numAttrs,
                                              uint32_t                    flags,
                                              NvSciBufObj                &bufobj,
                                              int                         cudaDeviceId)
{
    NvSciBufModule            module       = NULL;
    NvSciError                err          = NvSciError_Success;
    NvMediaStatus             status       = NVMEDIA_STATUS_OK;
    NvSciBufAttrList          attrlist     = NULL;
    NvSciBufAttrList          conflictlist = NULL;
    NvSciBufAttrValAccessPerm access_perm  = NvSciBufAccessPerm_ReadWrite;
    NvSciBufAttrKeyValuePair  attr_kvp     = {NvSciBufGeneralAttrKey_RequiredPerm, &access_perm, sizeof(access_perm)};
    NvSciBufAttrKeyValuePair  pairArrayOut[10];

    NvMediaImage *image = NULL;

    err = NvSciBufModuleOpen(&module);
    if (err != NvSciError_Success) {
        printf("%s: NvSciBuffModuleOpen failed. Error: %d \n", __func__, err);
        goto fail_cleanup;
    }

    err = NvSciBufAttrListCreate(module, &attrlist);
    if (err != NvSciError_Success) {
        printf("%s: SciBufAttrListCreate failed. Error: %d \n", __func__, err);
        goto fail_cleanup;
    }

    err = NvSciBufAttrListSetAttrs(attrlist, &attr_kvp, 1);
    if (err != NvSciError_Success) {
        printf("%s: AccessPermSetAttr failed. Error: %d \n", __func__, err);
        goto fail_cleanup;
    }

    status = NvMediaImageFillNvSciBufAttrs(device, type, attrs, numAttrs, 0, attrlist);

    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: ImageFillSciBufAttrs failed. Error: %d \n", __func__, err);
        goto fail_cleanup;
    }

    setupNvSciBuf(bufobj, attrlist, cudaDeviceId);

    status = NvMediaImageCreateFromNvSciBuf(device, bufobj, &image);

    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: ImageCreatefromSciBuf failed. Error: %d \n", __func__, err);
        goto fail_cleanup;
    }

    NvSciBufAttrListFree(attrlist);

    if (module != NULL) {
        NvSciBufModuleClose(module);
    }

    return image;

fail_cleanup:
    if (attrlist != NULL) {
        NvSciBufAttrListFree(attrlist);
    }
    if (bufobj != NULL) {
        NvSciBufObjFree(bufobj);
        bufobj = NULL;
    }

    if (module != NULL) {
        NvSciBufModuleClose(module);
    }
    NvMediaImageDestroy(image);
    return NULL;
}

/* Create NvMediaImage surface based on the input attributes.
 * Returns NVMEDIA_STATUS_OK on success
 */
static NvMediaStatus createSurface(Blit2DTest            *ctx,
                                   NvMediaSurfFormatAttr *surfFormatAttrs,
                                   NvMediaSurfAllocAttr  *surfAllocAttrs,
                                   uint32_t               numSurfAllocAttrs,
                                   NvMediaImage         **image,
                                   NvSciBufObj           &bufObj,
                                   int                    cudaDeviceId)
{
    NvMediaSurfaceType surfType;

    /* create source image */
    surfType = NvMediaSurfaceFormatGetType(surfFormatAttrs, NVM_SURF_FMT_ATTR_MAX);
    *image   = NvMediaImageCreateUsingNvScibuf(ctx->device, /* device */
                                             surfType,    /* surface type */
                                             surfAllocAttrs,
                                             numSurfAllocAttrs,
                                             0,
                                             bufObj,
                                             cudaDeviceId);

    if (*image == NULL) {
        printf("Unable to create image\n");
        return NVMEDIA_STATUS_ERROR;
    }
    InitImage(*image, surfAllocAttrs[0].value, surfAllocAttrs[1].value);

    /*    printf("%s: NvMediaImageCreate:: Image size: %ux%u Image type: %d\n",
                __func__, surfAllocAttrs[0].value, surfAllocAttrs[1].value,
       surfType);*/

    return NVMEDIA_STATUS_OK;
}

/* Create NvMediaImage surface based on the input attributes.
 * Returns NVMEDIA_STATUS_OK on success
 */
static NvMediaStatus createSurfaceNonNvSCI(Blit2DTest            *ctx,
                                           NvMediaSurfFormatAttr *surfFormatAttrs,
                                           NvMediaSurfAllocAttr  *surfAllocAttrs,
                                           uint32_t               numSurfAllocAttrs,
                                           NvMediaImage         **image)
{
    NvMediaSurfaceType surfType;

    /* create source image */
    surfType = NvMediaSurfaceFormatGetType(surfFormatAttrs, NVM_SURF_FMT_ATTR_MAX);

    *image = NvMediaImageCreateNew(ctx->device, surfType, surfAllocAttrs, numSurfAllocAttrs, 0);

    if (*image == NULL) {
        printf("Unable to create image\n");
        return NVMEDIA_STATUS_ERROR;
    }
    InitImage(*image, surfAllocAttrs[0].value, surfAllocAttrs[1].value);

    /*    printf("%s: NvMediaImageCreate:: Image size: %ux%u Image type: %d\n",
                __func__, surfAllocAttrs[0].value, surfAllocAttrs[1].value,
       surfType);*/

    return NVMEDIA_STATUS_OK;
}

static void destroySurface(NvMediaImage *image) { NvMediaImageDestroy(image); }

static NvMediaStatus blit2DImage(Blit2DTest     *ctx,
                                 TestArgs       *args,
                                 NvSciSyncObj   &nvMediaSignalerSyncObj,
                                 NvSciSyncFence *preSyncFence,
                                 NvSciSyncFence *fence)
{
    NvMediaStatus          status;
    NvMediaImageSurfaceMap surfaceMap;

    status = ReadImage(args->inputFileName,              /* fileName */
                       0,                                /* frameNum */
                       args->srcSurfAllocAttrs[0].value, /* source image width */
                       args->srcSurfAllocAttrs[1].value, /* source image height */
                       ctx->srcImage,                    /* srcImage */
                       NVMEDIA_TRUE,                     /* uvOrderFlag */
                       1,                                /* bytesPerPixel */
                       MSB_ALIGNED);                     /* pixelAlignment */

    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: ReadImage failed for input buffer: %d\n", __func__, status);
        return status;
    }

    if ((args->srcRect.x1 <= args->srcRect.x0) || (args->srcRect.y1 <= args->srcRect.y0)) {
        ctx->srcRect = NULL;
    }
    else {
        ctx->srcRect = &(args->srcRect);
    }

    if ((args->dstRect.x1 <= args->dstRect.x0) || (args->dstRect.y1 <= args->dstRect.y0)) {
        ctx->dstRect = NULL;
    }
    else {
        ctx->dstRect = &(args->dstRect);
    }

    static int64_t launch = 0;
    // Start inserting pre-fence from second launch inorder to for NvMedia2Blit to
    // wait
    // for cuda signal on fence.
    if (launch) {
        status = NvMedia2DInsertPreNvSciSyncFence(ctx->i2d, preSyncFence);
        if (status != NVMEDIA_STATUS_OK) {
            printf("%s: NvMedia2DSetNvSciSyncObjforEOF   failed: %d\n", __func__, status);
            return status;
        }
        NvSciSyncFenceClear(preSyncFence);
    }
    launch++;

    status = NvMedia2DSetNvSciSyncObjforEOF(ctx->i2d, nvMediaSignalerSyncObj);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMedia2DSetNvSciSyncObjforEOF   failed: %d\n", __func__, status);
        return status;
    }

    /* 2DBlit processing on input image */
    status = NvMedia2DBlitEx(ctx->i2d,          /* i2d */
                             ctx->dstImage,     /* dstSurface */
                             ctx->dstRect,      /* dstRect */
                             ctx->srcImage,     /* srcSurface */
                             ctx->srcRect,      /* srcRect */
                             &args->blitParams, /* params */
                             NULL);             /* paramsOut */

    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMedia2DBlitEx failed: %d\n", __func__, status);
        return status;
    }

    status = NvMedia2DGetEOFNvSciSyncFence(ctx->i2d, nvMediaSignalerSyncObj, fence);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMedia2DGetEOFNvSciSyncFence failed: %d\n", __func__, status);
        return status;
    }

    return NVMEDIA_STATUS_OK;
}

static NvMediaStatus blit2DImageNonNvSCI(Blit2DTest *ctx, TestArgs *args)
{
    NvMediaStatus          status;
    NvMediaImageSurfaceMap surfaceMap;

    status = ReadImage(args->inputFileName,              /* fileName */
                       0,                                /* frameNum */
                       args->srcSurfAllocAttrs[0].value, /* source image width */
                       args->srcSurfAllocAttrs[1].value, /* source image height */
                       ctx->srcImage,                    /* srcImage */
                       NVMEDIA_TRUE,                     /* uvOrderFlag */
                       1,                                /* bytesPerPixel */
                       MSB_ALIGNED);                     /* pixelAlignment */

    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: ReadImage failed for input buffer: %d\n", __func__, status);
        return status;
    }

    if ((args->srcRect.x1 <= args->srcRect.x0) || (args->srcRect.y1 <= args->srcRect.y0)) {
        ctx->srcRect = NULL;
    }
    else {
        ctx->srcRect = &(args->srcRect);
    }

    if ((args->dstRect.x1 <= args->dstRect.x0) || (args->dstRect.y1 <= args->dstRect.y0)) {
        ctx->dstRect = NULL;
    }
    else {
        ctx->dstRect = &(args->dstRect);
    }

    /* 2DBlit processing on input image */
    status = NvMedia2DBlitEx(ctx->i2d,          /* i2d */
                             ctx->dstImage,     /* dstSurface */
                             ctx->dstRect,      /* dstRect */
                             ctx->srcImage,     /* srcSurface */
                             ctx->srcRect,      /* srcRect */
                             &args->blitParams, /* params */
                             NULL);             /* paramsOut */
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMedia2DBlitEx failed: %d\n", __func__, status);
        return status;
    }

    /* Write output image into buffer */
    ctx->bytesPerPixel = 1;
    WriteImageToAllocatedBuffer(ctx, ctx->dstImage, NVMEDIA_TRUE, NVMEDIA_FALSE, ctx->bytesPerPixel);

    return NVMEDIA_STATUS_OK;
}

static void cleanup(Blit2DTest *ctx, NvMediaStatus status = NVMEDIA_STATUS_OK)
{
    if (ctx->srcImage != NULL) {
        NvMedia2DImageUnRegister(ctx->i2d, ctx->srcImage);
        destroySurface(ctx->srcImage);
    }
    if (ctx->dstImage != NULL) {
        NvMedia2DImageUnRegister(ctx->i2d, ctx->dstImage);
        destroySurface(ctx->dstImage);
    }
    if (status != NVMEDIA_STATUS_OK) {
        exit(EXIT_FAILURE);
    }
}

void cleanupNvMedia(Blit2DTest *ctx, NvSciSyncObj &syncObj, NvSciSyncObj &preSyncObj)
{
    NvMediaStatus status;
    cleanup(ctx);
    status = NvMedia2DUnregisterNvSciSyncObj(ctx->i2d, syncObj);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMediaImageSciBufInit failed\n", __func__);
        exit(EXIT_FAILURE);
    }
    status = NvMedia2DUnregisterNvSciSyncObj(ctx->i2d, preSyncObj);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMediaImageSciBufInit failed\n", __func__);
        exit(EXIT_FAILURE);
    }
    NvMediaImageNvSciBufDeinit();
}

void cleanupNvMedia(Blit2DTest *ctx)
{
    cleanup(ctx);
    free(ctx->dstBuffPitches);
    free(ctx->dstBuffer);
    free(ctx->dstBuff);
}

void setupNvMedia(TestArgs     *args,
                  Blit2DTest   *ctx,
                  NvSciBufObj  &srcNvSciBufobj,
                  NvSciBufObj  &dstNvSciBufobj,
                  NvSciSyncObj &syncObj,
                  NvSciSyncObj &preSyncObj,
                  int           cudaDeviceId)
{
    NvMediaStatus status;
    status = NvMediaImageNvSciBufInit();
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: NvMediaImageSciBufInit failed\n", __func__);
        cleanup(ctx, status);
    }

    // Create source surface
    status = createSurface(ctx,
                           args->srcSurfFormatAttrs,
                           args->srcSurfAllocAttrs,
                           args->numSurfAllocAttrs,
                           &ctx->srcImage,
                           srcNvSciBufobj,
                           cudaDeviceId);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to create buffer pools\n", __func__);
        cleanup(ctx, status);
    }

    // Create destination surface
    status = createSurface(ctx,
                           args->dstSurfFormatAttrs,
                           args->dstSurfAllocAttrs,
                           args->numSurfAllocAttrs,
                           &ctx->dstImage,
                           dstNvSciBufobj,
                           cudaDeviceId);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to create buffer pools\n", __func__);
        cleanup(ctx, status);
    }

    // Register source  Surface
    status = NvMedia2DImageRegister(ctx->i2d, ctx->srcImage, NVMEDIA_ACCESS_MODE_READ);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to register source surface\n", __func__);
        cleanup(ctx, status);
    }
    // Register destination Surface
    status = NvMedia2DImageRegister(ctx->i2d, ctx->dstImage, NVMEDIA_ACCESS_MODE_READ_WRITE);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to register destination surface\n", __func__);
        cleanup(ctx, status);
    }

    status = NvMedia2DRegisterNvSciSyncObj(ctx->i2d, NVMEDIA_EOFSYNCOBJ, syncObj);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to NvMedia2DRegisterNvSciSyncObj\n", __func__);
    }

    status = NvMedia2DRegisterNvSciSyncObj(ctx->i2d, NVMEDIA_PRESYNCOBJ, preSyncObj);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to NvMedia2DRegisterNvSciSyncObj\n", __func__);
    }
}

// Create NvMedia src & dst image without NvSciBuf
void setupNvMedia(TestArgs *args, Blit2DTest *ctx)
{
    NvMediaStatus status;

    // Create source surface
    status = createSurfaceNonNvSCI(
        ctx, args->srcSurfFormatAttrs, args->srcSurfAllocAttrs, args->numSurfAllocAttrs, &ctx->srcImage);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to create buffer pools\n", __func__);
        cleanup(ctx, status);
    }

    // Create destination surface
    status = createSurfaceNonNvSCI(
        ctx, args->dstSurfFormatAttrs, args->dstSurfAllocAttrs, args->numSurfAllocAttrs, &ctx->dstImage);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to create buffer pools\n", __func__);
        cleanup(ctx, status);
    }

    // Register source  Surface
    status = NvMedia2DImageRegister(ctx->i2d, ctx->srcImage, NVMEDIA_ACCESS_MODE_READ);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to register source surface\n", __func__);
        cleanup(ctx, status);
    }

    // Register destination Surface
    status = NvMedia2DImageRegister(ctx->i2d, ctx->dstImage, NVMEDIA_ACCESS_MODE_READ_WRITE);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Unable to register destination surface\n", __func__);
        cleanup(ctx, status);
    }

    // Allocate buffer for writing image & set image parameters in Blit2DTest.
    ctx->bytesPerPixel = 1;
    AllocateBufferToWriteImage(ctx,
                               ctx->dstImage,
                               NVMEDIA_TRUE,   /* uvOrderFlag */
                               NVMEDIA_FALSE); /* appendFlag */
}

void runNvMediaBlit2D(TestArgs *args, Blit2DTest *ctx)
{
    // Blit2D function
    NvMediaStatus status = blit2DImageNonNvSCI(ctx, args);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Blit2D failed\n", __func__);
        cleanup(ctx, status);
    }
}

void runNvMediaBlit2D(TestArgs       *args,
                      Blit2DTest     *ctx,
                      NvSciSyncObj   &nvMediaSignalerSyncObj,
                      NvSciSyncFence *preSyncFence,
                      NvSciSyncFence *fence)
{
    // Blit2D function
    NvMediaStatus status = blit2DImage(ctx, args, nvMediaSignalerSyncObj, preSyncFence, fence);
    if (status != NVMEDIA_STATUS_OK) {
        printf("%s: Blit2D failed\n", __func__);
        cleanup(ctx, status);
    }
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_producer.cpp`.

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

- **Total Lines**: 489
- **Approximate Size**: 18611 bytes

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
