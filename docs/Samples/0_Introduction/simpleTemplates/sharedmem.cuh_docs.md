# Documentation: Samples/0_Introduction/simpleTemplates/sharedmem.cuh
---
## File Metadata
- **Path**: `Samples/0_Introduction/simpleTemplates/sharedmem.cuh`
- **Filename**: `sharedmem.cuh`
- **Language**: cuda
- **Size**: 5205 bytes
- **Lines**: 186
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```cuda
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

#ifndef _SHAREDMEM_H_
#define _SHAREDMEM_H_

//****************************************************************************
// Because dynamically sized shared memory arrays are declared "extern",
// we can't templatize them directly.  To get around this, we declare a
// simple wrapper struct that will declare the extern array with a different
// name depending on the type.  This avoids compiler errors about duplicate
// definitions.
//
// To use dynamically allocated shared memory in a templatized __global__ or
// __device__ function, just replace code like this:
//
//
//  template<class T>
//  __global__ void
//  foo( T* g_idata, T* g_odata)
//  {
//      // Shared mem size is determined by the host app at run time
//      extern __shared__  T sdata[];
//      ...
//      doStuff(sdata);
//      ...
//   }
//
//   With this
//  template<class T>
//  __global__ void
//  foo( T* g_idata, T* g_odata)
//  {
//      // Shared mem size is determined by the host app at run time
//      SharedMemory<T> smem;
//      T* sdata = smem.getPointer();
//      ...
//      doStuff(sdata);
//      ...
//   }
//****************************************************************************

// This is the un-specialized struct.  Note that we prevent instantiation of
// this
// struct by putting an undefined symbol in the function body so it won't
// compile.
template <typename T> struct SharedMemory
{
    // Ensure that we won't compile any un-specialized types
    __device__ T *getPointer()
    {
        extern __device__ void error(void);
        error();
        return NULL;
    }
};

// Following are the specializations for the following types.
// int, uint, char, uchar, short, ushort, long, ulong, bool, float, and double
// One could also specialize it for user-defined types.

template <> struct SharedMemory<int>
{
    __device__ int *getPointer()
    {
        extern __shared__ int s_int[];
        return s_int;
    }
};

template <> struct SharedMemory<unsigned int>
{
    __device__ unsigned int *getPointer()
    {
        extern __shared__ unsigned int s_uint[];
        return s_uint;
    }
};

template <> struct SharedMemory<char>
{
    __device__ char *getPointer()
    {
        extern __shared__ char s_char[];
        return s_char;
    }
};

template <> struct SharedMemory<unsigned char>
{
    __device__ unsigned char *getPointer()
    {
        extern __shared__ unsigned char s_uchar[];
        return s_uchar;
    }
};

template <> struct SharedMemory<short>
{
    __device__ short *getPointer()
    {
        extern __shared__ short s_short[];
        return s_short;
    }
};

template <> struct SharedMemory<unsigned short>
{
    __device__ unsigned short *getPointer()
    {
        extern __shared__ unsigned short s_ushort[];
        return s_ushort;
    }
};

template <> struct SharedMemory<long>
{
    __device__ long *getPointer()
    {
        extern __shared__ long s_long[];
        return s_long;
    }
};

template <> struct SharedMemory<unsigned long>
{
    __device__ unsigned long *getPointer()
    {
        extern __shared__ unsigned long s_ulong[];
        return s_ulong;
    }
};

template <> struct SharedMemory<bool>
{
    __device__ bool *getPointer()
    {
        extern __shared__ bool s_bool[];
        return s_bool;
    }
};

template <> struct SharedMemory<float>
{
    __device__ float *getPointer()
    {
        extern __shared__ float s_float[];
        return s_float;
    }
};

template <> struct SharedMemory<double>
{
    __device__ double *getPointer()
    {
        extern __shared__ double s_double[];
        return s_double;
    }
};

#endif //_SHAREDMEM_H_

```

---
## High-Level Overview
This file is a cuda source file and 1 class/struct definition(s) in the CUDA Samples repository.


---
## Detailed Walkthrough
### Preprocessor Definitions
- **_SHAREDMEM_H_**: `//****************************************************************************`

### Classes / Structures
#### `struct SharedMemory`
- Defined in Samples/0_Introduction/simpleTemplates/sharedmem.cuh


---
## Usage Examples
This is a CUDA source file. Typical usage involves:
1. Compiling with nvcc (NVIDIA CUDA Compiler)
2. Linking with CUDA runtime libraries
3. Executing on NVIDIA GPU hardware


---
## Performance & Security Notes
### Performance Considerations
- CUDA kernel execution is asynchronous
- Memory transfers between host and device can be a bottleneck
- Thread block and grid dimensions affect performance

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

