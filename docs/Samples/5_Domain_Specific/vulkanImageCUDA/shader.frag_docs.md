# Documentation: Samples/5_Domain_Specific/vulkanImageCUDA/shader.frag
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/vulkanImageCUDA/shader.frag`
- **Filename**: `shader.frag`
- **Language**: text
- **Size**: 343 bytes
- **Lines**: 13
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```text
#version 450
#extension GL_ARB_separate_shader_objects : enable
#extension GL_NV_gpu_shader5 : enable

layout(location = 0) in vec3 fragColor;
layout(location = 1) in vec2 fragTexCoord;
layout(binding = 1) uniform sampler2D texSampler;

layout(location = 0) out vec4 outColor;

void main() {
    outColor = texture(texSampler, fragTexCoord);
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

