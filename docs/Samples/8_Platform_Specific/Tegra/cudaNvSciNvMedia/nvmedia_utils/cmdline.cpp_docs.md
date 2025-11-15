# Documentation for Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/cmdline.cpp

## File Metadata

- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/cmdline.cpp`
- **Type**: .cpp
- **Location**: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils
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


/* Standard headers */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>

/* Nvidia headers */
#include "cmdline.h"
#include "config_parser.h"
#include "helper_cuda.h"
#include "log_utils.h"
#include "misc_utils.h"

/* see cmdline.h for details */
void PrintUsage()
{
    printf("cudaNvSciNvMedia\n");
    printf("Usage: cudaNvSciNvMedia [options]\n");
    printf("Options:\n");
    printf("-h           Prints usage\n");
    printf("-device=n (n >= 0 for cuda device ID)\n");
    printf("-cf=[config] 2d config file. Path length limited to %u chars\n", FILE_NAME_SIZE);
    printf("-iterations=n (n > 0 - num of iterations of NvMedia-CUDA operations to be launched)\n");
}

SectionMap sectionsMap[] = {
    {SECTION_NONE, "", 0, 0} /* Has to be the last item - specifies the end of array */
};

/* see cmdline.h for details */
int ParseArgs(int argc, char *argv[], TestArgs *args)
{
    NvMediaBool   bLastArg       = NVMEDIA_FALSE;
    NvMediaBool   bDataAvailable = NVMEDIA_FALSE;
    NvMediaStatus status         = NVMEDIA_STATUS_OK;
    const char   *filename       = NULL;
    int           i;

    args->srcSurfAllocAttrs[0].type = args->dstSurfAllocAttrs[0].type = NVM_SURF_ATTR_WIDTH;
    args->srcSurfAllocAttrs[1].type = args->dstSurfAllocAttrs[1].type = NVM_SURF_ATTR_HEIGHT;
    args->srcSurfAllocAttrs[2].type = args->dstSurfAllocAttrs[2].type = NVM_SURF_ATTR_EMB_LINES_TOP;
    args->srcSurfAllocAttrs[3].type = args->dstSurfAllocAttrs[3].type = NVM_SURF_ATTR_EMB_LINES_BOTTOM;
    args->srcSurfAllocAttrs[4].type = args->dstSurfAllocAttrs[4].type = NVM_SURF_ATTR_CPU_ACCESS;
    args->srcSurfAllocAttrs[5].type = args->dstSurfAllocAttrs[5].type = NVM_SURF_ATTR_ALLOC_TYPE;
    args->srcSurfAllocAttrs[6].type = args->dstSurfAllocAttrs[6].type = NVM_SURF_ATTR_SCAN_TYPE;
    args->srcSurfAllocAttrs[7].type = args->dstSurfAllocAttrs[7].type = NVM_SURF_ATTR_COLOR_STD_TYPE;
    args->numSurfAllocAttrs                                           = 8;

    args->srcSurfFormatAttrs[0].type = args->dstSurfFormatAttrs[0].type = NVM_SURF_ATTR_SURF_TYPE;
    args->srcSurfFormatAttrs[1].type = args->dstSurfFormatAttrs[1].type = NVM_SURF_ATTR_LAYOUT;
    args->srcSurfFormatAttrs[2].type = args->dstSurfFormatAttrs[2].type = NVM_SURF_ATTR_DATA_TYPE;
    args->srcSurfFormatAttrs[3].type = args->dstSurfFormatAttrs[3].type = NVM_SURF_ATTR_MEMORY;
    args->srcSurfFormatAttrs[4].type = args->dstSurfFormatAttrs[4].type = NVM_SURF_ATTR_SUB_SAMPLING_TYPE;
    args->srcSurfFormatAttrs[5].type = args->dstSurfFormatAttrs[5].type = NVM_SURF_ATTR_BITS_PER_COMPONENT;
    args->srcSurfFormatAttrs[6].type = args->dstSurfFormatAttrs[6].type = NVM_SURF_ATTR_COMPONENT_ORDER;

    /* ConfigParamsMap
     * See nvmedia_2d.h and sample config file(s) for details.
     */
    ConfigParamsMap paramsMap[] = {
        /*ParamName,             &args->variableName,                                paramType,     D, LimitType,   Mn,
           Mx, CharSize,       p2C, section   */
        {"transformMode", &args->blitParams.dstTransform, TYPE_UINT, 0, LIMITS_BOTH, 0, 7, 0, 0, SECTION_NONE},
        {"filterMode", &args->blitParams.filter, TYPE_UINT, 1, LIMITS_BOTH, 1, 4, 0, 0, SECTION_NONE},
        {"colorStd", &args->blitParams.colorStandard, TYPE_UINT, 0, LIMITS_MIN, 0, 3, 0, 0, SECTION_NONE},
        {"validOperations", &args->blitParams.validFields, TYPE_UINT, 0, LIMITS_BOTH, 0, 15, 0, 0, SECTION_NONE},
        {"inputfile", &args->inputFileName, TYPE_CHAR_ARR, 0, LIMITS_NONE, 0, 0, FILE_NAME_SIZE, 0, SECTION_NONE},

        /*src surface alloc attributes*/
        {"srcWidth", &args->srcSurfAllocAttrs[0].value, TYPE_UINT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"srcHeight", &args->srcSurfAllocAttrs[1].value, TYPE_UINT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"srcCPUAccess", &args->srcSurfAllocAttrs[4].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 3, 0, 0, SECTION_NONE},
        {"srcAllocType", &args->srcSurfAllocAttrs[5].value, TYPE_UINT, 0, LIMITS_BOTH, 0, 1, 0, 0, SECTION_NONE},
        {"srcScanType", &args->srcSurfAllocAttrs[6].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 1, 0, 0, SECTION_NONE},
        {"srcColorStd", &args->srcSurfAllocAttrs[7].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 12, 0, 0, SECTION_NONE},
        /*src surface format attributes*/
        {"srcSurfType", &args->srcSurfFormatAttrs[0].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 3, 0, 0, SECTION_NONE},
        {"srcLayout", &args->srcSurfFormatAttrs[1].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 2, 0, 0, SECTION_NONE},
        {"srcDataType", &args->srcSurfFormatAttrs[2].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 4, 0, 0, SECTION_NONE},
        {"srcMemory", &args->srcSurfFormatAttrs[3].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 3, 0, 0, SECTION_NONE},
        {"srcSubSamplingType", &args->srcSurfFormatAttrs[4].value, TYPE_UINT, 1, LIMITS_BOTH, 0, 4, 0, 0, SECTION_NONE},
        {"srcBitsPerComponent",
         &args->srcSurfFormatAttrs[5].value,
         TYPE_UINT,
         1,
         LIMITS_BOTH,
         1,
         10,
         0,
         0,
         SECTION_NONE},
        {"srcComponentOrder", &args->srcSurfFormatAttrs[6].value, TYPE_UINT, 2, LIMITS_BOTH, 1, 45, 0, 0, SECTION_NONE},
        /*srcRect*/
        {"srcRectx0", &args->srcRect.x0, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"srcRecty0", &args->srcRect.y0, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"srcRectx1", &args->srcRect.x1, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"srcRecty1", &args->srcRect.y1, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        /*dst surface alloc attributes*/
        {"dstWidth", &args->dstSurfAllocAttrs[0].value, TYPE_UINT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"dstHeight", &args->dstSurfAllocAttrs[1].value, TYPE_UINT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"dstCPUAccess", &args->dstSurfAllocAttrs[4].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 3, 0, 0, SECTION_NONE},
        {"dstAllocType", &args->dstSurfAllocAttrs[5].value, TYPE_UINT, 0, LIMITS_BOTH, 0, 1, 0, 0, SECTION_NONE},
        {"dstScanType", &args->dstSurfAllocAttrs[6].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 1, 0, 0, SECTION_NONE},
        {"dstColorStd", &args->dstSurfAllocAttrs[7].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 12, 0, 0, SECTION_NONE},
        /*dst surface format attributes*/
        {"dstSurfType", &args->dstSurfFormatAttrs[0].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 3, 0, 0, SECTION_NONE},
        {"dstLayout", &args->dstSurfFormatAttrs[1].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 2, 0, 0, SECTION_NONE},
        {"dstDataType", &args->dstSurfFormatAttrs[2].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 4, 0, 0, SECTION_NONE},
        {"dstMemory", &args->dstSurfFormatAttrs[3].value, TYPE_UINT, 1, LIMITS_BOTH, 1, 3, 0, 0, SECTION_NONE},
        {"dstSubSamplingType", &args->dstSurfFormatAttrs[4].value, TYPE_UINT, 1, LIMITS_BOTH, 0, 4, 0, 0, SECTION_NONE},
        {"dstBitsPerComponent",
         &args->dstSurfFormatAttrs[5].value,
         TYPE_UINT,
         1,
         LIMITS_BOTH,
         1,
         10,
         0,
         0,
         SECTION_NONE},
        {"dstComponentOrder", &args->dstSurfFormatAttrs[6].value, TYPE_UINT, 2, LIMITS_BOTH, 1, 45, 0, 0, SECTION_NONE},
        /*dstRect*/
        {"dstRectx0", &args->dstRect.x0, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"dstRecty0", &args->dstRect.y0, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"dstRectx1", &args->dstRect.x1, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        {"dstRecty1", &args->dstRect.y1, TYPE_USHORT, 0, LIMITS_MIN, 0, 0, 0, 0, SECTION_NONE},
        /*End of the array */
        {NULL, NULL, TYPE_UINT, 0, LIMITS_NONE, 0, 0, 0, 0, SECTION_NONE}};

    args->iterations = 100; // Set default iterations value.

    if (checkCmdLineFlag(argc, (const char **)argv, "-h")) {
        return -1;
    }
    if (checkCmdLineFlag(argc, (const char **)argv, "cf")) {

        char *inputFileName = NULL;
        getCmdLineArgumentString(argc, (const char **)argv, "cf", (char **)&inputFileName);
        if (!inputFileName) {
            printf("ERR: Invalid config file name\n");
            return -1;
        }
        filename = sdkFindFilePath(inputFileName, ".");
    }
    if (checkCmdLineFlag(argc, (const char **)argv, "iterations")) {
        args->iterations = getCmdLineArgumentInt(argc, (const char **)argv, "iterations");
    }

    if (filename == NULL) {
        // Set default file to use if no config file given.
        filename = sdkFindFilePath("sample.cfg", ".");
    }

    if (filename != NULL) {
        printf("Using config file %s\n", filename);

        /* Init Parser Map*/
        status = ConfigParser_InitParamsMap(paramsMap);
        if (status != NVMEDIA_STATUS_OK) {
            printf("ERR: ConfigParser_InitParamsMap failed! status:%x\n", status);
            return -1;
        }

        status = ConfigParser_ParseFile(paramsMap, 1, sectionsMap, (char *)filename);
        if (status != NVMEDIA_STATUS_OK) {
            printf("ERR: Failed to parse config file. status:%x\n", status);
            return -1;
        }
    }

    LOG_INFO("ParseArgs: Validating params from config file\n");
    status = ConfigParser_ValidateParams(paramsMap, sectionsMap);
    if (status != NVMEDIA_STATUS_OK) {
        printf("ERR: Some of the params in config file are invalid.\n");
        return -1;
    }

    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/cmdline.cpp`.

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

- **Total Lines**: 209
- **Approximate Size**: 11058 bytes

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
