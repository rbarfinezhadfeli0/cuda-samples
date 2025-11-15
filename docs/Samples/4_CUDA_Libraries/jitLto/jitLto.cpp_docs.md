# Documentation for Samples/4_CUDA_Libraries/jitLto/jitLto.cpp

## File Metadata

- **Path**: `Samples/4_CUDA_Libraries/jitLto/jitLto.cpp`
- **Type**: .cpp
- **Location**: Samples/4_CUDA_Libraries/jitLto
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

#include <cstring>
#include <cuda.h>
#include <iostream>
#include <nvJitLink.h>
#include <nvrtc.h>

#define NUM_THREADS 128
#define NUM_BLOCKS  32

#define NVRTC_SAFE_CALL(x)                                                                            \
    do {                                                                                              \
        nvrtcResult result = x;                                                                       \
        if (result != NVRTC_SUCCESS) {                                                                \
            std::cerr << "\nerror: " #x " failed with error " << nvrtcGetErrorString(result) << '\n'; \
            exit(1);                                                                                  \
        }                                                                                             \
    } while (0)
#define CUDA_SAFE_CALL(x)                                                     \
    do {                                                                      \
        CUresult result = x;                                                  \
        if (result != CUDA_SUCCESS) {                                         \
            const char *msg;                                                  \
            cuGetErrorName(result, &msg);                                     \
            std::cerr << "\nerror: " #x " failed with error " << msg << '\n'; \
            exit(1);                                                          \
        }                                                                     \
    } while (0)
#define NVJITLINK_SAFE_CALL(h, x)                                                \
    do {                                                                         \
        nvJitLinkResult result = x;                                              \
        if (result != NVJITLINK_SUCCESS) {                                       \
            std::cerr << "\nerror: " #x " failed with error " << result << '\n'; \
            size_t lsize;                                                        \
            result = nvJitLinkGetErrorLogSize(h, &lsize);                        \
            if (result == NVJITLINK_SUCCESS && lsize > 0) {                      \
                char *log = (char *)malloc(lsize);                               \
                result    = nvJitLinkGetErrorLog(h, log);                        \
                if (result == NVJITLINK_SUCCESS) {                               \
                    std::cerr << "error log: " << log << '\n';                   \
                    free(log);                                                   \
                }                                                                \
            }                                                                    \
            exit(1);                                                             \
        }                                                                        \
    } while (0)

const char *lto_saxpy = "                                       \n\
extern __device__ float compute(float a, float x, float y);     \n\
                                                                \n\
extern \"C\" __global__                                         \n\
void saxpy(float a, float *x, float *y, float *out, size_t n)   \n\
{                                                               \n\
  size_t tid = blockIdx.x * blockDim.x + threadIdx.x;           \n\
  if (tid < n) {                                                \n\
    out[tid] = compute(a, x[tid], y[tid]);                      \n\
  }                                                             \n\
}                                                               \n";

const char *lto_compute = "                                     \n\
__device__  float compute(float a, float x, float y) {          \n\
  return a * x + y;                                             \n\
}                                                               \n";

// compile code into LTOIR, returning the IR and its size
static void getLTOIR(const char *code, const char *name, char **ltoIR, size_t *ltoIRSize)
{
    // Create an instance of nvrtcProgram with the code string.
    nvrtcProgram prog;
    NVRTC_SAFE_CALL(nvrtcCreateProgram(&prog,  // prog
                                       code,   // buffer
                                       name,   // name
                                       0,      // numHeaders
                                       NULL,   // headers
                                       NULL)); // includeNames

    // specify that LTO IR should be generated for LTO operation
    const char *opts[]        = {"-dlto", "--relocatable-device-code=true"};
    nvrtcResult compileResult = nvrtcCompileProgram(prog,  // prog
                                                    2,     // numOptions
                                                    opts); // options
    // Obtain compilation log from the program.
    size_t logSize;
    NVRTC_SAFE_CALL(nvrtcGetProgramLogSize(prog, &logSize));
    char *log = new char[logSize];
    NVRTC_SAFE_CALL(nvrtcGetProgramLog(prog, log));
    std::cout << log << '\n';
    delete[] log;
    if (compileResult != NVRTC_SUCCESS) {
        exit(1);
    }
    // Obtain generated LTO IR from the program.
    NVRTC_SAFE_CALL(nvrtcGetLTOIRSize(prog, ltoIRSize));
    *ltoIR = new char[*ltoIRSize];
    NVRTC_SAFE_CALL(nvrtcGetLTOIR(prog, *ltoIR));
    // Destroy the program.
    NVRTC_SAFE_CALL(nvrtcDestroyProgram(&prog));
}

