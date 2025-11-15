# Documentation for Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.h

## File Metadata

- **Path**: `Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.h`
- **Type**: .h
- **Location**: Samples/5_Domain_Specific/simpleVulkan
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```h
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

#pragma once
#ifndef __VULKANBASEAPP_H__
#define __VULKANBASEAPP_H__

#include <string>
#include <vector>
#include <vulkan/vulkan.h>
#ifdef _WIN64
#define NOMINMAX
// Add windows.h to the include path
#include <windows.h>
// Add vulkan_win32.h to the include path
#include <vulkan/vulkan_win32.h>
#endif /* _WIN64 */

/* remove _VK_TIMELINE_SEMAPHORE to use binary semaphores */
// use vulkan timeline semaphore
#define _VK_TIMELINE_SEMAPHORE

struct GLFWwindow;

class VulkanBaseApp
{
public:
    VulkanBaseApp(const std::string &appName, bool enableValidation = false);
    static VkExternalSemaphoreHandleTypeFlagBits getDefaultSemaphoreHandleType();
    static VkExternalMemoryHandleTypeFlagBits    getDefaultMemHandleType();
    virtual ~VulkanBaseApp();
    void            init();
    void           *getMemHandle(VkDeviceMemory memory, VkExternalMemoryHandleTypeFlagBits handleType);
    void           *getSemaphoreHandle(VkSemaphore semaphore, VkExternalSemaphoreHandleTypeFlagBits handleType);
    void            createExternalSemaphore(VkSemaphore &semaphore, VkExternalSemaphoreHandleTypeFlagBits handleType);
    void            createBuffer(VkDeviceSize          size,
                                 VkBufferUsageFlags    usage,
                                 VkMemoryPropertyFlags properties,
                                 VkBuffer             &buffer,
                                 VkDeviceMemory       &bufferMemory);
    void            createExternalBuffer(VkDeviceSize                       size,
                                         VkBufferUsageFlags                 usage,
                                         VkMemoryPropertyFlags              properties,
                                         VkExternalMemoryHandleTypeFlagsKHR extMemHandleType,
                                         VkBuffer                          &buffer,
                                         VkDeviceMemory                    &bufferMemory);
    void            importExternalBuffer(void                              *handle,
                                         VkExternalMemoryHandleTypeFlagBits handleType,
                                         size_t                             size,
                                         VkBufferUsageFlags                 usage,
                                         VkMemoryPropertyFlags              properties,
                                         VkBuffer                          &buffer,
                                         VkDeviceMemory                    &memory);
    void            copyBuffer(VkBuffer dst, VkBuffer src, VkDeviceSize size);
    VkCommandBuffer beginSingleTimeCommands();
    void            endSingleTimeCommands(VkCommandBuffer commandBuffer);
    void            mainLoop();

protected:
    const std::string                                          m_appName;
    const bool                                                 m_enableValidation;
    VkInstance                                                 m_instance;
    VkDebugUtilsMessengerEXT                                   m_debugMessenger;
    VkSurfaceKHR                                               m_surface;
    VkPhysicalDevice                                           m_physicalDevice;
    VkDevice                                                   m_device;
    VkQueue                                                    m_graphicsQueue;
    VkQueue                                                    m_presentQueue;
    VkSwapchainKHR                                             m_swapChain;
    std::vector<VkImage>                                       m_swapChainImages;
    VkFormat                                                   m_swapChainFormat;
    VkExtent2D                                                 m_swapChainExtent;
    std::vector<VkImageView>                                   m_swapChainImageViews;
    std::vector<std::pair<VkShaderStageFlagBits, std::string>> m_shaderFiles;
    VkRenderPass                                               m_renderPass;
    VkPipelineLayout                                           m_pipelineLayout;
    VkPipeline                                                 m_graphicsPipeline;
    std::vector<VkFramebuffer>                                 m_swapChainFramebuffers;
    VkCommandPool                                              m_commandPool;
    std::vector<VkCommandBuffer>                               m_commandBuffers;
    std::vector<VkSemaphore>                                   m_imageAvailableSemaphores;
    std::vector<VkSemaphore>                                   m_renderFinishedSemaphores;
    std::vector<VkFence>                                       m_inFlightFences;
    std::vector<VkBuffer>                                      m_uniformBuffers;
    std::vector<VkDeviceMemory>                                m_uniformMemory;
    VkSemaphore                                                m_vkPresentationSemaphore;
    VkSemaphore                                                m_vkTimelineSemaphore;
    VkDescriptorSetLayout                                      m_descriptorSetLayout;
    VkDescriptorPool                                           m_descriptorPool;
    std::vector<VkDescriptorSet>                               m_descriptorSets;
    VkImage                                                    m_depthImage;
    VkDeviceMemory                                             m_depthImageMemory;
    VkImageView                                                m_depthImageView;
    size_t                                                     m_currentFrame;
    bool                                                       m_framebufferResized;
    uint8_t                                                    m_vkDeviceUUID[VK_UUID_SIZE];

    virtual void                      initVulkanApp() {}
    virtual void                      fillRenderingCommandBuffer(VkCommandBuffer &buffer) {}
    virtual std::vector<const char *> getRequiredExtensions() const;
    virtual std::vector<const char *> getRequiredDeviceExtensions() const;
    virtual void                      getVertexDescriptions(std::vector<VkVertexInputBindingDescription>   &bindingDesc,
                                                            std::vector<VkVertexInputAttributeDescription> &attribDesc);
    virtual void                      getAssemblyStateInfo(VkPipelineInputAssemblyStateCreateInfo &info);
    virtual void                      getWaitFrameSemaphores(std::vector<VkSemaphore>          &wait,
                                                             std::vector<VkPipelineStageFlags> &waitStages) const;
    virtual void                      getSignalFrameSemaphores(std::vector<VkSemaphore> &signal) const;
    virtual VkDeviceSize              getUniformSize() const;
    virtual void                      updateUniformBuffer(uint32_t imageIndex);
    virtual void                      drawFrame();

private:
    GLFWwindow *m_window;

    void initWindow();
    void initVulkan();
    void createInstance();
    void createSurface();
    void createDevice();
    void createSwapChain();
    void createImageViews();
    void createRenderPass();
    void createDescriptorSetLayout();
    void createGraphicsPipeline();
    void createFramebuffers();
    void createCommandPool();
    void createDepthResources();
    void createUniformBuffers();
    void createDescriptorPool();
    void createDescriptorSets();
    void createCommandBuffers();
    void createSyncObjects();

    void cleanupSwapChain();
    void recreateSwapChain();

    bool        isSuitableDevice(VkPhysicalDevice dev) const;
    static void resizeCallback(GLFWwindow *window, int width, int height);
};

void readFile(std::istream &s, std::vector<char> &data);

#endif /* __VULKANBASEAPP_H__ */

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.h`.

### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

## Detailed Analysis

### File Statistics

- **Total Lines**: 168
- **Approximate Size**: 9416 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
