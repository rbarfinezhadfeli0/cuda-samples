# Keywords: Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h
---

**Total Keywords**: 56

---

## L

### LINMATH_H {#linmathh}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `#define LINMATH_H

#include <math.h>

// Converts degrees to radians.
#define degreesToRadians(angle`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `pted from Android's OpenGL Matrix.java.       `


## D

### degreesToRadians {#degreestoradians}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `#define degreesToRadians(angleDegrees) (angleDegrees * M_PI / 180.0)

// Converts radians to degrees`


## M

### mat4x4_add {#mat4x4add}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_add(mat4x4 M, mat4x4 a, mat4x4 b)
{`

### mat4x4_col {#mat4x4col}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_col(vec4 r, mat4x4 M, int i)
{`

### mat4x4_dup {#mat4x4dup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_dup(mat4x4 M, mat4x4 N)
{`

### mat4x4_from_quat {#mat4x4fromquat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_from_quat(mat4x4 M, quat q)
{`

### mat4x4_from_vec3_mul_outer {#mat4x4fromvec3mulouter}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_from_vec3_mul_outer(mat4x4 M, vec3 a, vec3 b)
{`

### mat4x4_frustum {#mat4x4frustum}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_frustum(mat4x4 M, float l, float r, float b, float t, float n, float f)
{`

### mat4x4_identity {#mat4x4identity}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_identity(mat4x4 M)
{`

### mat4x4_invert {#mat4x4invert}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_invert(mat4x4 T, mat4x4 M)
{`

### mat4x4_look_at {#mat4x4lookat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_look_at(mat4x4 m, vec3 eye, vec3 center, vec3 up)
{`

### mat4x4_mul {#mat4x4mul}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_mul(mat4x4 M, mat4x4 a, mat4x4 b)
{`

### mat4x4_mul_vec4 {#mat4x4mulvec4}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_mul_vec4(vec4 r, mat4x4 M, vec4 v)
{`

### mat4x4_ortho {#mat4x4ortho}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_ortho(mat4x4 M, float l, float r, float b, float t, float n, float f)
{`

### mat4x4_orthonormalize {#mat4x4orthonormalize}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_orthonormalize(mat4x4 R, mat4x4 M)
{`

### mat4x4_perspective {#mat4x4perspective}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_perspective(mat4x4 m, float y_fov, float aspect, float n, float f)
{`

### mat4x4_rotate {#mat4x4rotate}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_rotate(mat4x4 R, mat4x4 M, float x, float y, float z, float angle)
{`

### mat4x4_rotate_X {#mat4x4rotatex}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_rotate_X(mat4x4 Q, mat4x4 M, float angle)
{`

### mat4x4_rotate_Y {#mat4x4rotatey}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_rotate_Y(mat4x4 Q, mat4x4 M, float angle)
{`

### mat4x4_rotate_Z {#mat4x4rotatez}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_rotate_Z(mat4x4 Q, mat4x4 M, float angle)
{`

### mat4x4_row {#mat4x4row}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_row(vec4 r, mat4x4 M, int i)
{`

### mat4x4_scale {#mat4x4scale}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_scale(mat4x4 M, mat4x4 a, float k)
{`

### mat4x4_scale_aniso {#mat4x4scaleaniso}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_scale_aniso(mat4x4 M, mat4x4 a, float x, float y, float z)
{`

### mat4x4_sub {#mat4x4sub}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_sub(mat4x4 M, mat4x4 a, mat4x4 b)
{`

### mat4x4_translate {#mat4x4translate}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_translate(mat4x4 T, float x, float y, float z)
{`

### mat4x4_translate_in_place {#mat4x4translateinplace}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_translate_in_place(mat4x4 M, float x, float y, float z)
{`

### mat4x4_transpose {#mat4x4transpose}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4_transpose(mat4x4 M, mat4x4 N)
{`

### mat4x4o_mul_quat {#mat4x4omulquat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void mat4x4o_mul_quat(mat4x4 R, mat4x4 M, quat q)
{`


## Q

### quat_add {#quatadd}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_add(quat r, quat a, quat b)
{`

### quat_conj {#quatconj}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_conj(quat r, quat q)
{`

### quat_from_mat4x4 {#quatfrommat4x4}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_from_mat4x4(quat q, mat4x4 M)
{`

### quat_identity {#quatidentity}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_identity(quat q)
{`

### quat_inner_product {#quatinnerproduct}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `float quat_inner_product(quat a, quat b)
{`

### quat_mul {#quatmul}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_mul(quat r, quat p, quat q)
{`

### quat_mul_vec3 {#quatmulvec3}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_mul_vec3(vec3 r, quat q, vec3 v)
{`

### quat_norm {#quatnorm}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `#define quat_norm vec4_norm
static inline void quat_mul_vec3(vec3 r, quat q, vec3 v)
{
    quat v_ =`

### quat_scale {#quatscale}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_scale(quat r, quat v, float s)
{`

### quat_sub {#quatsub}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void quat_sub(quat r, quat a, quat b)
{`


## R

### radiansToDegrees {#radianstodegrees}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `#define radiansToDegrees(angleRadians) (angleRadians * 180.0 / M_PI)

typedef float      vec3[3];
st`


## V

### vec3_add {#vec3add}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec3_add(vec3 r, vec3 const a, vec3 const b)
{`

### vec3_len {#vec3len}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `float vec3_len(vec3 const v) {`

### vec3_mul_cross {#vec3mulcross}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec3_mul_cross(vec3 r, vec3 const a, vec3 const b)
{`

### vec3_mul_inner {#vec3mulinner}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `float vec3_mul_inner(vec3 const a, vec3 const b)
{`

### vec3_norm {#vec3norm}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void  vec3_norm(vec3 r, vec3 const v)
{`

### vec3_reflect {#vec3reflect}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec3_reflect(vec3 r, vec3 const v, vec3 const n)
{`

### vec3_scale {#vec3scale}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec3_scale(vec3 r, vec3 const v, float const s)
{`

### vec3_sub {#vec3sub}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec3_sub(vec3 r, vec3 const a, vec3 const b)
{`

### vec4_add {#vec4add}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec4_add(vec4 r, vec4 const a, vec4 const b)
{`

### vec4_len {#vec4len}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `float vec4_len(vec4 v) {`

### vec4_mul_cross {#vec4mulcross}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec4_mul_cross(vec4 r, vec4 a, vec4 b)
{`

### vec4_mul_inner {#vec4mulinner}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `float vec4_mul_inner(vec4 a, vec4 b)
{`

### vec4_norm {#vec4norm}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void  vec4_norm(vec4 r, vec4 v)
{`

### vec4_reflect {#vec4reflect}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec4_reflect(vec4 r, vec4 v, vec4 n)
{`

### vec4_scale {#vec4scale}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec4_scale(vec4 r, vec4 v, float s)
{`

### vec4_sub {#vec4sub}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/vulkanImageCUDA/linmath.h](./linmath.h_docs.md)
- **Context**: `void vec4_sub(vec4 r, vec4 const a, vec4 const b)
{`

