# Keywords: Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h
---

**Total Keywords**: 58

---

## B

### ByteCount {#bytecount}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `viceptr src, size_t ByteCount);
    typedef CUres`

### ByteOffset {#byteoffset}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `fSetAddress(size_t *ByteOffset, CUtexref hTexRef, `


## C

### CUDAAPI {#cudaapi}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDAAPI
#endif

    /**
     * \defgroup CUDA_INITIALIZE Initialization
     *
     * This s`

### CUDA_ARRAY3D_2DARRAY {#cudaarray3d2darray}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY3D_2DARRAY 0x01

/**
 * This flag must be set in order to bind a surface reference`

### CUDA_ARRAY3D_DESCRIPTOR {#cudaarray3ddescriptor}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY3D_DESCRIPTOR    CUDA_ARRAY3D_DESCRIPTOR_v1
#endif /* CUDA_FORCE_LEGACY32_INTERNAL`

### CUDA_ARRAY3D_DESCRIPTOR_st {#cudaarray3ddescriptorst}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY3D_DESCRIPTOR_st CUDA_ARRAY3D_DESCRIPTOR_v1_st
#define CUDA_ARRAY3D_DESCRIPTOR    `

### CUDA_ARRAY3D_LAYERED {#cudaarray3dlayered}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY3D_LAYERED 0x01

/**
 * Deprecated, use CUDA_ARRAY3D_LAYERED
 */
#define CUDA_ARRA`

### CUDA_ARRAY3D_SURFACE_LDST {#cudaarray3dsurfaceldst}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY3D_SURFACE_LDST 0x02

/**
 * Override the texref format with a format inferred fro`

### CUDA_ARRAY_DESCRIPTOR {#cudaarraydescriptor}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY_DESCRIPTOR      CUDA_ARRAY_DESCRIPTOR_v1
#define CUDA_ARRAY3D_DESCRIPTOR_st CUDA_`

### CUDA_ARRAY_DESCRIPTOR_st {#cudaarraydescriptorst}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_ARRAY_DESCRIPTOR_st   CUDA_ARRAY_DESCRIPTOR_v1_st
#define CUDA_ARRAY_DESCRIPTOR      CU`

### CUDA_CB {#cudacb}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_CB
#endif

    /**
     * CUDA stream callback
     * \param hStream The stream the cal`

### CUDA_MEMCPY2D {#cudamemcpy2d}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_MEMCPY2D              CUDA_MEMCPY2D_v1
#define CUDA_MEMCPY3D_st           CUDA_MEMCPY3D`

### CUDA_MEMCPY2D_st {#cudamemcpy2dst}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_MEMCPY2D_st           CUDA_MEMCPY2D_v1_st
#define CUDA_MEMCPY2D              CUDA_MEMCP`

### CUDA_MEMCPY3D {#cudamemcpy3d}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_MEMCPY3D              CUDA_MEMCPY3D_v1
#define CUDA_ARRAY_DESCRIPTOR_st   CUDA_ARRAY_DE`

### CUDA_MEMCPY3D_PEER_st {#cudamemcpy3dpeerst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUDA_MEMCPY3D_PEER_st`

### CUDA_MEMCPY3D_st {#cudamemcpy3dst}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_MEMCPY3D_st           CUDA_MEMCPY3D_v1_st
#define CUDA_MEMCPY3D              CUDA_MEMCP`

### CUDA_POINTER_ATTRIBUTE_P2P_TOKENS_st {#cudapointerattributep2ptokensst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUDA_POINTER_ATTRIBUTE_P2P_TOKENS_st`

### CUDA_RESOURCE_DESC_st {#cudaresourcedescst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUDA_RESOURCE_DESC_st`

### CUDA_RESOURCE_VIEW_DESC_st {#cudaresourceviewdescst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUDA_RESOURCE_VIEW_DESC_st`

### CUDA_TEXTURE_DESC_st {#cudatexturedescst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUDA_TEXTURE_DESC_st`

### CUDA_VERSION {#cudaversion}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUDA_VERSION 3020 /* 3.2 */

#ifdef __cplusplus
extern "C"
{
#endif

/**
 * CUDA device poin`

### CU_IPC_HANDLE_SIZE {#cuipchandlesize}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_IPC_HANDLE_SIZE 64

    typedef struct CUipcEventHandle_st
    {
        char reserved[CU`

### CU_LAUNCH_PARAM_BUFFER_POINTER {#culaunchparambufferpointer}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_LAUNCH_PARAM_BUFFER_POINTER ((void *)0x01)

/**
 * Indicator that the next value in the \`

### CU_LAUNCH_PARAM_BUFFER_SIZE {#culaunchparambuffersize}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_LAUNCH_PARAM_BUFFER_SIZE ((void *)0x02)

/**
 * For texture references loaded into the mo`

### CU_LAUNCH_PARAM_END {#culaunchparamend}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_LAUNCH_PARAM_END ((void *)0x00)

/**
 * Indicator that the next value in the \p extra par`

### CU_MEMHOSTALLOC_DEVICEMAP {#cumemhostallocdevicemap}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_MEMHOSTALLOC_DEVICEMAP 0x02

/**
 * If set, host memory is allocated as write-combined - `

### CU_MEMHOSTALLOC_PORTABLE {#cumemhostallocportable}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_MEMHOSTALLOC_PORTABLE 0x01

/**
 * If set, host memory is mapped into CUDA address space `

### CU_MEMHOSTALLOC_WRITECOMBINED {#cumemhostallocwritecombined}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_MEMHOSTALLOC_WRITECOMBINED 0x04

/**
 * If set, host memory is portable between CUDA cont`

### CU_MEMHOSTREGISTER_DEVICEMAP {#cumemhostregisterdevicemap}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_MEMHOSTREGISTER_DEVICEMAP 0x02

/**
 * If set, peer memory is mapped into CUDA address sp`

### CU_MEMHOSTREGISTER_PORTABLE {#cumemhostregisterportable}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_MEMHOSTREGISTER_PORTABLE 0x01

/**
 * If set, host memory is mapped into CUDA address spa`

### CU_MEMPEERREGISTER_DEVICEMAP {#cumempeerregisterdevicemap}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_MEMPEERREGISTER_DEVICEMAP 0x02
#endif

#if __CUDA_API_VERSION >= 3020

    /**
     * 2D `

### CU_PARAM_TR_DEFAULT {#cuparamtrdefault}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_PARAM_TR_DEFAULT -1

    /** @} */ /* END CUDA_TYPES */

#if defined(WIN32) || defined(_W`

### CU_TRSA_OVERRIDE_FORMAT {#cutrsaoverrideformat}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_TRSA_OVERRIDE_FORMAT 0x01

/**
 * Read the texture as integers rather than promoting the `

### CU_TRSF_NORMALIZED_COORDINATES {#cutrsfnormalizedcoordinates}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_TRSF_NORMALIZED_COORDINATES 0x02

/**
 * Perform sRGB->linear conversion during texture r`

### CU_TRSF_READ_AS_INTEGER {#cutrsfreadasinteger}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_TRSF_READ_AS_INTEGER 0x01

/**
 * Use normalized texture coordinates in the range [0,1) i`

### CU_TRSF_SRGB {#cutrsfsrgb}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CU_TRSF_SRGB 0x10

/**
 * For texture references loaded into the module, use default texunit`

### CUarray_st {#cuarrayst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUarray_st`

### CUctx_st {#cuctxst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUctx_st`

### CUdeviceptr {#cudeviceptr}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define CUdeviceptr                CUdeviceptr_v1
#define CUDA_MEMCPY2D_st           CUDA_MEMCPY2D_v`

### CUdevprop_st {#cudevpropst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUdevprop_st`

### CUevent_st {#cueventst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUevent_st`

### CUfunc_st {#cufuncst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUfunc_st`

### CUgraphicsResource_st {#cugraphicsresourcest}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUgraphicsResource_st`

### CUipcEventHandle_st {#cuipceventhandlest}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUipcEventHandle_st`

### CUipcMemHandle_st {#cuipcmemhandlest}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUipcMemHandle_st`

### CUmipmappedArray_st {#cumipmappedarrayst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUmipmappedArray_st`

### CUmod_st {#cumodst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUmod_st`

### CUstream_st {#custreamst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUstream_st`

### CUsurfref_st {#cusurfrefst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUsurfref_st`

### CUtexref_st {#cutexrefst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUtexref_st`

### CUuuid_st {#cuuuidst}

- **Type**: type
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `struct CUuuid_st`


## E

### ElementSizeBytes {#elementsizebytes}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `       unsigned int ElementSizeBytes);
#else
typedef CUr`


## N

### NumChannels {#numchannels}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `     unsigned int   NumChannels; /**< Channels per `

### NumPackedComponents {#numpackedcomponents}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `ray_format fmt, int NumPackedComponents);
    typedef CUres`


## W

### WidthInBytes {#widthinbytes}

- **Type**: identifier
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: ` */

        size_t WidthInBytes; /**< Width of 2D m`


## _

### __CUDA_API_VERSION {#cudaapiversion}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define __CUDA_API_VERSION 5000

/**
 * \defgroup CUDA_DRIVER CUDA Driver API
 *
 * This section des`

### __cuda_cuda_h__ {#cudacudah}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define __cuda_cuda_h__ 1

/**
 * CUDA API versioning support
 */
#define __CUDA_API_VERSION 5000

/`

### __cuda_drvapi_dynlink_cuda_h__ {#cudadrvapidynlinkcudah}

- **Type**: macro
- **File**: [Samples/0_Introduction/matrixMulDynlinkJIT/cuda_drvapi_dynlink_cuda.h](./cuda_drvapi_dynlink_cuda.h_docs.md)
- **Context**: `#define __cuda_drvapi_dynlink_cuda_h__

#include <stdlib.h>


#define __cuda_cuda_h__ 1

/**
 * CUDA`

