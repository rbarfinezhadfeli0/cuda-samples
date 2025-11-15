# Keywords: Common/paramgl.h
---

**Total Keywords**: 28

---

## C

### Color {#color}

- **Type**: type
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `struct Color`


## G

### GetCurrent {#getcurrent}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `_KEY_RIGHT:
        GetCurrent()->Increment();
   `

### GetName {#getname}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `y + m_font_h, (*p)->GetName().c_str(),
        `

### GetPercentage {#getpercentage}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `_bar_w - 1) * (*p)->GetPercentage())),
            (G`

### GetSize {#getsize}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `_separation * list->GetSize();
      } else {
 `

### GetValueString {#getvaluestring}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `              (*p)->GetValueString().c_str(), m_font,
`


## I

### IsList {#islist}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `) {
      if ((*p)->IsList()) {
        ParamL`


## M

### Motion {#motion}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `bool Motion(int x, int y) {`

### Mouse {#mouse}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `bool Mouse(int x, int y, int button = GLUT_LEFT_BUTTON,
             int state = GLUT_DOWN) {`


## O

### OpenGL {#opengl}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `amList to do simple OpenGL rendering of a para`


## P

### PARAMGL_H {#paramglh}

- **Type**: macro
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `#define PARAMGL_H

#if defined(__APPLE__) || defined(MACOSX)
#include <GLUT/glut.h>
#else
#include <`

### ParamBase {#parambase}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `   for (std::vector<ParamBase *>::const_iterator `

### ParamList {#paramlist}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: ` class derived from ParamList to do simple OpenGL`

### ParamListGL {#paramlistgl}

- **Type**: type
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `class ParamListGL`


## R

### Render {#render}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void Render(int x, int y, bool shadow = false) {`


## S

### SetActive {#setactive}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void SetActive(bool b) {`

### SetBarColorInner {#setbarcolorinner}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void SetBarColorInner(float r, float g, float b) {`

### SetBarColorOuter {#setbarcolorouter}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void SetBarColorOuter(float r, float g, float b) {`

### SetFont {#setfont}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void SetFont(void *font, int height) {`

### SetPercentage {#setpercentage}

- **Type**: identifier
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `      (*m_current)->SetPercentage(0.0);
      return `

### SetSelectedColor {#setselectedcolor}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void SetSelectedColor(float r, float g, float b) {`

### SetUnSelectedColor {#setunselectedcolor}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void SetUnSelectedColor(float r, float g, float b) {`

### Special {#special}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void Special(int key, int x, int y) {`


## B

### beginWinCoords {#beginwincoords}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void beginWinCoords(void) {`


## D

### derived {#derived}

- **Type**: type
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `class derived`


## E

### endWinCoords {#endwincoords}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void endWinCoords(void) {`


## G

### glPrint {#glprint}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void glPrint(int x, int y, const char *s, void *font) {`

### glPrintShadowed {#glprintshadowed}

- **Type**: function
- **File**: [Common/paramgl.h](./paramgl.h_docs.md)
- **Context**: `void glPrintShadowed(int x, int y, const char *s, void *font,
                            float *col`

