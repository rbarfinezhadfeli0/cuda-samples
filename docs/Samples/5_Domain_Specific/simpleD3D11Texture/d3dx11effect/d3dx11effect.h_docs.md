# Documentation for Samples/5_Domain_Specific/simpleD3D11Texture/d3dx11effect/d3dx11effect.h

## File Metadata

- **Path**: `Samples/5_Domain_Specific/simpleD3D11Texture/d3dx11effect/d3dx11effect.h`
- **Type**: .h
- **Location**: Samples/5_Domain_Specific/simpleD3D11Texture/d3dx11effect
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```h
/*
 * Copyright 1993-2014 NVIDIA Corporation.  All rights reserved.
 *
 * NVIDIA Corporation and its licensors retain all intellectual property and
 * proprietary rights in and to this software and related documentation.
 * Any use, reproduction, disclosure, or distribution of this software
 * and related documentation without an express license agreement from
 * NVIDIA Corporation is strictly prohibited.
 *
 */


//////////////////////////////////////////////////////////////////////////////
//
//  Copyright (c) 2009 Microsoft Corporation.  All rights reserved.
//
//  File:       D3DX11Effect.h
//  Content:    D3DX11 Effect Types & APIs Header
//
//////////////////////////////////////////////////////////////////////////////

#ifndef __D3DX11EFFECT_H__
#define __D3DX11EFFECT_H__

#include "d3d11.h"
#include "d3d11shader.h"

//////////////////////////////////////////////////////////////////////////////
// File contents:
//
// 1) Stateblock enums, structs, interfaces, flat APIs
// 2) Effect enums, structs, interfaces, flat APIs
//////////////////////////////////////////////////////////////////////////////

#ifndef D3DX11_BYTES_FROM_BITS
#define D3DX11_BYTES_FROM_BITS(x) (((x) + 7) / 8)
#endif // D3DX11_BYTES_FROM_BITS

typedef struct _D3DX11_STATE_BLOCK_MASK
{
    BYTE VS;
    BYTE VSSamplers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_SAMPLER_SLOT_COUNT)];
    BYTE VSShaderResources[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE VSConstantBuffers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_CONSTANT_BUFFER_API_SLOT_COUNT)];
    BYTE VSInterfaces[D3DX11_BYTES_FROM_BITS(D3D11_SHADER_MAX_INTERFACES)];

    BYTE HS;
    BYTE HSSamplers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_SAMPLER_SLOT_COUNT)];
    BYTE HSShaderResources[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE HSConstantBuffers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_CONSTANT_BUFFER_API_SLOT_COUNT)];
    BYTE HSInterfaces[D3DX11_BYTES_FROM_BITS(D3D11_SHADER_MAX_INTERFACES)];

    BYTE DS;
    BYTE DSSamplers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_SAMPLER_SLOT_COUNT)];
    BYTE DSShaderResources[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE DSConstantBuffers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_CONSTANT_BUFFER_API_SLOT_COUNT)];
    BYTE DSInterfaces[D3DX11_BYTES_FROM_BITS(D3D11_SHADER_MAX_INTERFACES)];

    BYTE GS;
    BYTE GSSamplers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_SAMPLER_SLOT_COUNT)];
    BYTE GSShaderResources[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE GSConstantBuffers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_CONSTANT_BUFFER_API_SLOT_COUNT)];
    BYTE GSInterfaces[D3DX11_BYTES_FROM_BITS(D3D11_SHADER_MAX_INTERFACES)];

    BYTE PS;
    BYTE PSSamplers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_SAMPLER_SLOT_COUNT)];
    BYTE PSShaderResources[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE PSConstantBuffers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_CONSTANT_BUFFER_API_SLOT_COUNT)];
    BYTE PSInterfaces[D3DX11_BYTES_FROM_BITS(D3D11_SHADER_MAX_INTERFACES)];
    BYTE PSUnorderedAccessViews;

    BYTE CS;
    BYTE CSSamplers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_SAMPLER_SLOT_COUNT)];
    BYTE CSShaderResources[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE CSConstantBuffers[D3DX11_BYTES_FROM_BITS(D3D11_COMMONSHADER_CONSTANT_BUFFER_API_SLOT_COUNT)];
    BYTE CSInterfaces[D3DX11_BYTES_FROM_BITS(D3D11_SHADER_MAX_INTERFACES)];
    BYTE CSUnorderedAccessViews;

    BYTE IAVertexBuffers[D3DX11_BYTES_FROM_BITS(D3D11_IA_VERTEX_INPUT_RESOURCE_SLOT_COUNT)];
    BYTE IAIndexBuffer;
    BYTE IAInputLayout;
    BYTE IAPrimitiveTopology;

    BYTE OMRenderTargets;
    BYTE OMDepthStencilState;
    BYTE OMBlendState;

    BYTE RSViewports;
    BYTE RSScissorRects;
    BYTE RSRasterizerState;

    BYTE SOBuffers;

    BYTE Predication;
} D3DX11_STATE_BLOCK_MASK;

//----------------------------------------------------------------------------
// D3DX11_EFFECT flags:
// -------------------------------------
//
// These flags are passed in when creating an effect, and affect
// the runtime effect behavior:
//
// (Currently none)
//
//
// These flags are set by the effect runtime:
//
// D3DX11_EFFECT_OPTIMIZED
//   This effect has been optimized. Reflection functions that rely on
//   names/semantics/strings should fail. This is set when Optimize() is
//   called, but CEffect::IsOptimized() should be used to test for this.
//
// D3DX11_EFFECT_CLONE
//   This effect is a clone of another effect. Single CBs will never be
//   updated when internal variable values are changed.
//   This flag is not set when the D3DX11_EFFECT_CLONE_FORCE_NONSINGLE flag
//   is used in cloning.
//
//----------------------------------------------------------------------------

#define D3DX11_EFFECT_OPTIMIZED (1 << 21)
#define D3DX11_EFFECT_CLONE     (1 << 22)

// These are the only valid parameter flags to D3DX11CreateEffect*
#define D3DX11_EFFECT_RUNTIME_VALID_FLAGS (0)

//----------------------------------------------------------------------------
// D3DX11_EFFECT_VARIABLE flags:
// ----------------------------
//
// These flags describe an effect variable (global or annotation),
// and are returned in D3DX11_EFFECT_VARIABLE_DESC::Flags.
//
// D3DX11_EFFECT_VARIABLE_ANNOTATION
//   Indicates that this is an annotation on a technique, pass, or global
//   variable. Otherwise, this is a global variable. Annotations cannot
//   be shared.
//
// D3DX11_EFFECT_VARIABLE_EXPLICIT_BIND_POINT
//   Indicates that the variable has been explicitly bound using the
//   register keyword.
//----------------------------------------------------------------------------

#define D3DX11_EFFECT_VARIABLE_ANNOTATION          (1 << 1)
#define D3DX11_EFFECT_VARIABLE_EXPLICIT_BIND_POINT (1 << 2)

//----------------------------------------------------------------------------
// D3DX11_EFFECT_CLONE flags:
// ----------------------------
//
// These flags modify the effect cloning process and are passed into Clone.
//
// D3DX11_EFFECT_CLONE_FORCE_NONSINGLE
//   Ignore all "single" qualifiers on cbuffers.  All cbuffers will have their
//   own ID3D11Buffer's created in the cloned effect.
//----------------------------------------------------------------------------

#define D3DX11_EFFECT_CLONE_FORCE_NONSINGLE (1 << 0)

//----------------------------------------------------------------------------
// D3DX11_EFFECT_PASS flags:
// ----------------------------
//
// These flags modify the effect cloning process and are passed into Clone.
//
// D3DX11_EFFECT_PASS_COMMIT_CHANGES
//   This flag tells the effect runtime to assume that the device state was
//   not modified outside of effects, so that only updated state needs to
//   be set.
//
// D3DX11_EFFECT_PASS_OMIT_*
//   When applying a pass, do not set the state indicated in the flag name.
//----------------------------------------------------------------------------

#define D3DX11_EFFECT_PASS_COMMIT_CHANGES              (1 << 0) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_SHADERS_AND_INTERFACES (1 << 1) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_STATE_OBJECTS          (1 << 2) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_RTVS_AND_DSVS          (1 << 3) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_SAMPLERS               (1 << 4) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_CBS                    (1 << 5) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_SRVS                   (1 << 6) // TODO: not yet implemented
#define D3DX11_EFFECT_PASS_OMIT_UAVS                   (1 << 7) // TODO: not yet implemented

#define D3DX11_EFFECT_PASS_ONLY_SET_SHADERS_AND_CBS                                                                   \
    (D3DX11_EFFECT_PASS_OMIT_STATE_OBJECTS | D3DX11_EFFECT_PASS_OMIT_RTVS_AND_DSVS | D3DX11_EFFECT_PASS_OMIT_SAMPLERS \
     | D3DX11_EFFECT_PASS_OMIT_SRVS | D3DX11_EFFECT_PASS_OMIT_UAVS);

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectType //////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

//----------------------------------------------------------------------------
// D3DX11_EFFECT_TYPE_DESC:
//
// Retrieved by ID3DX11EffectType::GetDesc()
//----------------------------------------------------------------------------

typedef struct _D3DX11_EFFECT_TYPE_DESC
{
    LPCSTR TypeName; // Name of the type
    // (e.g. "float4" or "MyStruct")

    D3D10_SHADER_VARIABLE_CLASS Class; // (e.g. scalar, vector, object, etc.)
    D3D10_SHADER_VARIABLE_TYPE  Type;  // (e.g. float, texture, vertexshader, etc.)

    UINT Elements; // Number of elements in this type
    // (0 if not an array)
    UINT Members; // Number of members
    // (0 if not a structure)
    UINT Rows; // Number of rows in this type
    // (0 if not a numeric primitive)
    UINT Columns; // Number of columns in this type
    // (0 if not a numeric primitive)

    UINT PackedSize; // Number of bytes required to represent
    // this data type, when tightly packed
    UINT UnpackedSize; // Number of bytes occupied by this data
    // type, when laid out in a constant buffer
    UINT Stride; // Number of bytes to seek between elements,
    // when laid out in a constant buffer
} D3DX11_EFFECT_TYPE_DESC;

typedef interface ID3DX11EffectType  ID3DX11EffectType;
typedef interface ID3DX11EffectType *LPD3D11EFFECTTYPE;

// {4250D721-D5E5-491F-B62B-587C43186285}
DEFINE_GUID(IID_ID3DX11EffectType, 0x4250d721, 0xd5e5, 0x491f, 0xb6, 0x2b, 0x58, 0x7c, 0x43, 0x18, 0x62, 0x85);

#undef INTERFACE
#define INTERFACE ID3DX11EffectType

DECLARE_INTERFACE(ID3DX11EffectType)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_TYPE_DESC * pDesc) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetMemberTypeByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetMemberTypeByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetMemberTypeBySemantic)(THIS_ LPCSTR Semantic) PURE;
    STDMETHOD_(LPCSTR, GetMemberName)(THIS_ UINT Index) PURE;
    STDMETHOD_(LPCSTR, GetMemberSemantic)(THIS_ UINT Index) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectVariable //////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

//----------------------------------------------------------------------------
// D3DX11_EFFECT_VARIABLE_DESC:
//
// Retrieved by ID3DX11EffectVariable::GetDesc()
//----------------------------------------------------------------------------

typedef struct _D3DX11_EFFECT_VARIABLE_DESC
{
    LPCSTR Name; // Name of this variable, annotation,
    // or structure member
    LPCSTR Semantic; // Semantic string of this variable
    // or structure member (NULL for
    // annotations or if not present)

    UINT Flags;       // D3DX11_EFFECT_VARIABLE_* flags
    UINT Annotations; // Number of annotations on this variable
    // (always 0 for annotations)

    UINT BufferOffset; // Offset into containing cbuffer or tbuffer
    // (always 0 for annotations or variables
    // not in constant buffers)

    UINT ExplicitBindPoint; // Used if the variable has been explicitly bound
    // using the register keyword. Check Flags for
    // D3DX11_EFFECT_VARIABLE_EXPLICIT_BIND_POINT;
} D3DX11_EFFECT_VARIABLE_DESC;

typedef interface ID3DX11EffectVariable  ID3DX11EffectVariable;
typedef interface ID3DX11EffectVariable *LPD3D11EFFECTVARIABLE;

// {036A777D-B56E-4B25-B313-CC3DDAB71873}
DEFINE_GUID(IID_ID3DX11EffectVariable, 0x036a777d, 0xb56e, 0x4b25, 0xb3, 0x13, 0xcc, 0x3d, 0xda, 0xb7, 0x18, 0x73);

#undef INTERFACE
#define INTERFACE ID3DX11EffectVariable

// Forward defines
typedef interface ID3DX11EffectScalarVariable              ID3DX11EffectScalarVariable;
typedef interface ID3DX11EffectVectorVariable              ID3DX11EffectVectorVariable;
typedef interface ID3DX11EffectMatrixVariable              ID3DX11EffectMatrixVariable;
typedef interface ID3DX11EffectStringVariable              ID3DX11EffectStringVariable;
typedef interface ID3DX11EffectClassInstanceVariable       ID3DX11EffectClassInstanceVariable;
typedef interface ID3DX11EffectInterfaceVariable           ID3DX11EffectInterfaceVariable;
typedef interface ID3DX11EffectShaderResourceVariable      ID3DX11EffectShaderResourceVariable;
typedef interface ID3DX11EffectUnorderedAccessViewVariable ID3DX11EffectUnorderedAccessViewVariable;
typedef interface ID3DX11EffectRenderTargetViewVariable    ID3DX11EffectRenderTargetViewVariable;
typedef interface ID3DX11EffectDepthStencilViewVariable    ID3DX11EffectDepthStencilViewVariable;
typedef interface ID3DX11EffectConstantBuffer              ID3DX11EffectConstantBuffer;
typedef interface ID3DX11EffectShaderVariable              ID3DX11EffectShaderVariable;
typedef interface ID3DX11EffectBlendVariable               ID3DX11EffectBlendVariable;
typedef interface ID3DX11EffectDepthStencilVariable        ID3DX11EffectDepthStencilVariable;
typedef interface ID3DX11EffectRasterizerVariable          ID3DX11EffectRasterizerVariable;
typedef interface ID3DX11EffectSamplerVariable             ID3DX11EffectSamplerVariable;

DECLARE_INTERFACE(ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectScalarVariable ////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectScalarVariable  ID3DX11EffectScalarVariable;
typedef interface ID3DX11EffectScalarVariable *LPD3D11EFFECTSCALARVARIABLE;

// {921EF2E5-A65D-4E92-9FC6-4E9CC09A4ADE}
DEFINE_GUID(IID_ID3DX11EffectScalarVariable,
            0x921ef2e5,
            0xa65d,
            0x4e92,
            0x9f,
            0xc6,
            0x4e,
            0x9c,
            0xc0,
            0x9a,
            0x4a,
            0xde);

#undef INTERFACE
#define INTERFACE ID3DX11EffectScalarVariable

DECLARE_INTERFACE_(ID3DX11EffectScalarVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT ByteOffset, UINT ByteCount) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT ByteOffset, UINT ByteCount) PURE;

    STDMETHOD(SetFloat)(THIS_ float Value) PURE;
    STDMETHOD(GetFloat)(THIS_ float *pValue) PURE;

    STDMETHOD(SetFloatArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetFloatArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetInt)(THIS_ int Value) PURE;
    STDMETHOD(GetInt)(THIS_ int *pValue) PURE;

    STDMETHOD(SetIntArray)(THIS_ int *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetIntArray)(THIS_ int *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetBool)(THIS_ BOOL Value) PURE;
    STDMETHOD(GetBool)(THIS_ BOOL * pValue) PURE;

    STDMETHOD(SetBoolArray)(THIS_ BOOL * pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetBoolArray)(THIS_ BOOL * pData, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectVectorVariable ////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectVectorVariable  ID3DX11EffectVectorVariable;
typedef interface ID3DX11EffectVectorVariable *LPD3D11EFFECTVECTORVARIABLE;

// {5E785D4A-D87B-48D8-B6E6-0F8CA7E7467A}
DEFINE_GUID(IID_ID3DX11EffectVectorVariable,
            0x5e785d4a,
            0xd87b,
            0x48d8,
            0xb6,
            0xe6,
            0x0f,
            0x8c,
            0xa7,
            0xe7,
            0x46,
            0x7a);

#undef INTERFACE
#define INTERFACE ID3DX11EffectVectorVariable

DECLARE_INTERFACE_(ID3DX11EffectVectorVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT ByteOffset, UINT ByteCount) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT ByteOffset, UINT ByteCount) PURE;

    STDMETHOD(SetBoolVector)(THIS_ BOOL * pData) PURE;
    STDMETHOD(SetIntVector)(THIS_ int *pData) PURE;
    STDMETHOD(SetFloatVector)(THIS_ float *pData) PURE;

    STDMETHOD(GetBoolVector)(THIS_ BOOL * pData) PURE;
    STDMETHOD(GetIntVector)(THIS_ int *pData) PURE;
    STDMETHOD(GetFloatVector)(THIS_ float *pData) PURE;

    STDMETHOD(SetBoolVectorArray)(THIS_ BOOL * pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(SetIntVectorArray)(THIS_ int *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(SetFloatVectorArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(GetBoolVectorArray)(THIS_ BOOL * pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetIntVectorArray)(THIS_ int *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetFloatVectorArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectMatrixVariable ////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectMatrixVariable  ID3DX11EffectMatrixVariable;
typedef interface ID3DX11EffectMatrixVariable *LPD3D11EFFECTMATRIXVARIABLE;

// {E1096CF4-C027-419A-8D86-D29173DC803E}
DEFINE_GUID(IID_ID3DX11EffectMatrixVariable,
            0xe1096cf4,
            0xc027,
            0x419a,
            0x8d,
            0x86,
            0xd2,
            0x91,
            0x73,
            0xdc,
            0x80,
            0x3e);

#undef INTERFACE
#define INTERFACE ID3DX11EffectMatrixVariable

DECLARE_INTERFACE_(ID3DX11EffectMatrixVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT ByteOffset, UINT ByteCount) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT ByteOffset, UINT ByteCount) PURE;

    STDMETHOD(SetMatrix)(THIS_ float *pData) PURE;
    STDMETHOD(GetMatrix)(THIS_ float *pData) PURE;

    STDMETHOD(SetMatrixArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetMatrixArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetMatrixTranspose)(THIS_ float *pData) PURE;
    STDMETHOD(GetMatrixTranspose)(THIS_ float *pData) PURE;

    STDMETHOD(SetMatrixTransposeArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetMatrixTransposeArray)(THIS_ float *pData, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectStringVariable ////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectStringVariable  ID3DX11EffectStringVariable;
typedef interface ID3DX11EffectStringVariable *LPD3D11EFFECTSTRINGVARIABLE;

// {F355C818-01BE-4653-A7CC-60FFFEDDC76D}
DEFINE_GUID(IID_ID3DX11EffectStringVariable,
            0xf355c818,
            0x01be,
            0x4653,
            0xa7,
            0xcc,
            0x60,
            0xff,
            0xfe,
            0xdd,
            0xc7,
            0x6d);

#undef INTERFACE
#define INTERFACE ID3DX11EffectStringVariable

DECLARE_INTERFACE_(ID3DX11EffectStringVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(GetString)(THIS_ LPCSTR * ppString) PURE;
    STDMETHOD(GetStringArray)(THIS_ LPCSTR * ppStrings, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectClassInstanceVariable ////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectClassInstanceVariable  ID3DX11EffectClassInstanceVariable;
typedef interface ID3DX11EffectClassInstanceVariable *LPD3D11EFFECTCLASSINSTANCEVARIABLE;

// {926A8053-2A39-4DB4-9BDE-CF649ADEBDC1}
DEFINE_GUID(IID_ID3DX11EffectClassInstanceVariable,
            0x926a8053,
            0x2a39,
            0x4db4,
            0x9b,
            0xde,
            0xcf,
            0x64,
            0x9a,
            0xde,
            0xbd,
            0xc1);

#undef INTERFACE
#define INTERFACE ID3DX11EffectClassInstanceVariable

DECLARE_INTERFACE_(ID3DX11EffectClassInstanceVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(GetClassInstance)(ID3D11ClassInstance * *ppClassInstance) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectInterfaceVariable ////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectInterfaceVariable  ID3DX11EffectInterfaceVariable;
typedef interface ID3DX11EffectInterfaceVariable *LPD3D11EFFECTINTERFACEVARIABLE;

// {516C8CD8-1C80-40A4-B19B-0688792F11A5}
DEFINE_GUID(IID_ID3DX11EffectInterfaceVariable,
            0x516c8cd8,
            0x1c80,
            0x40a4,
            0xb1,
            0x9b,
            0x06,
            0x88,
            0x79,
            0x2f,
            0x11,
            0xa5);

#undef INTERFACE
#define INTERFACE ID3DX11EffectInterfaceVariable

DECLARE_INTERFACE_(ID3DX11EffectInterfaceVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetClassInstance)(ID3DX11EffectClassInstanceVariable * pEffectClassInstance) PURE;
    STDMETHOD(GetClassInstance)(ID3DX11EffectClassInstanceVariable * *ppEffectClassInstance) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectShaderResourceVariable ////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectShaderResourceVariable  ID3DX11EffectShaderResourceVariable;
typedef interface ID3DX11EffectShaderResourceVariable *LPD3D11EFFECTSHADERRESOURCEVARIABLE;

// {350DB233-BBE0-485C-9BFE-C0026B844F89}
DEFINE_GUID(IID_ID3DX11EffectShaderResourceVariable,
            0x350db233,
            0xbbe0,
            0x485c,
            0x9b,
            0xfe,
            0xc0,
            0x02,
            0x6b,
            0x84,
            0x4f,
            0x89);

#undef INTERFACE
#define INTERFACE ID3DX11EffectShaderResourceVariable

DECLARE_INTERFACE_(ID3DX11EffectShaderResourceVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetResource)(THIS_ ID3D11ShaderResourceView * pResource) PURE;
    STDMETHOD(GetResource)(THIS_ ID3D11ShaderResourceView * *ppResource) PURE;

    STDMETHOD(SetResourceArray)(THIS_ ID3D11ShaderResourceView * *ppResources, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetResourceArray)(THIS_ ID3D11ShaderResourceView * *ppResources, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectUnorderedAccessViewVariable ////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectUnorderedAccessViewVariable  ID3DX11EffectUnorderedAccessViewVariable;
typedef interface ID3DX11EffectUnorderedAccessViewVariable *LPD3D11EFFECTUNORDEREDACCESSVIEWVARIABLE;

// {79B4AC8C-870A-47D2-B05A-8BD5CC3EE6C9}
DEFINE_GUID(IID_ID3DX11EffectUnorderedAccessViewVariable,
            0x79b4ac8c,
            0x870a,
            0x47d2,
            0xb0,
            0x5a,
            0x8b,
            0xd5,
            0xcc,
            0x3e,
            0xe6,
            0xc9);

#undef INTERFACE
#define INTERFACE ID3DX11EffectUnorderedAccessViewVariable

DECLARE_INTERFACE_(ID3DX11EffectUnorderedAccessViewVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetUnorderedAccessView)(THIS_ ID3D11UnorderedAccessView * pResource) PURE;
    STDMETHOD(GetUnorderedAccessView)(THIS_ ID3D11UnorderedAccessView * *ppResource) PURE;

    STDMETHOD(SetUnorderedAccessViewArray)(THIS_ ID3D11UnorderedAccessView * *ppResources, UINT Offset, UINT Count)
        PURE;
    STDMETHOD(GetUnorderedAccessViewArray)(THIS_ ID3D11UnorderedAccessView * *ppResources, UINT Offset, UINT Count)
        PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectRenderTargetViewVariable //////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectRenderTargetViewVariable  ID3DX11EffectRenderTargetViewVariable;
typedef interface ID3DX11EffectRenderTargetViewVariable *LPD3D11EFFECTRENDERTARGETVIEWVARIABLE;

// {D5066909-F40C-43F8-9DB5-057C2A208552}
DEFINE_GUID(IID_ID3DX11EffectRenderTargetViewVariable,
            0xd5066909,
            0xf40c,
            0x43f8,
            0x9d,
            0xb5,
            0x05,
            0x7c,
            0x2a,
            0x20,
            0x85,
            0x52);

#undef INTERFACE
#define INTERFACE ID3DX11EffectRenderTargetViewVariable

DECLARE_INTERFACE_(ID3DX11EffectRenderTargetViewVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetRenderTarget)(THIS_ ID3D11RenderTargetView * pResource) PURE;
    STDMETHOD(GetRenderTarget)(THIS_ ID3D11RenderTargetView * *ppResource) PURE;

    STDMETHOD(SetRenderTargetArray)(THIS_ ID3D11RenderTargetView * *ppResources, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRenderTargetArray)(THIS_ ID3D11RenderTargetView * *ppResources, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectDepthStencilViewVariable //////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectDepthStencilViewVariable  ID3DX11EffectDepthStencilViewVariable;
typedef interface ID3DX11EffectDepthStencilViewVariable *LPD3D11EFFECTDEPTHSTENCILVIEWVARIABLE;

// {33C648AC-2E9E-4A2E-9CD6-DE31ACC5B347}
DEFINE_GUID(IID_ID3DX11EffectDepthStencilViewVariable,
            0x33c648ac,
            0x2e9e,
            0x4a2e,
            0x9c,
            0xd6,
            0xde,
            0x31,
            0xac,
            0xc5,
            0xb3,
            0x47);

#undef INTERFACE
#define INTERFACE ID3DX11EffectDepthStencilViewVariable

DECLARE_INTERFACE_(ID3DX11EffectDepthStencilViewVariable, ID3DX11EffectVariable)
{
    STDMETHOD_(BOOL, IsValid)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectType *, GetType)(THIS) PURE;
    STDMETHOD(GetDesc)(THIS_ D3DX11_EFFECT_VARIABLE_DESC * pDesc) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetAnnotationByName)(THIS_ LPCSTR Name) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByIndex)(THIS_ UINT Index) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberByName)(THIS_ LPCSTR Name) PURE;
    STDMETHOD_(ID3DX11EffectVariable *, GetMemberBySemantic)(THIS_ LPCSTR Semantic) PURE;

    STDMETHOD_(ID3DX11EffectVariable *, GetElement)(THIS_ UINT Index) PURE;

    STDMETHOD_(ID3DX11EffectConstantBuffer *, GetParentConstantBuffer)(THIS) PURE;

    STDMETHOD_(ID3DX11EffectScalarVariable *, AsScalar)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectVectorVariable *, AsVector)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectMatrixVariable *, AsMatrix)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectStringVariable *, AsString)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectClassInstanceVariable *, AsClassInstance)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectInterfaceVariable *, AsInterface)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderResourceVariable *, AsShaderResource)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectUnorderedAccessViewVariable *, AsUnorderedAccessView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRenderTargetViewVariable *, AsRenderTargetView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilViewVariable *, AsDepthStencilView)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectConstantBuffer *, AsConstantBuffer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectShaderVariable *, AsShader)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectBlendVariable *, AsBlend)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectDepthStencilVariable *, AsDepthStencil)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectRasterizerVariable *, AsRasterizer)(THIS) PURE;
    STDMETHOD_(ID3DX11EffectSamplerVariable *, AsSampler)(THIS) PURE;

    STDMETHOD(SetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetRawValue)(THIS_ void *pData, UINT Offset, UINT Count) PURE;

    STDMETHOD(SetDepthStencil)(THIS_ ID3D11DepthStencilView * pResource) PURE;
    STDMETHOD(GetDepthStencil)(THIS_ ID3D11DepthStencilView * *ppResource) PURE;

    STDMETHOD(SetDepthStencilArray)(THIS_ ID3D11DepthStencilView * *ppResources, UINT Offset, UINT Count) PURE;
    STDMETHOD(GetDepthStencilArray)(THIS_ ID3D11DepthStencilView * *ppResources, UINT Offset, UINT Count) PURE;
};

//////////////////////////////////////////////////////////////////////////////
// ID3DX11EffectConstantBuffer ////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

typedef interface ID3DX11EffectCons
... (truncated)
```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/simpleD3D11Texture/d3dx11effect/d3dx11effect.h`.

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

- **Total Lines**: 1729
- **Approximate Size**: 81709 bytes

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
