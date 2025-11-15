# Keywords: Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp
---

**Total Keywords**: 83

---

## A

### AdapterLuid {#adapterluid}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `12deviceluid = desc.AdapterLuid;
    }

    // Desc`

### AllocateAndInitializeSid {#allocateandinitializesid}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `_SID_AUTHORITY;
    AllocateAndInitializeSid(&sidIdentifierAutho`


## B

### BlendState {#blendstate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `T);
        psoDesc.BlendState                    `

### BufferCount {#buffercount}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `;
    swapChainDesc.BufferCount           = FrameCo`

### BufferLocation {#bufferlocation}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` m_vertexBufferView.BufferLocation = m_vertexBuffer->G`

### BufferUsage {#bufferusage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `;
    swapChainDesc.BufferUsage           = DXGI_US`


## C

### ClearRenderTargetView {#clearrendertargetview}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `
    m_commandList->ClearRenderTargetView(rtvHandle, clearCol`

### CloseHandle {#closehandle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `ndleDesc));
        CloseHandle(sharedHandle);

   `

### ComPtr {#comptr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `vice.
    {
        ComPtr<ID3D12Debug> debugC`

### CreateCommandAllocator {#createcommandallocator}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateCommandAllocator(D3D12_COMMAND_LIST_`

### CreateCommandList {#createcommandlist}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateCommandList(0,
                `

### CreateCommandQueue {#createcommandqueue}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateCommandQueue(&queueDesc, IID_PPV`

### CreateCommittedResource {#createcommittedresource}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateCommittedResource(&CD3DX12_HEAP_PROPE`

### CreateDXGIFactory2 {#createdxgifactory2}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `;
    ThrowIfFailed(CreateDXGIFactory2(dxgiFactoryFlags, I`

### CreateDescriptorHeap {#createdescriptorheap}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateDescriptorHeap(&rtvHeapDesc, IID_P`

### CreateEvent {#createevent}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `     m_fenceEvent = CreateEvent(nullptr, FALSE, FAL`

### CreateFence {#createfence}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `          m_device->CreateFence(m_fenceValues[m_fra`

### CreateGraphicsPipelineState {#creategraphicspipelinestate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateGraphicsPipelineState(&psoDesc, IID_PPV_A`

### CreateRenderTargetView {#createrendertargetview}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `          m_device->CreateRenderTargetView(m_renderTargets[n].`

### CreateRootSignature {#createrootsignature}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateRootSignature(
            0, pSi`

### CreateSharedHandle {#createsharedhandle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `wIfFailed(m_device->CreateSharedHandle(
            m_vert`

### CreateSwapChainForHwnd {#createswapchainforhwnd}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `owIfFailed(factory->CreateSwapChainForHwnd(m_commandQueue.Get(`


## D

### DepthStencilState {#depthstencilstate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `T);
        psoDesc.DepthStencilState                  = `

### DrawInstanced {#drawinstanced}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `
    m_commandList->DrawInstanced(vertBufHeight * ver`


## E

### EnableDebugLayer {#enabledebuglayer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `   debugController->EnableDebugLayer();

            // `

### EnumWarpAdapter {#enumwarpadapter}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `owIfFailed(factory->EnumWarpAdapter(IID_PPV_ARGS(&warpA`

### ExecuteCommandList {#executecommandlist}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `   // However, when ExecuteCommandList() is called on a pa`

### ExecuteCommandLists {#executecommandlists}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `    m_commandQueue->ExecuteCommandLists(_countof(ppCommandL`


## F

### FrameCount {#framecount}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `erCount           = FrameCount;
    swapChainDesc.`

### FreeSid {#freesid}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` (*ppSID) {
        FreeSid(*ppSID);
    }
    `


## G

### GetAddressOf {#getaddressof}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `RSION_1, pSignature.GetAddressOf(), pError.GetAddres`

### GetAssetFullPath {#getassetfullpath}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `:wstring filePath = GetAssetFullPath("shaders.hlsl");
  `

### GetBuffer {#getbuffer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `Failed(m_swapChain->GetBuffer(n, IID_PPV_ARGS(&m_`

### GetBufferPointer {#getbufferpointer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `     0, pSignature->GetBufferPointer(), pSignature->GetB`

### GetBufferSize {#getbuffersize}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `nter(), pSignature->GetBufferSize(), IID_PPV_ARGS(&m_`

### GetCPUDescriptorHandleForHeapStart {#getcpudescriptorhandleforheapstart}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `tvHandle(m_rtvHeap->GetCPUDescriptorHandleForHeapStart());

        // Cre`

### GetCompletedValue {#getcompletedvalue}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `y.
    if (m_fence->GetCompletedValue() < m_fenceValues[m`

### GetCurrentBackBufferIndex {#getcurrentbackbufferindex}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `ndex = m_swapChain->GetCurrentBackBufferIndex();

    // Create d`

### GetDesc1 {#getdesc1}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `   hardwareAdapter->GetDesc1(&desc);
        m_d`

### GetDescriptorHandleIncrementSize {#getdescriptorhandleincrementsize}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `torSize = m_device->GetDescriptorHandleIncrementSize(D3D12_DESCRIPTOR_HE`

### GetGPUVirtualAddress {#getgpuvirtualaddress}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `n = m_vertexBuffer->GetGPUVirtualAddress();
        m_vertex`

### GetHardwareAdapter {#gethardwareadapter}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `areAdapter;
        GetHardwareAdapter(factory.Get(), &har`

### GetHwnd {#gethwnd}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `  Win32Application::GetHwnd(),
                `

### GetLastError {#getlasterror}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `(HRESULT_FROM_WIN32(GetLastError()));
        }

   `

### GetResourceAllocationInfo {#getresourceallocationinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `          m_device->GetResourceAllocationInfo(m_nodeMask, 1, &CD3`


## H

### HighPart {#highpart}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `p(&m_dx12deviceluid.HighPart,
                  `


## I

### InitAsDescriptorTable {#initasdescriptortable}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `;
        parameter.InitAsDescriptorTable(1, &range, D3D12_SH`

### InitCuda {#initcuda}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `LoadPipeline();
    InitCuda();
    LoadAssets()`

### InitializeSecurityDescriptor {#initializesecuritydescriptor}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `zeof(PSID *));

    InitializeSecurityDescriptor(m_winPSecurityDescr`

### InputLayout {#inputlayout}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `{};
        psoDesc.InputLayout                    `


## L

### LoadAssets {#loadassets}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `    InitCuda();
    LoadAssets();
}

// Load the r`

### LoadPipeline {#loadpipeline}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `rop::OnInit()
{
    LoadPipeline();
    InitCuda();
`

### LocalFree {#localfree}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` (*ppACL) {
        LocalFree(*ppACL);
    }
    `

### LowPart {#lowpart}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `p(&m_dx12deviceluid.LowPart, devProp.luid, size`


## M

### MakeWindowAssociation {#makewindowassociation}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `owIfFailed(factory->MakeWindowAssociation(Win32Application::G`

### MoveToNextFrame {#movetonextframe}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `tFenceValue));

    MoveToNextFrame();
}

void DX12Cuda`


## N

### NumDescriptors {#numdescriptors}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `        rtvHeapDesc.NumDescriptors             = Frame`

### NumRenderTargets {#numrendertargets}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `NT;
        psoDesc.NumRenderTargets                   =`


## O

### OnDestroy {#ondestroy}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `id DX12CudaInterop::OnDestroy()
{
    // Ensure t`

### OnInit {#oninit}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `id DX12CudaInterop::OnInit()
{
    LoadPipelin`

### OnRender {#onrender}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `id DX12CudaInterop::OnRender()
{
    // Record a`


## P

### PopulateCommandList {#populatecommandlist}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `e command list.
    PopulateCommandList();

    // Execute `

### PrimitiveTopologyType {#primitivetopologytype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `AX;
        psoDesc.PrimitiveTopologyType              = D3D1`


## R

### RasterizerState {#rasterizerstate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `));
        psoDesc.RasterizerState                    `

### ResourceBarrier {#resourcebarrier}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `
    m_commandList->ResourceBarrier(1,
                `

### RunSineWaveKernel {#runsinewavekernel}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `fferDesc));
        RunSineWaveKernel(vertBufWidth, vertB`


## S

### SampleDesc {#sampledesc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `;
    swapChainDesc.SampleDesc.Count      = 1;

  `

### SampleMask {#samplemask}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `T);
        psoDesc.SampleMask                    `

### SetEntriesInAcl {#setentriesinacl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `LPTSTR)*ppSID;

    SetEntriesInAcl(1, &explicitAccess,`

### SetEventOnCompletion {#seteventoncompletion}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `owIfFailed(m_fence->SetEventOnCompletion(m_fenceValues[m_fra`

### SetGraphicsRootSignature {#setgraphicsrootsignature}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `
    m_commandList->SetGraphicsRootSignature(m_rootSignature.Get`

### SetSecurityDescriptorDacl {#setsecuritydescriptordacl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` NULL, ppACL);

    SetSecurityDescriptorDacl(m_winPSecurityDescr`

### ShaderStructs {#shaderstructs}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` <wrl.h>

#include "ShaderStructs.h"
#include "d3dx12`

### SizeInBytes {#sizeinbytes}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` m_vertexBufferView.SizeInBytes    = vertexBufferSi`

### StrideInBytes {#strideinbytes}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: ` m_vertexBufferView.StrideInBytes  = sizeof(Vertex);
`

### SwapEffect {#swapeffect}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `;
    swapChainDesc.SwapEffect            = DXGI_S`


## T

### ThrowIfFailed {#throwiffailed}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `tory4> factory;
    ThrowIfFailed(CreateDXGIFactory2(`

### TrusteeForm {#trusteeform}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `licitAccess.Trustee.TrusteeForm  = TRUSTEE_IS_SID;
`

### TrusteeType {#trusteetype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `licitAccess.Trustee.TrusteeType  = TRUSTEE_IS_WELL_`


## W

### WaitForGpu {#waitforgpu}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `continuing.
        WaitForGpu();
    }
}

// Rend`

### WaitForSingleObjectEx {#waitforsingleobjectex}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `m_fenceEvent));
    WaitForSingleObjectEx(m_fenceEvent, INFIN`

### WindowsSecurityAttributes {#windowssecurityattributes}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `class WindowsSecurityAttributes`


## Z

### ZeroMemory {#zeromemory}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/simpleD3D12.cpp](./simpleD3D12.cpp_docs.md)
- **Context**: `explicitAccess;
    ZeroMemory(&explicitAccess, si`

