# Documentation: Samples/4_CUDA_Libraries/oceanFFT/data/ocean.frag
---
## File Metadata
- **Path**: `Samples/4_CUDA_Libraries/oceanFFT/data/ocean.frag`
- **Filename**: `ocean.frag`
- **Language**: text
- **Size**: 960 bytes
- **Lines**: 30
- **Generated**: 2025-11-15 12:53:51 UTC

---
## Original Source
```text
// GLSL fragment shader
varying vec3 eyeSpacePos;
varying vec3 worldSpaceNormal;
varying vec3 eyeSpaceNormal;

uniform vec4 deepColor;
uniform vec4 shallowColor;
uniform vec4 skyColor;
uniform vec3 lightDir;

void main()
{
    vec3 eyeVector              = normalize(eyeSpacePos);
    vec3 eyeSpaceNormalVector   = normalize(eyeSpaceNormal);
    vec3 worldSpaceNormalVector = normalize(worldSpaceNormal);

    float facing    = max(0.0, dot(eyeSpaceNormalVector, -eyeVector));
    float fresnel   = pow(1.0 - facing, 5.0); // Fresnel approximation
    float diffuse   = max(0.0, dot(worldSpaceNormalVector, lightDir));
    
//    vec4 waterColor = mix(shallowColor, deepColor, facing);
    vec4 waterColor = deepColor;
    
//    gl_FragColor = gl_Color;
//    gl_FragColor = vec4(fresnel);
//    gl_FragColor = vec4(diffuse);
//    gl_FragColor = waterColor;
//    gl_FragColor = waterColor*diffuse;
    gl_FragColor = waterColor*diffuse + skyColor*fresnel;
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

