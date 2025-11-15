# Keywords: Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp
---

**Total Keywords**: 77

---

## A

### API {#api}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `D3D API (locate drivers, does not mean device is found)
    {`

### AcquireSync {#acquiresync}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` = g_pKeyedMutex11->AcquireSync(key++, INFINITE);
 `

### ActiveRenderTargetToPPM {#activerendertargettoppm}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `  CheckRenderD3D11::ActiveRenderTargetToPPM(g_pd3dDevice, cur_i`

### AddRef {#addref}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `CudaCapableAdapter->AddRef();
            cuda`

### AntialiasedLineEnable {#antialiasedlineenable}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.AntialiasedLineEnable = false;
    g_pd3d`

### AssertOrQuit {#assertorquit}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `#define AssertOrQuit(x)                                                                           \
`


## B

### BindFlags {#bindflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `ght;
    bufferDesc.BindFlags      = D3D11_BIND_V`

### BufferCount {#buffercount}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `sizeof(sd));
    sd.BufferCount                    `

### BufferDesc {#bufferdesc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `        = 1;
    sd.BufferDesc.Width              `

### BufferUsage {#bufferusage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `minator = 1;
    sd.BufferUsage                    `

### ByteWidth {#bytewidth}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `ULT;
    bufferDesc.ByteWidth      = sizeof(Verte`


## C

### CheckRenderD3D11 {#checkrenderd3d11}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `                    CheckRenderD3D11::ActiveRenderTarget`

### Cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `void Cleanup()
{`

### ClearColor {#clearcolor}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `ackbuffer
    float ClearColor[4] = {0.5f, 0.5f, 0`

### ClearRenderTargetView {#clearrendertargetview}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `_pd3dDeviceContext->ClearRenderTargetView(g_pSwapChainRTV, Cl`

### CreateBuffer {#createbuffer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` hr = g_pd3dDevice->CreateBuffer(&bufferDesc, NULL, `

### CreateInputLayout {#createinputlayout}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` hr = g_pd3dDevice->CreateInputLayout(inputElementDescs, `

### CreatePixelShader {#createpixelshader}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` hr = g_pd3dDevice->CreatePixelShader(PS->GetBufferPointe`

### CreateRasterizerState {#createrasterizerstate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `;
    g_pd3dDevice->CreateRasterizerState(&rasterizerState, &`

### CreateRenderTargetView {#createrendertargetview}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` hr = g_pd3dDevice->CreateRenderTargetView(pBuffer, NULL, &g_p`

### CreateVertexShader {#createvertexshader}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` hr = g_pd3dDevice->CreateVertexShader(VS->GetBufferPointe`

### CreateWindow {#createwindow}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `
    HWND hWnd    = CreateWindow(wc.lpszClassName,
 `

### CullMode {#cullmode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.CullMode              = D3D1`


## D

### DefWindowProc {#defwindowproc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `;
    }

    return DefWindowProc(hWnd, msg, wParam, `

### DepthBias {#depthbias}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.DepthBias             = false`

### DepthBiasClamp {#depthbiasclamp}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.DepthBiasClamp        = 0;
    ras`

### DepthClipEnable {#depthclipenable}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.DepthClipEnable       = false;
    `

### DeviceContext {#devicecontext}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `/ Get the immediate DeviceContext
    g_pd3dDevice->G`

### DispatchMessage {#dispatchmessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `g);
                DispatchMessage(&msg);
            `

### DrawScene {#drawscene}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `bool DrawScene(uint64_t &key)
{`


## E

### EnumAdapters1 {#enumadapters1}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `     hr = pFactory->EnumAdapters1(adapter, &pAdapter)`


## F

### FeatureLevels {#featurelevels}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `                 // FeatureLevels
                   `

### FillMode {#fillmode}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.FillMode              = D3D1`

### FrontCounterClockwise {#frontcounterclockwise}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.FrontCounterClockwise = false;
    raster`


## G

### GetBuffer {#getbuffer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` hr = g_pSwapChain->GetBuffer(0, __uuidof(ID3D11T`

### GetBufferPointer {#getbufferpointer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` char *)pErrorMsgs->GetBufferPointer();
            prin`

### GetBufferSize {#getbuffersize}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `ufferPointer(), VS->GetBufferSize(), NULL, &g_pVertex`

### GetDesc {#getdesc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `CudaCapableAdapter->GetDesc(&adapterDesc);
    `

### GetImmediateContext {#getimmediatecontext}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `t
    g_pd3dDevice->GetImmediateContext(&g_pd3dDeviceContex`

### GetModuleHandle {#getmodulehandle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `                    GetModuleHandle(NULL),
            `

### GetSharedHandle {#getsharedhandle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    hr = pResource->GetSharedHandle(&sharedHandle);
   `

### GetSystemMetrics {#getsystemmetrics}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `   int  xBorder = ::GetSystemMetrics(SM_CXSIZEFRAME);
  `


## I

### InitD3D {#initd3d}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `HRESULT InitD3D(HWND hWnd)
{`

### InterOP {#interop}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `        "CUDA/D3D11 InterOP",
                 `


## M

### MAX_EPSILON {#maxepsilon}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `#define MAX_EPSILON 10

static char *SDK_name = "simpleD3D11";

//----------------------------------`

### MaxDepth {#maxdepth}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `epth = 0.0f;
    vp.MaxDepth = 1.0f;
    vp.TopL`

### MinDepth {#mindepth}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `indowHeight;
    vp.MinDepth = 0.0f;
    vp.MaxD`

### MiscFlags {#miscflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `= 0;
    bufferDesc.MiscFlags      = D3D11_RESOUR`

### MsgProc {#msgproc}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `WINAPI MsgProc(HWND hWnd, UINT msg, WPARAM wParam, LPARAM lParam)
{`

### MultisampleEnable {#multisampleenable}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.MultisampleEnable     = false;
    ra`


## N

### NAME_LEN {#namelen}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `#define NAME_LEN 512

bool findCUDADevice()
{
    int deviceCount = 0;
    // This function call ret`


## O

### OutputWindow {#outputwindow}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `RGET_OUTPUT;
    sd.OutputWindow                    `


## P

### PSInput {#psinput}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `struct PSInput`

### PeekMessage {#peekmessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `) {
            if (PeekMessage(&msg, NULL, 0U, 0U,`

### PostQuitMessage {#postquitmessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `                    PostQuitMessage(0);
               `


## Q

### QueryInterface {#queryinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `r = g_VertexBuffer->QueryInterface(__uuidof(IDXGIKeyed`


## R

### RefreshRate {#refreshrate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `;
    sd.BufferDesc.RefreshRate.Numerator   = 60;
 `

### RegisterClassEx {#registerclassex}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `         NULL};
    RegisterClassEx(&wc);

    // Creat`

### ReleaseSync {#releasesync}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: ` = g_pKeyedMutex11->ReleaseSync(key);
    AssertOrQ`

### Render {#render}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `void Render()
{`

### RunSineWaveKernel {#runsinewavekernel}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `n vertex buffer
    RunSineWaveKernel(extSemaphore, key, `


## S

### SampleDesc {#sampledesc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `     = hWnd;
    sd.SampleDesc.Count              `

### ScissorEnable {#scissorenable}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.ScissorEnable         = false;
  `

### ShaderStructs {#shaderstructs}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `d3d11.h>

#include "ShaderStructs.h"
#include "sinewa`

### ShowWindow {#showwindow}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `        NULL);

    ShowWindow(hWnd, SW_SHOWDEFAUL`

### SlopeScaledDepthBias {#slopescaleddepthbias}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `    rasterizerState.SlopeScaledDepthBias  = 0;
    rasterize`


## T

### TopLeftX {#topleftx}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `epth = 1.0f;
    vp.TopLeftX = 0;
    vp.TopLeft`

### TopLeftY {#toplefty}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `opLeftX = 0;
    vp.TopLeftY = 0;
    g_pd3dDevi`

### TranslateMessage {#translatemessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `) {
                TranslateMessage(&msg);
            `


## U

### UnregisterClass {#unregisterclass}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `class
    UnregisterClass`

### UpdateWindow {#updatewindow}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `W_SHOWDEFAULT);
    UpdateWindow(hWnd);

    // Init`


## V

### ValidateRect {#validaterect}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `e WM_PAINT:
        ValidateRect(hWnd, NULL);
      `


## W

### WNDCLASSEX {#wndclassex}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `class
    WNDCLASSEX`


## Z

### ZeroMemory {#zeromemory}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `   MSG msg;
        ZeroMemory(&msg, sizeof(msg));`


## F

### findCUDADevice {#findcudadevice}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `bool findCUDADevice()
{`

### findDXDevice {#finddxdevice}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `bool findDXDevice(char *dev_name)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleD3D11/simpleD3D11.cpp](./simpleD3D11.cpp_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`

