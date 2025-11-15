# Keywords: Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h
---

**Total Keywords**: 53

---

## M

### MM_ARRAY_STR {#mmarraystr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_ARRAY_STR      "array"
#define MM_DENSE_STR      "array"
#define MM_COORDINATE_STR "coord`

### MM_COMPLEX_STR {#mmcomplexstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_COMPLEX_STR    "complex"
#define MM_REAL_STR       "real"
#define MM_INT_STR        "inte`

### MM_COORDINATE_STR {#mmcoordinatestr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_COORDINATE_STR "coordinate"
#define MM_SPARSE_STR     "coordinate"
#define MM_COMPLEX_STR`

### MM_COULD_NOT_READ_FILE {#mmcouldnotreadfile}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_COULD_NOT_READ_FILE  11
#define MM_PREMATURE_EOF        12
#define MM_NOT_MTX            `

### MM_COULD_NOT_WRITE_FILE {#mmcouldnotwritefile}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_COULD_NOT_WRITE_FILE 17


    /******************** Matrix Market internal definitions **`

### MM_DENSE_STR {#mmdensestr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_DENSE_STR      "array"
#define MM_COORDINATE_STR "coordinate"
#define MM_SPARSE_STR     "`

### MM_GENERAL_STR {#mmgeneralstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_GENERAL_STR    "general"
#define MM_SYMM_STR       "symmetric"
#define MM_HERM_STR       `

### MM_HERM_STR {#mmhermstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_HERM_STR       "hermitian"
#define MM_SKEW_STR       "skew-symmetric"
#define MM_PATTERN_`

### MM_INT_STR {#mmintstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_INT_STR        "integer"
#define MM_GENERAL_STR    "general"
#define MM_SYMM_STR       "s`

### MM_IO_H {#mmioh}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_IO_H

#include <stdio.h>

#if defined(__cplusplus)
extern "C"
{
#endif /* __cplusplus */
`

### MM_LINE_TOO_LONG {#mmlinetoolong}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_LINE_TOO_LONG        16
#define MM_COULD_NOT_WRITE_FILE 17


    /******************** Ma`

### MM_MAX_LINE_LENGTH {#mmmaxlinelength}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_MAX_LINE_LENGTH  1025
#define MatrixMarketBanner  "%%MatrixMarket"
#define MM_MAX_TOKEN_L`