int main(int argc, char *argv[])
{
    unsigned int    cuda_major = 0;
    unsigned int    cuda_minor = 0;
    nvJitLinkResult res        = nvJitLinkVersion(&cuda_major, &cuda_minor);
    if (res != NVJITLINK_SUCCESS) {
        std::cerr << "Version check failed" << '\n';
    }
    else {
        std::cout << "CUDA " << cuda_major << "." << cuda_minor << '\n';
    }


    char  *ltoIR1;
    char  *ltoIR2;
    size_t ltoIR1Size;
    size_t ltoIR2Size;
    // getLTOIR uses nvrtc to get the LTOIR.
    // We could also use nvcc offline with -dlto -fatbin
    // to generate the IR, but using nvrtc keeps the build simpler.
    getLTOIR(lto_saxpy, "lto_saxpy.cu", &ltoIR1, &ltoIR1Size);
    getLTOIR(lto_compute, "lto_compute.cu", &ltoIR2, &ltoIR2Size);

    CUdevice          cuDevice;
    CUcontext         context;
    CUmodule          module;
    CUfunction        kernel;
    CUctxCreateParams ctxCreateParams = {};
    CUDA_SAFE_CALL(cuInit(0));
    CUDA_SAFE_CALL(cuDeviceGet(&cuDevice, 0));
    CUDA_SAFE_CALL(cuCtxCreate(&context, &ctxCreateParams, 0, cuDevice));

    // Dynamically determine the arch to link for
    int major = 0;
    int minor = 0;
    CUDA_SAFE_CALL(cuDeviceGetAttribute(&major, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MAJOR, cuDevice));
    CUDA_SAFE_CALL(cuDeviceGetAttribute(&minor, CU_DEVICE_ATTRIBUTE_COMPUTE_CAPABILITY_MINOR, cuDevice));
    int  arch = major * 10 + minor;
    char smbuf[16];
    memset(smbuf, 0, 16);
    sprintf(smbuf, "-arch=sm_%d", arch);

    // Load the generated LTO IR and link them together
    nvJitLinkHandle handle;
    const char     *lopts[] = {"-lto", smbuf};
    NVJITLINK_SAFE_CALL(handle, nvJitLinkCreate(&handle, 2, lopts));

    NVJITLINK_SAFE_CALL(handle,
                        nvJitLinkAddData(handle, NVJITLINK_INPUT_LTOIR, (void *)ltoIR1, ltoIR1Size, "lto_saxpy"));
    NVJITLINK_SAFE_CALL(handle,
                        nvJitLinkAddData(handle, NVJITLINK_INPUT_LTOIR, (void *)ltoIR2, ltoIR2Size, "lto_compute"));

    // The call to nvJitLinkComplete causes linker to link together the two
    // LTO IR modules, do optimization on the linked LTO IR,
    // and generate cubin from it.
    NVJITLINK_SAFE_CALL(handle, nvJitLinkComplete(handle));

    // check error log
    size_t logSize;
    NVJITLINK_SAFE_CALL(handle, nvJitLinkGetErrorLogSize(handle, &logSize));
    if (logSize > 0) {
        char *log = (char *)malloc(logSize + 1);
        NVJITLINK_SAFE_CALL(handle, nvJitLinkGetErrorLog(handle, log));
        std::cout << "Error log: " << log << std::endl;
        free(log);
    }

    // get linked cubin
    size_t cubinSize;
    NVJITLINK_SAFE_CALL(handle, nvJitLinkGetLinkedCubinSize(handle, &cubinSize));
    void *cubin = malloc(cubinSize);
    NVJITLINK_SAFE_CALL(handle, nvJitLinkGetLinkedCubin(handle, cubin));

    NVJITLINK_SAFE_CALL(handle, nvJitLinkDestroy(&handle));
    delete[] ltoIR1;
    delete[] ltoIR2;

    // cubin is linked, so now load it
    CUDA_SAFE_CALL(cuModuleLoadData(&module, cubin));
    CUDA_SAFE_CALL(cuModuleGetFunction(&kernel, module, "saxpy"));

    // Generate input for execution, and create output buffers.
    size_t n          = NUM_THREADS * NUM_BLOCKS;
    size_t bufferSize = n * sizeof(float);
    float  a          = 5.1f;
    float *hX = new float[n], *hY = new float[n], *hOut = new float[n];
    for (size_t i = 0; i < n; ++i) {
        hX[i] = static_cast<float>(i);
        hY[i] = static_cast<float>(i * 2);
    }
    CUdeviceptr dX, dY, dOut;
    CUDA_SAFE_CALL(cuMemAlloc(&dX, bufferSize));
    CUDA_SAFE_CALL(cuMemAlloc(&dY, bufferSize));
    CUDA_SAFE_CALL(cuMemAlloc(&dOut, bufferSize));
    CUDA_SAFE_CALL(cuMemcpyHtoD(dX, hX, bufferSize));
    CUDA_SAFE_CALL(cuMemcpyHtoD(dY, hY, bufferSize));
    // Execute SAXPY.
    void *args[] = {&a, &dX, &dY, &dOut, &n};
    CUDA_SAFE_CALL(cuLaunchKernel(kernel,
                                  NUM_BLOCKS,
                                  1,
                                  1, // grid dim
                                  NUM_THREADS,
                                  1,
                                  1, // block dim
                                  0,
                                  NULL, // shared mem and stream
                                  args,
                                  0)); // arguments
    CUDA_SAFE_CALL(cuCtxSynchronize());
    // Retrieve and print output.
    CUDA_SAFE_CALL(cuMemcpyDtoH(hOut, dOut, bufferSize));

    for (size_t i = 0; i < n; ++i) {
        std::cout << a << " * " << hX[i] << " + " << hY[i] << " = " << hOut[i] << '\n';
    }
    // check last value to verify
    if (hOut[n - 1] == 29074.5) {
        std::cout << "PASSED!\n";
    }
    else {
        std::cout << "values not expected?\n";
    }
    // Release resources.
    CUDA_SAFE_CALL(cuMemFree(dX));
    CUDA_SAFE_CALL(cuMemFree(dY));
    CUDA_SAFE_CALL(cuMemFree(dOut));
    CUDA_SAFE_CALL(cuModuleUnload(module));
    CUDA_SAFE_CALL(cuCtxDestroy(context));
    free(cubin);
    delete[] hX;
    delete[] hY;
    delete[] hOut;
    return 0;
}

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/4_CUDA_Libraries/jitLto/jitLto.cpp`.

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

- **Total Lines**: 261
- **Approximate Size**: 12245 bytes

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
