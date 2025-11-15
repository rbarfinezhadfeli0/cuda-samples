# Documentation: Samples/4_CUDA_Libraries/oceanFFT/data/ocean.vert
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/oceanFFT/data/ocean.vert`
- **Filename**: `ocean.vert`
- **Language**: text
- **Size**: 881 bytes
- **Lines**: 25
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```text
// GLSL vertex shader
varying vec3 eyeSpacePos;
varying vec3 worldSpaceNormal;
varying vec3 eyeSpaceNormal;
uniform float heightScale; // = 0.5;
uniform float chopiness;   // = 1.0;
uniform vec2  size;        // = vec2(256.0, 256.0);

void main()
{
    float height     = gl_MultiTexCoord0.x;
    vec2  slope      = gl_MultiTexCoord1.xy;

    // calculate surface normal from slope for shading
	vec3 normal      = normalize(cross( vec3(0.0, slope.y*heightScale, 2.0 / size.x), vec3(2.0 / size.y, slope.x*heightScale, 0.0)));
    worldSpaceNormal = normal;

    // calculate position and transform to homogeneous clip space
    vec4 pos         = vec4(gl_Vertex.x, height * heightScale, gl_Vertex.z, 1.0);
    gl_Position      = gl_ModelViewProjectionMatrix * pos;
    
    eyeSpacePos      = (gl_ModelViewMatrix * pos).xyz;
    eyeSpaceNormal   = (gl_NormalMatrix * normal).xyz;
}

```

---
## High-Level Overview
This file is a text source file in the CUDA Samples repository.


---
## Detailed Walkthrough

---
## Usage Examples
Refer to the repository documentation for usage instructions.


---
## Performance & Security Notes
### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