### MM_MAX_TOKEN_LENGTH {#mmmaxtokenlength}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_MAX_TOKEN_LENGTH 64

    typedef char MM_typecode[4];

    char *mm_typecode_to_str(MM_ty`

### MM_MTX_STR {#mmmtxstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_MTX_STR        "matrix"
#define MM_ARRAY_STR      "array"
#define MM_DENSE_STR      "arra`

### MM_NOT_MTX {#mmnotmtx}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_NOT_MTX              13
#define MM_NO_HEADER            14
#define MM_UNSUPPORTED_TYPE   `

### MM_NO_HEADER {#mmnoheader}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_NO_HEADER            14
#define MM_UNSUPPORTED_TYPE     15
#define MM_LINE_TOO_LONG      `

### MM_PATTERN_STR {#mmpatternstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_PATTERN_STR    "pattern"


    /*  high level routines */
    int mm_read_mtx_crd(char *f`

### MM_PREMATURE_EOF {#mmprematureeof}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_PREMATURE_EOF        12
#define MM_NOT_MTX              13
#define MM_NO_HEADER          `

### MM_REAL_STR {#mmrealstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_REAL_STR       "real"
#define MM_INT_STR        "integer"
#define MM_GENERAL_STR    "gene`

### MM_SKEW_STR {#mmskewstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_SKEW_STR       "skew-symmetric"
#define MM_PATTERN_STR    "pattern"


    /*  high level `

### MM_SPARSE_STR {#mmsparsestr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_SPARSE_STR     "coordinate"
#define MM_COMPLEX_STR    "complex"
#define MM_REAL_STR      `

### MM_SYMM_STR {#mmsymmstr}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_SYMM_STR       "symmetric"
#define MM_HERM_STR       "hermitian"
#define MM_SKEW_STR     `

### MM_UNSUPPORTED_TYPE {#mmunsupportedtype}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MM_UNSUPPORTED_TYPE     15
#define MM_LINE_TOO_LONG        16
#define MM_COULD_NOT_WRITE_FIL`

### MatrixMarket {#matrixmarket}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `ttp://math.nist.gov/MatrixMarket for details.
 *
 *
`

### MatrixMarketBanner {#matrixmarketbanner}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define MatrixMarketBanner  "%%MatrixMarket"
#define MM_MAX_TOKEN_LENGTH 64

    typedef char MM_typ`

### mm_clear_typecode {#mmcleartypecode}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_clear_typecode(typecode) ((*typecode)[0] = (*typecode)[1] = (*typecode)[2] = ' ', (*typec`

### mm_initialize_typecode {#mminitializetypecode}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_initialize_typecode(typecode) mm_clear_typecode(typecode)


    /********************* Ma`

### mm_is_array {#mmisarray}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_array(typecode)      ((typecode)[1] == 'A')

#define mm_is_complex(typecode) ((typecod`

### mm_is_complex {#mmiscomplex}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_complex(typecode) ((typecode)[2] == 'C')
#define mm_is_real(typecode)    ((typecode)[2`

### mm_is_coordinate {#mmiscoordinate}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_coordinate(typecode) ((typecode)[1] == 'C')
#define mm_is_dense(typecode)      ((typec`

### mm_is_dense {#mmisdense}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_dense(typecode)      ((typecode)[1] == 'A')
#define mm_is_array(typecode)      ((typec`

### mm_is_general {#mmisgeneral}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_general(typecode)   ((typecode)[3] == 'G')
#define mm_is_skew(typecode)      ((typecod`

### mm_is_hermitian {#mmishermitian}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_hermitian(typecode) ((typecode)[3] == 'H')

    int mm_is_valid(MM_typecode matcode); `

### mm_is_integer {#mmisinteger}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_integer(typecode) ((typecode)[2] == 'I')

#define mm_is_symmetric(typecode) ((typecode`

### mm_is_matrix {#mmismatrix}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_matrix(typecode) ((typecode)[0] == 'M')

#define mm_is_sparse(typecode)     ((typecode`

### mm_is_pattern {#mmispattern}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_pattern(typecode) ((typecode)[2] == 'P')
#define mm_is_integer(typecode) ((typecode)[2`

### mm_is_real {#mmisreal}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_real(typecode)    ((typecode)[2] == 'R')
#define mm_is_pattern(typecode) ((typecode)[2`

### mm_is_skew {#mmisskew}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_skew(typecode)      ((typecode)[3] == 'K')
#define mm_is_hermitian(typecode) ((typecod`

### mm_is_sparse {#mmissparse}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_sparse(typecode)     ((typecode)[1] == 'C')
#define mm_is_coordinate(typecode) ((typec`

### mm_is_symmetric {#mmissymmetric}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_is_symmetric(typecode) ((typecode)[3] == 'S')
#define mm_is_general(typecode)   ((typecod`

### mm_set_array {#mmsetarray}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_array(typecode)      ((*typecode)[1] = 'A')
#define mm_set_dense(typecode)      mm_se`

### mm_set_complex {#mmsetcomplex}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_complex(typecode) ((*typecode)[2] = 'C')
#define mm_set_real(typecode)    ((*typecode`

### mm_set_coordinate {#mmsetcoordinate}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_coordinate(typecode) ((*typecode)[1] = 'C')
#define mm_set_array(typecode)      ((*ty`

### mm_set_dense {#mmsetdense}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_dense(typecode)      mm_set_array(typecode)
#define mm_set_sparse(typecode)     mm_se`

### mm_set_general {#mmsetgeneral}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_general(typecode)   ((*typecode)[3] = 'G')
#define mm_set_skew(typecode)      ((*type`

### mm_set_hermitian {#mmsethermitian}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_hermitian(typecode) ((*typecode)[3] = 'H')

#define mm_clear_typecode(typecode) ((*ty`

### mm_set_integer {#mmsetinteger}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_integer(typecode) ((*typecode)[2] = 'I')


#define mm_set_symmetric(typecode) ((*type`

### mm_set_matrix {#mmsetmatrix}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_matrix(typecode)     ((*typecode)[0] = 'M')
#define mm_set_coordinate(typecode) ((*ty`

### mm_set_pattern {#mmsetpattern}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_pattern(typecode) ((*typecode)[2] = 'P')
#define mm_set_integer(typecode) ((*typecode`

### mm_set_real {#mmsetreal}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_real(typecode)    ((*typecode)[2] = 'R')
#define mm_set_pattern(typecode) ((*typecode`

### mm_set_skew {#mmsetskew}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_skew(typecode)      ((*typecode)[3] = 'K')
#define mm_set_hermitian(typecode) ((*type`

### mm_set_sparse {#mmsetsparse}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_sparse(typecode)     mm_set_coordinate(typecode)

#define mm_set_complex(typecode) ((`

### mm_set_symmetric {#mmsetsymmetric}

- **Type**: macro
- **File**: [Samples/4_CUDA_Libraries/cuSolverSp_LowlevelQR/mmio.h](./mmio.h_docs.md)
- **Context**: `#define mm_set_symmetric(typecode) ((*typecode)[3] = 'S')
#define mm_set_general(typecode)   ((*type`

