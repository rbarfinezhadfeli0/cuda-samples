# Documentation: Samples/2_Concepts_and_Techniques/eigenvalues/matlab.h
---
## File Metadata
- **Path**: `Samples/2_Concepts_and_Techniques/eigenvalues/matlab.h`
- **Filename**: `matlab.h`
- **Language**: h
- **Size**: 5179 bytes
- **Lines**: 121
- **Generated**: 2025-11-15 12:53:54 UTC

---
## Original Source
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

/* Header for utility functionality.
 * Host code.
 */

#ifndef _MATLAB_H_
#define _MATLAB_H_

// includes, system
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// includes, project

////////////////////////////////////////////////////////////////////////////////
//! Write a tridiagonal, symmetric matrix in vector representation and
//! it's eigenvalues
//! @param  filename  name of output file
//! @param  d  diagonal entries of the matrix
//! @param  s  superdiagonal entries of the matrix (len = n - 1)
//! @param  eigenvals  eigenvalues of the matrix
//! @param  indices  vector of len n containing the position of the eigenvalues
//!                  if these are sorted in ascending order
//! @param  n  size of the matrix
////////////////////////////////////////////////////////////////////////////////
extern "C" void writeTridiagSymMatlab(const char *filename, float *d, float *s, float *eigenvals, const unsigned int n);

////////////////////////////////////////////////////////////////////////////////
//! Write matrix to a file in Matlab format
//! @param  file  file handle to which to write he matrix
//! @param  mat_name  name of matrix in Matlab
//! @param  mat  matrix to write to the file
//! @param  mat_size  size of the (square) matrix \a mat
////////////////////////////////////////////////////////////////////////////////
template <class T, class S> void writeMatrixMatlab(T &file, const char *mat_name, S *&mat, const unsigned int mat_size);

////////////////////////////////////////////////////////////////////////////////
//! Write vector to a file in Matlab format
//! @param  file  file handle to which to write he matrix
//! @param  vec_name  name of vector in Matlab
//! @param  vec  matrix to write to the file
//! @param  vec_len  length of the vector
////////////////////////////////////////////////////////////////////////////////
template <class T, class S> void writeVectorMatlab(T &file, const char *vec_name, S *&vec, const unsigned int vec_len);

// implementations

////////////////////////////////////////////////////////////////////////////////
//! Write matrix to a file in Matlab format
//! @param  file  file handle to which to write he matrix
//! @param  mat_name  name of matrix in Matlab
//! @param  mat  matrix to write to the file
//! @param  mat_size  size of the (square) matrix \a mat
////////////////////////////////////////////////////////////////////////////////
template <class T, class S> void writeMatrixMatlab(T &file, const char *mat_name, S *&mat, const unsigned int mat_size)
{
    const unsigned int pitch = sizeof(S) * mat_size;

    file << mat_name << " = [";

    for (unsigned int i = 0; i < mat_size; ++i) {
        for (unsigned int j = 0; j < mat_size; ++j) {
            file << getMatrix(mat, pitch, i, j) << " ";
        }

        if (i != mat_size - 1) {
            file << "; ";
        }
    }

    file << "];\n";
}

////////////////////////////////////////////////////////////////////////////////
//! Write vector to a file in Matlab format
//! @param  file  file handle to which to write he matrix
//! @param  vec_name  name of vector in Matlab
//! @param  vec  matrix to write to the file
//! @param  vec_len  length of the vector
////////////////////////////////////////////////////////////////////////////////
template <class T, class S> void writeVectorMatlab(T &file, const char *vec_name, S *&vec, const unsigned int vec_len)
{
    file << vec_name << " = [";

    for (unsigned int i = 0; i < vec_len; ++i) {
        file << vec[i] << " ";
    }

    file << "];\n";
}

#endif // _MATLAB_H_

```

---
## High-Level Overview
This file is a h source file with 2 function(s) in the CUDA Samples repository.

**Dependencies**: 4 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `math.h`
- `stdio.h`
- `stdlib.h`
- `string.h`

### Preprocessor Definitions
- **_MATLAB_H_**: `// includes, system`

### Functions
#### `void writeMatrixMatlab(T &file, const char *mat_name, S *&mat, const unsigned int mat_size)`
- Function in Samples/2_Concepts_and_Techniques/eigenvalues/matlab.h

#### `void writeVectorMatlab(T &file, const char *vec_name, S *&vec, const unsigned int vec_len)`
- Function in Samples/2_Concepts_and_Techniques/eigenvalues/matlab.h


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


---
## Performance & Security Notes
### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

