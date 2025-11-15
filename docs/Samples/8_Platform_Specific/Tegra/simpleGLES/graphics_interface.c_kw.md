# Keywords: Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c
---

**Total Keywords**: 22

---

## B

### BlackPixel {#blackpixel}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `                    BlackPixel(display, screen),
 `

### ButtonPressMask {#buttonpressmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `     ExposureMask | ButtonPressMask | KeyPressMask | St`

### ButtonReleaseMask {#buttonreleasemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `ructureNotifyMask | ButtonReleaseMask
                   `


## C

### ColormapChangeMask {#colormapchangemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `ibilityChangeMask | ColormapChangeMask);

    XMapWindow(d`


## D

### DefaultScreen {#defaultscreen}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `\n");

    screen = DefaultScreen(display);

    eglD`


## E

### EnterWindowMask {#enterwindowmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: ` | KeyReleaseMask | EnterWindowMask | LeaveWindowMask |`

### ExposureMask {#exposuremask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `n,
                 ExposureMask | ButtonPressMask |`


## G

### GET_GLERROR {#getglerror}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `#define GET_GLERROR(ret)                                                                   \
    {  `


## K

### KeyPressMask {#keypressmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `| ButtonPressMask | KeyPressMask | StructureNotifyMa`

### KeyReleaseMask {#keyreleasemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `                  | KeyReleaseMask | EnterWindowMask |`


## L

### LeaveWindowMask {#leavewindowmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `| EnterWindowMask | LeaveWindowMask | PointerMotionMask`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `derr, "[%s line %d] OpenGL Error: 0x%x\n", __F`

### OpenVG {#openvg}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `text Query Returned OpenVG. This is Unsupporte`


## P

### PointerMotionMask {#pointermotionmask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `| LeaveWindowMask | PointerMotionMask | Button1MotionMask`


## R

### RootWindow {#rootwindow}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `                    RootWindow(display, screen),
 `


## S

### StructureNotifyMask {#structurenotifymask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `sk | KeyPressMask | StructureNotifyMask | ButtonReleaseMask`


## V

### VisibilityChangeMask {#visibilitychangemask}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `Button2MotionMask | VisibilityChangeMask | ColormapChangeMas`


## W

### WhitePixel {#whitepixel}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `                    WhitePixel(display, screen));
`


## G

### graphics_close_window {#graphicsclosewindow}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `void graphics_close_window()
{`

### graphics_set_windowtitle {#graphicssetwindowtitle}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `void graphics_set_windowtitle(const char *windowname) {`

### graphics_setup_window {#graphicssetupwindow}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `int graphics_setup_window(int xpos, int ypos, int width, int height, const char *windowname)
{`

### graphics_swap_buffers {#graphicsswapbuffers}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/simpleGLES/graphics_interface.c](./graphics_interface.c_docs.md)
- **Context**: `void graphics_swap_buffers() {`

