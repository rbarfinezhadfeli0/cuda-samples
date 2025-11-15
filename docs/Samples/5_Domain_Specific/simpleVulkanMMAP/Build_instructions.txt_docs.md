# Documentation: Samples/5_Domain_Specific/simpleVulkanMMAP/Build_instructions.txt
---
## File Metadata
- **Path**: `Samples/5_Domain_Specific/simpleVulkanMMAP/Build_instructions.txt`
- **Filename**: `Build_instructions.txt`
- **Language**: text
- **Size**: 1969 bytes
- **Lines**: 36
- **Generated**: 2025-11-15 12:53:52 UTC

---
## Original Source
```text
For Windows:
Follow these steps once you have installed Vulkan SDK for Windows from https://www.lunarg.com/vulkan-sdk/
-- Install GLFW3 library at suitable location
-- Open the simpleVulkan VS project file.
To add the GLFW3 library path
-- Right click on Project name "simpleVulkan" click on "Properties"
-- In Property pages window go to Linker -> General. Here in "Additional Libraries Directories" edit and add path to glfw3dll.lib
To add the GLFW3 headers path
-- Right click on Project name "simpleVulkan" click on "Properties"
-- In Property pages window go to "VC++ Directories" section. Here in "Include Directories" edit and add path to GLFW3 headers include directory location.
** Make sure to add path to glfw3.dll in your PATH environment variable**


For Linux:
-- Install the Vulkan SDK from https://www.lunarg.com/vulkan-sdk/  and follow environment setup instructions.
-- Install GLFW3 library through your OS package repository. For example: apt-get for Ubuntu and dnf for RHEL/CentOS. Below is for Ubuntu:
    sudo apt-get install libglfw3
    sudo apt-get install libglfw3-dev
-- Install "libxcb1-dev" and "xorg-dev" as GLFW3 is depended on it
-- Add Vulkan and GLFW3 libraries directories to LD_LIBRARY_PATH


For Linux aarch64(L4T):
-- Install GLFW3 library using "sudo apt-get install libglfw3-dev" this will provide glfw3
-- install above will also provide libvulkan-dev as dependencies
-- Add Vulkan and GLFW3 libraries directories to LD_LIBRARY_PATH
-- Pass path to vulkan sdk while building 'make VULKAN_SDK_PATH=<PATH_TO_VULKAN_SDK>', VULKAN_SDK_PATH in this scenario is typically "/usr"


For Shader changes:
-- Update the montecarlo.vert and/or montecarlo.frag shader source file as required
-- Use the glslc shader compiler from the installed Vulkan SDK's bin directory to compile shaders as:
    glslc montecarlo.vert -o vert.spv
    glslc montecarlo.frag -o frag.spv
** Make sure to add glslc's path in your PATH environment variable **

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

