# Documentation for Common/helper_cusolver.h

## File Metadata

- **Path**: `Common/helper_cusolver.h`
- **Type**: .h
- **Location**: Common
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```h
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

#ifndef HELPER_CUSOLVER
#define HELPER_CUSOLVER

#include <ctype.h>
#include <cuda_runtime.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "cusparse.h"

#define SWITCH_CHAR '-'

struct testOpts {
  char *sparse_mat_filename;  // by switch -F<filename>
  const char *testFunc;       // by switch -R<name>
  const char *reorder;        // by switch -P<name>
  int lda;                    // by switch -lda<int>
};

double vec_norminf(int n, const double *x) {
  double norminf = 0;
  for (int j = 0; j < n; j++) {
    double x_abs = fabs(x[j]);
    norminf = (norminf > x_abs) ? norminf : x_abs;
  }
  return norminf;
}

/*
 * |A| = max { |A|*ones(m,1) }
 */
double mat_norminf(int m, int n, const double *A, int lda) {
  double norminf = 0;
  for (int i = 0; i < m; i++) {
    double sum = 0.0;
    for (int j = 0; j < n; j++) {
      double A_abs = fabs(A[i + j * lda]);
      sum += A_abs;
    }
    norminf = (norminf > sum) ? norminf : sum;
  }
  return norminf;
}

/*
 * |A| = max { |A|*ones(m,1) }
 */
double csr_mat_norminf(int m, int n, int nnzA, const cusparseMatDescr_t descrA,
                       const double *csrValA, const int *csrRowPtrA,
                       const int *csrColIndA) {
  const int baseA =
      (CUSPARSE_INDEX_BASE_ONE == cusparseGetMatIndexBase(descrA)) ? 1 : 0;

  double norminf = 0;
  for (int i = 0; i < m; i++) {
    double sum = 0.0;
    const int start = csrRowPtrA[i] - baseA;
    const int end = csrRowPtrA[i + 1] - baseA;
    for (int colidx = start; colidx < end; colidx++) {
      // const int j = csrColIndA[colidx] - baseA;
      double A_abs = fabs(csrValA[colidx]);
      sum += A_abs;
    }
    norminf = (norminf > sum) ? norminf : sum;
  }
  return norminf;
}

void display_matrix(int m, int n, int nnzA, const cusparseMatDescr_t descrA,
                    const double *csrValA, const int *csrRowPtrA,
                    const int *csrColIndA) {
  const int baseA =
      (CUSPARSE_INDEX_BASE_ONE == cusparseGetMatIndexBase(descrA)) ? 1 : 0;

  printf("m = %d, n = %d, nnz = %d, matlab base-1\n", m, n, nnzA);

  for (int row = 0; row < m; row++) {
    const int start = csrRowPtrA[row] - baseA;
    const int end = csrRowPtrA[row + 1] - baseA;
    for (int colidx = start; colidx < end; colidx++) {
      const int col = csrColIndA[colidx] - baseA;
      double Areg = csrValA[colidx];
      printf("A(%d, %d) = %20.16E\n", row + 1, col + 1, Areg);
    }
  }
}

#if defined(_WIN32)
#if !defined(WIN32_LEAN_AND_MEAN)
#define WIN32_LEAN_AND_MEAN
#endif
#include <windows.h>
double second(void) {
  LARGE_INTEGER t;
  static double oofreq;
  static int checkedForHighResTimer;
  static BOOL hasHighResTimer;

  if (!checkedForHighResTimer) {
    hasHighResTimer = QueryPerformanceFrequency(&t);
    oofreq = 1.0 / (double)t.QuadPart;
    checkedForHighResTimer = 1;
  }
  if (hasHighResTimer) {
    QueryPerformanceCounter(&t);
    return (double)t.QuadPart * oofreq;
  } else {
    return (double)GetTickCount() / 1000.0;
  }
}

#elif defined(__linux__) || defined(__QNX__)
#include <stddef.h>
#include <sys/resource.h>
#include <sys/time.h>
double second(void) {
  struct timeval tv;
  gettimeofday(&tv, NULL);
  return (double)tv.tv_sec + (double)tv.tv_usec / 1000000.0;
}

#elif defined(__APPLE__)
#include <stddef.h>
#include <sys/resource.h>
#include <sys/sysctl.h>
#include <sys/time.h>
#include <sys/types.h>
double second(void) {
  struct timeval tv;
  gettimeofday(&tv, NULL);
  return (double)tv.tv_sec + (double)tv.tv_usec / 1000000.0;
}
#else
#error unsupported platform
#endif

#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Common/helper_cusolver.h`.

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

- **Total Lines**: 167
- **Approximate Size**: 5161 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

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

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

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
