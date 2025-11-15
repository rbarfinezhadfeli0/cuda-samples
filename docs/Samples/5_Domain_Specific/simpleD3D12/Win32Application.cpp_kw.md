# Keywords: Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp
---

**Total Keywords**: 26

---

## A

### AdjustWindowRect {#adjustwindowrect}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `>GetHeight())};
    AdjustWindowRect(&windowRect, WS_OVE`


## C

### CommandLineToArgvW {#commandlinetoargvw}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `
    LPWSTR *argv = CommandLineToArgvW(GetCommandLineW(), `

### CreateWindow {#createwindow}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `to it.
    m_hwnd = CreateWindow(windowClass.lpszCla`


## D

### DefWindowProc {#defwindowproc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: ` didn't.
    return DefWindowProc(hWnd, message, wPar`

### DispatchMessage {#dispatchmessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `(&msg);
            DispatchMessage(&msg);
        }
  `


## G

### GetCommandLineW {#getcommandlinew}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: ` CommandLineToArgvW(GetCommandLineW(), &argc);
    pSam`

### GetHeight {#getheight}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `cast<LONG>(pSample->GetHeight())};
    AdjustWind`

### GetTitle {#gettitle}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `           pSample->GetTitle(),
                `

### GetWidth {#getwidth}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `cast<LONG>(pSample->GetWidth()), static_cast<LON`

### GetWindowLongPtr {#getwindowlongptr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `t<DX12CudaSample *>(GetWindowLongPtr(hWnd, GWLP_USERDATA`


## H

### HelloTexture {#hellotexture}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `D3D12HelloWorld/src/HelloTexture,
  which is license`


## L

### LoadCursor {#loadcursor}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `ass.hCursor       = LoadCursor(NULL, IDC_ARROW);
 `

### LocalFree {#localfree}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `gs(argv, argc);
    LocalFree(argv);

    // Init`


## O

### OnDestroy {#ondestroy}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `    }

    pSample->OnDestroy();

    // Return t`

### OnInit {#oninit}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `tialize the sample. OnInit is defined in each `

### OnKeyDown {#onkeydown}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `           pSample->OnKeyDown(static_cast<UINT8>(`

### OnKeyUp {#onkeyup}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `           pSample->OnKeyUp(static_cast<UINT8>(`

### OnRender {#onrender}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `           pSample->OnRender();
        }
      `


## P

### ParseCommandLineArgs {#parsecommandlineargs}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `argc);
    pSample->ParseCommandLineArgs(argv, argc);
    Lo`

### PeekMessage {#peekmessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: ` queue.
        if (PeekMessage(&msg, NULL, 0, 0, P`

### PostQuitMessage {#postquitmessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `WM_DESTROY:
        PostQuitMessage(0);
        return `


## R

### RegisterClassEx {#registerclassex}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `daSampleClass";
    RegisterClassEx(&windowClass);

   `


## S

### SetWindowLongPtr {#setwindowlongptr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `T>(lParam);
        SetWindowLongPtr(hWnd, GWLP_USERDATA`

### ShowWindow {#showwindow}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `ple->OnInit();

    ShowWindow(m_hwnd, nCmdShow);
`


## T

### TranslateMessage {#translatemessage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `OVE)) {
            TranslateMessage(&msg);
            `


## W

### WindowProc {#windowproc}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleD3D12/Win32Application.cpp](./Win32Application.cpp_docs.md)
- **Context**: `ass.lpfnWndProc   = WindowProc;
    windowClass.hI`

