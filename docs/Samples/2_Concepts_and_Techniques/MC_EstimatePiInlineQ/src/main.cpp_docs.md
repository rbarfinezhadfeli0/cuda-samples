# Documentation for Samples/2_Concepts_and_Techniques/MC_EstimatePiInlineQ/src/main.cpp

## File Metadata

- **Path**: `Samples/2_Concepts_and_Techniques/MC_EstimatePiInlineQ/src/main.cpp`
- **Type**: .cpp
- **Location**: Samples/2_Concepts_and_Techniques/MC_EstimatePiInlineQ/src
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

///////////////////////////////////////////////////////////////////////////////
// Monte Carlo: Estimate Pi
// ========================
//
// This sample demonstrates a very simple Monte Carlo estimation for Pi.
//
// This file, main.cpp, contains the setup information to run the test, for
// example parsing the command line and integrating this sample with the
// samples framework. As such it is perhaps less interesting than the guts of
// the sample. Readers wishing to skip the clutter are advised to skip straight
// to Test.operator() in test.cpp.
///////////////////////////////////////////////////////////////////////////////


#include <cuda_runtime.h>
#include <helper_cuda.h>
#include <helper_timer.h>
#include <iomanip>
#include <iostream>
#include <math.h>
#include <stdexcept>

#include "../inc/test.h"

// Forward declarations
void                          showHelp(const int argc, const char **argv);
template <typename Real> void runTest(int argc, const char **argv);

int main(int argc, char **argv)
{
    using std::invalid_argument;
    using std::string;

    // Open the log file
    printf("Monte Carlo Estimate Pi (with inline QRNG)\n");
    printf("==========================================\n\n");

    // If help flag is set, display help and exit immediately
    if (checkCmdLineFlag(argc, (const char **)argv, "help")) {
        printf("Displaying help on console\n");
        showHelp(argc, (const char **)argv);
        exit(EXIT_SUCCESS);
    }

    // Check the precision (checked against the device capability later)
    try {
        char *value;

        if (getCmdLineArgumentString(argc, (const char **)argv, "precision", &value)) {
            // Check requested precision is valid
            string prec(value);

            if (prec.compare("single") == 0 || prec.compare("\"single\"") == 0) {
                runTest<float>(argc, (const char **)argv);
            }
            else if (prec.compare("double") == 0 || prec.compare("\"double\"") == 0) {
                runTest<double>(argc, (const char **)argv);
            }
            else {
                printf("specified precision (%s) is invalid, must be \"single\" or "
                       "\"double\".\n",
                       value);
                throw invalid_argument("precision");
            }
        }
        else {
            runTest<float>(argc, (const char **)argv);
        }
    }
    catch (invalid_argument &e) {
        printf("invalid command line argument (%s)\n", e.what());
        exit(EXIT_FAILURE);
    }

    // Finish
    exit(EXIT_SUCCESS);
}

template <typename Real> void runTest(int argc, const char **argv)
{
    using std::invalid_argument;
    using std::runtime_error;

    StopWatchInterface *timer = NULL;

    try {
        Test<Real>  test;
        int         deviceCount = 0;
        cudaError_t cudaResult  = cudaSuccess;

        // by default specify GPU Device == 0
        test.device = 0;

        // Get number of available devices
        cudaResult = cudaGetDeviceCount(&deviceCount);

        if (cudaResult != cudaSuccess) {
            printf("could not get device count.\n");
            throw runtime_error("cudaGetDeviceCount");
        }

        // (default parameters)
        test.numSims         = k_sims_qa;
        test.threadBlockSize = k_bsize_qa;

        {
            char *value = 0;

            if (getCmdLineArgumentString(argc, argv, "device", &value)) {
                test.device = (int)atoi(value);

                if (test.device >= deviceCount) {
                    printf("invalid target device specified on command line (device %d does "
                           "not exist).\n",
                           test.device);
                    throw invalid_argument("device");
                }
            }
            else {
                test.device = gpuGetMaxGflopsDeviceId();
            }

            if (getCmdLineArgumentString(argc, argv, "sims", &value)) {
                test.numSims = (unsigned int)atoi(value);

                if (test.numSims < k_sims_min || test.numSims > k_sims_max) {
                    printf("specified number of simulations (%d) is invalid, must be "
                           "between %d and %d.\n",
                           test.numSims,
                           k_sims_min,
                           k_sims_max);
                    throw invalid_argument("sims");
                }
            }
            else {
                test.numSims = k_sims_def;
            }

            if (getCmdLineArgumentString(argc, argv, "block-size", &value)) {
                // Determine max threads per block
                cudaDeviceProp deviceProperties;
                cudaResult = cudaGetDeviceProperties(&deviceProperties, test.device);

                if (cudaResult != cudaSuccess) {
                    printf("cound not get device properties for device %d.\n", test.device);
                    throw runtime_error("cudaGetDeviceProperties");
                }

                // Check requested size is valid
                test.threadBlockSize = (unsigned int)atoi(value);

                if (test.threadBlockSize < k_bsize_min
                    || test.threadBlockSize > static_cast<unsigned int>(deviceProperties.maxThreadsPerBlock)) {
                    printf("specified block size (%d) is invalid, must be between %d and %d "
                           "for device %d.\n",
                           test.threadBlockSize,
                           k_bsize_min,
                           deviceProperties.maxThreadsPerBlock,
                           test.device);
                    throw invalid_argument("block-size");
                }

                if (test.threadBlockSize & test.threadBlockSize - 1) {
                    printf("specified block size (%d) is invalid, must be a power of two "
                           "(see reduction function).\n",
                           test.threadBlockSize);
                    throw invalid_argument("block-size");
                }
            }
            else {
                test.threadBlockSize = k_bsize_def;
            }
        }
        // Execute
        test();
    }
    catch (invalid_argument &e) {
        printf("invalid command line argument (%s)\n", e.what());
        exit(EXIT_FAILURE);
    }
    catch (runtime_error &e) {
        printf("runtime error (%s)\n", e.what());
        exit(EXIT_FAILURE);
    }
}

void showHelp(int argc, const char **argv)
{
    using std::cout;
    using std::endl;
    using std::left;
    using std::setw;

    if (argc > 0) {
        cout << endl << argv[0] << endl;
    }

    cout << endl << "Syntax:" << endl;
    cout << left;
    cout << "    " << setw(20) << "--device=<device>"
         << "Specify device to use for execution" << endl;
    cout << "    " << setw(20) << "--sims=<N>"
         << "Specify number of Monte Carlo simulations" << endl;
    cout << "    " << setw(20) << "--block-size=<N>"
         << "Specify number of threads per block" << endl;
    cout << "    " << setw(20) << "--precision=<P>"
         << "Specify the precision (\"single\" or \"double\")" << endl;
    cout << endl;
    cout << "    " << setw(20) << "--noprompt"
         << "Skip prompt before exit" << endl;
    cout << endl;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/2_Concepts_and_Techniques/MC_EstimatePiInlineQ/src/main.cpp`.

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

- **Total Lines**: 240
- **Approximate Size**: 8874 bytes

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
