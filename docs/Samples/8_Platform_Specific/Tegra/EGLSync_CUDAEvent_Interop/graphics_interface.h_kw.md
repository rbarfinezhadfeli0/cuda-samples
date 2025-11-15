# Keywords: Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h
---

**Total Keywords**: 21

---

## B

### ButtonPressMask {#buttonpressmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `sk | KeyPressMask | ButtonPressMask | ButtonReleaseMask`

### ButtonReleaseMask {#buttonreleasemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `| ButtonPressMask | ButtonReleaseMask | KeyReleaseMask
  `


## C

### ClientMessage {#clientmessage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `e                 = ClientMessage;
    xEvent.xclient`

### CopyFromParent {#copyfromparent}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `                    CopyFromParent,
                  `


## D

### DefaultRootWindow {#defaultrootwindow}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `      xRootWindow = DefaultRootWindow(display);
    XSetW`

### DefaultScreen {#defaultscreen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `\n");

    screen = DefaultScreen(display);

    eglD`


## E

### ExposureMask {#exposuremask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `ibutes.event_mask = ExposureMask;
    win           `


## G

### GET_GLERROR {#getglerror}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `#define GET_GLERROR(ret)                                                                   \
    {  `


## I

### InputOutput {#inputoutput}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `                    InputOutput,
                  `


## K

### KeyPressMask {#keypressmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `     ExposureMask | KeyPressMask | ButtonPressMask |`

### KeyReleaseMask {#keyreleasemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `ButtonReleaseMask | KeyReleaseMask
                   `


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `derr, "[%s line %d] OpenGL Error: 0x%x\n", __F`

### OpenVG {#openvg}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `text Query Returned OpenVG. This is Unsupporte`


## P

### PointerMotionMask {#pointermotionmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `ibilityChangeMask | PointerMotionMask);

    EGLint windo`


## S

### SubstructureNotifyMask {#substructurenotifymask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `xRootWindow, false, SubstructureNotifyMask, &xEvent);

    XSt`


## V

### VisibilityChangeMask {#visibilitychangemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `                  | VisibilityChangeMask | PointerMotionMask`


## E

### error_exit {#errorexit}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `void error_exit(const char *format, ...)
{`


## G

### graphics_close_window {#graphicsclosewindow}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `void graphics_close_window()
{`

### graphics_set_windowtitle {#graphicssetwindowtitle}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `void graphics_set_windowtitle(const char *windowname) {`

### graphics_setup_window {#graphicssetupwindow}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `int graphics_setup_window(int xpos, int ypos, int width, int height, const char *windowname)
{`

### graphics_swap_buffers {#graphicsswapbuffers}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/graphics_interface.h](./graphics_interface.h_docs.md)
- **Context**: `void graphics_swap_buffers() {`

