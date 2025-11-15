# Keywords: Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp
---

**Total Keywords**: 139

---

## A

### AllocateAndInitializeSid {#allocateandinitializesid}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `_SID_AUTHORITY;
    AllocateAndInitializeSid(&sidIdentifierAutho`


## F

### FreeSid {#freesid}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: ` (*ppSID) {
        FreeSid(*ppSID);
    }
    `


## G

### GLFW_INCLUDE_VULKAN {#glfwincludevulkan}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `#define GLFW_INCLUDE_VULKAN
#define GLM_FORCE_DEPTH_ZERO_TO_ONE
#include <GLFW/glfw3.h>

#ifdef _WIN`

### GLM_FORCE_DEPTH_ZERO_TO_ONE {#glmforcedepthzerotoone}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `#define GLM_FORCE_DEPTH_ZERO_TO_ONE
#include <GLFW/glfw3.h>

#ifdef _WIN64
#include <VersionHelpers.`


## I

### InitializeSecurityDescriptor {#initializesecuritydescriptor}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `zeof(PSID *));

    InitializeSecurityDescriptor(m_winPSecurityDescr`

### IsWindows8OrGreater {#iswindows8orgreater}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `f _WIN64
    return IsWindows8OrGreater() ? VK_EXTERNAL_SEM`

### IsWindows8Point1OrGreater {#iswindows8point1orgreater}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `f _WIN64
    return IsWindows8Point1OrGreater() ? VK_EXTERNAL_MEM`


## L

### LocalFree {#localfree}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: ` (*ppACL) {
        LocalFree(*ppACL);
    }
    `


## S

### SetEntriesInAcl {#setentriesinacl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `LPTSTR)*ppSID;

    SetEntriesInAcl(1, &explicitAccess,`

### SetSecurityDescriptorDacl {#setsecuritydescriptordacl}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: ` NULL, ppACL);

    SetSecurityDescriptorDacl(m_winPSecurityDescr`


## T

### TrusteeForm {#trusteeform}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `licitAccess.Trustee.TrusteeForm  = TRUSTEE_IS_SID;
`

### TrusteeType {#trusteetype}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `licitAccess.Trustee.TrusteeType  = TRUSTEE_IS_WELL_`


## V

### VersionHelpers {#versionhelpers}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ef _WIN64
#include <VersionHelpers.h>
#include <aclapi`

### VkApplicationInfo {#vkapplicationinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `rted!");
    }

    VkApplicationInfo appInfo  = {};
    `

### VkAttachmentDescription {#vkattachmentdescription}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `eRenderPass()
{
    VkAttachmentDescription colorAttachment = {`

### VkAttachmentReference {#vkattachmentreference}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ESENT_SRC_KHR;

    VkAttachmentReference colorAttachmentRef `

### VkBool32 {#vkbool32}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `

static VKAPI_ATTR VkBool32 VKAPI_CALL debugCal`

### VkBuffer {#vkbuffer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkBuffer             &buffer`

### VkBufferCopy {#vkbuffercopy}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `imeCommands();

    VkBufferCopy copyRegion = {};
  `

### VkBufferCreateInfo {#vkbuffercreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `bufferMemory)
{
    VkBufferCreateInfo bufferInfo = {};
  `

### VkBufferUsageFlags {#vkbufferusageflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkBufferUsageFlags    usage,
         `

### VkClearValue {#vkclearvalue}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ainExtent;

        VkClearValue clearColors[2];
   `

### VkCommandBuffer {#vkcommandbuffer}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `   initVulkan();
}

VkCommandBuffer VulkanBaseApp::begi`

### VkCommandBufferAllocateInfo {#vkcommandbufferallocateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `imeCommands()
{
    VkCommandBufferAllocateInfo allocInfo = {};
   `

### VkCommandBufferBeginInfo {#vkcommandbufferbegininfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ommandBuffer);

    VkCommandBufferBeginInfo beginInfo = {};
   `

### VkCommandPoolCreateInfo {#vkcommandpoolcreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `CommandPool()
{
    VkCommandPoolCreateInfo poolInfo = {};
    `

### VkDebugUtilsMessageSeverityFlagBitsEXT {#vkdebugutilsmessageseverityflagbitsext}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `_CALL debugCallback(VkDebugUtilsMessageSeverityFlagBitsEXT      messageSeverit`

### VkDebugUtilsMessageTypeFlagsEXT {#vkdebugutilsmessagetypeflagsext}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkDebugUtilsMessageTypeFlagsEXT             message`

### VkDebugUtilsMessengerCallbackDataEXT {#vkdebugutilsmessengercallbackdataext}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `              const VkDebugUtilsMessengerCallbackDataEXT *pCallbackData,
   `

### VkDebugUtilsMessengerCreateInfoEXT {#vkdebugutilsmessengercreateinfoext}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: ` = exts.data();
    VkDebugUtilsMessengerCreateInfoEXT debugCreateInfo = {`

### VkDescriptorBufferInfo {#vkdescriptorbufferinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `sets!");
    }

    VkDescriptorBufferInfo bufferInfo    = {};`

### VkDescriptorPoolCreateInfo {#vkdescriptorpoolcreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `Images.size());
    VkDescriptorPoolCreateInfo poolInfo = {};
    `

### VkDescriptorPoolSize {#vkdescriptorpoolsize}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `criptorPool()
{
    VkDescriptorPoolSize poolSize       = {}`

### VkDescriptorSetAllocateInfo {#vkdescriptorsetallocateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ptorSetLayout);
    VkDescriptorSetAllocateInfo        allocInfo = `

### VkDescriptorSetLayout {#vkdescriptorsetlayout}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `)
{
    std::vector<VkDescriptorSetLayout> layouts(m_swapChai`

### VkDescriptorSetLayoutBinding {#vkdescriptorsetlayoutbinding}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `orSetLayout()
{
    VkDescriptorSetLayoutBinding uboLayoutBinding = `

### VkDescriptorSetLayoutCreateInfo {#vkdescriptorsetlayoutcreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `GE_VERTEX_BIT;

    VkDescriptorSetLayoutCreateInfo layoutInfo = {};
  `

### VkDevice {#vkdevice}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `iew createImageView(VkDevice dev, VkImage image,`

### VkDeviceCreateInfo {#vkdevicecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `       = true;

    VkDeviceCreateInfo createInfo = {};
  `

### VkDeviceMemory {#vkdevicememory}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkDeviceMemory       &imageMemory)`

### VkDeviceQueueCreateInfo {#vkdevicequeuecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `);

    std::vector<VkDeviceQueueCreateInfo> queueCreateInfos;
`

### VkDeviceSize {#vkdevicesize}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `formBuffers()
{
    VkDeviceSize size = getUniformSi`

### VkExportMemoryAllocateInfoKHR {#vkexportmemoryallocateinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `if /* _WIN64 */
    VkExportMemoryAllocateInfoKHR vulkanExportMemoryA`

### VkExportMemoryWin32HandleInfoKHR {#vkexportmemorywin32handleinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ityAttributes;

    VkExportMemoryWin32HandleInfoKHR vulkanExportMemoryW`

### VkExportSemaphoreCreateInfoKHR {#vkexportsemaphorecreateinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `RE_CREATE_INFO;
    VkExportSemaphoreCreateInfoKHR exportSemaphoreCrea`

### VkExtensionProperties {#vkextensionproperties}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `r);
    std::vector<VkExtensionProperties> availableExtension`

### VkExtent2D {#vkextent2d}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `bestMode;
}

static VkExtent2D chooseSwapExtent(GL`

### VkExternalMemoryBufferCreateInfo {#vkexternalmemorybuffercreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ODE_EXCLUSIVE;

    VkExternalMemoryBufferCreateInfo externalMemoryBuffe`

### VkExternalMemoryHandleTypeFlagBits {#vkexternalmemoryhandletypeflagbits}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `dif /* _WIN64 */
}

VkExternalMemoryHandleTypeFlagBits VulkanBaseApp::getD`

### VkExternalMemoryHandleTypeFlagsKHR {#vkexternalmemoryhandletypeflagskhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkExternalMemoryHandleTypeFlagsKHR extMemHandleType,
 `

### VkExternalSemaphoreHandleTypeFlagBits {#vkexternalsemaphorehandletypeflagbits}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `Resized(false)
{
}

VkExternalSemaphoreHandleTypeFlagBits VulkanBaseApp::getD`

### VkFenceCreateInfo {#vkfencecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `RE_CREATE_INFO;
    VkFenceCreateInfo fenceInfo         =`

### VkFormat {#vkformat}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `* _WIN64 */

static VkFormat findSupportedFormat`

### VkFormatFeatureFlags {#vkformatfeatureflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkFormatFeatureFlags         features)
{`

### VkFormatProperties {#vkformatproperties}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ndidates) {
        VkFormatProperties props;
        vkGe`

### VkFramebufferCreateInfo {#vkframebuffercreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `mageView};

        VkFramebufferCreateInfo framebufferInfo = {`

### VkGraphicsPipelineCreateInfo {#vkgraphicspipelinecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `yout!");
    }

    VkGraphicsPipelineCreateInfo pipelineInfo = {};
`

### VkImage {#vkimage}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `eView(VkDevice dev, VkImage image, VkFormat for`

### VkImageAspectFlags {#vkimageaspectflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `e, VkFormat format, VkImageAspectFlags aspectFlags)
{
    `

### VkImageCreateInfo {#vkimagecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `&imageMemory)
{
    VkImageCreateInfo imageInfo = {};
   `

### VkImageLayout {#vkimagelayout}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkImageLayout  oldLayout,
       `

### VkImageMemoryBarrier {#vkimagememorybarrier}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `imeCommands();

    VkImageMemoryBarrier barrier = {};
    b`

### VkImageTiling {#vkimagetiling}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkImageTiling                tili`

### VkImageUsageFlags {#vkimageusageflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkImageUsageFlags     usage,
        `

### VkImageView {#vkimageview}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `= extent;
}

static VkImageView createImageView(VkD`

### VkImageViewCreateInfo {#vkimageviewcreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `     imageView;
    VkImageViewCreateInfo createInfo         `

### VkImportMemoryFdInfoKHR {#vkimportmemoryfdinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `  = NULL;
#else
    VkImportMemoryFdInfoKHR handleInfo = {};
  `

### VkImportMemoryWin32HandleInfoKHR {#vkimportmemorywin32handleinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `

#ifdef _WIN64
    VkImportMemoryWin32HandleInfoKHR handleInfo = {};
  `

### VkInstanceCreateInfo {#vkinstancecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `I_VERSION_1_2;

    VkInstanceCreateInfo createInfo = {};
  `

### VkLayerProperties {#vklayerproperties}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `)
{
    std::vector<VkLayerProperties> availableLayers;
 `

### VkMemoryAllocateInfo {#vkmemoryallocateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `Requirements);

    VkMemoryAllocateInfo allocInfo = {};
   `

### VkMemoryGetFdInfoKHR {#vkmemorygetfdinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `  int fd = -1;

    VkMemoryGetFdInfoKHR vkMemoryGetFdInfoKH`

### VkMemoryGetWin32HandleInfoKHR {#vkmemorygetwin32handleinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `LE handle = 0;

    VkMemoryGetWin32HandleInfoKHR vkMemoryGetWin32Han`

### VkMemoryPropertyFlags {#vkmemorypropertyflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `int32_t typeFilter, VkMemoryPropertyFlags properties)
{
    V`

### VkMemoryRequirements {#vkmemoryrequirements}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `mage!");
    }

    VkMemoryRequirements memRequirements;
  `

### VkPhysicalDevice {#vkphysicaldevice}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `findSupportedFormat(VkPhysicalDevice             physica`

### VkPhysicalDeviceFeatures {#vkphysicaldevicefeatures}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `teInfo);
    }

    VkPhysicalDeviceFeatures deviceFeatures = {}`

### VkPhysicalDeviceIDProperties {#vkphysicaldeviceidproperties}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `presentQueue);

    VkPhysicalDeviceIDProperties vkPhysicalDeviceIDP`

### VkPhysicalDeviceMemoryProperties {#vkphysicaldevicememoryproperties}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `s properties)
{
    VkPhysicalDeviceMemoryProperties memProperties;
    `

### VkPhysicalDeviceProperties2 {#vkphysicaldeviceproperties2}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `       = NULL;

    VkPhysicalDeviceProperties2 vkPhysicalDevicePro`

### VkPhysicalDeviceVulkan12Features {#vkphysicaldevicevulkan12features}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ELINE_SEMAPHORE
    VkPhysicalDeviceVulkan12Features vk12features = {};
`

### VkPipelineColorBlendAttachmentState {#vkpipelinecolorblendattachmentstate}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `   = VK_FALSE;

    VkPipelineColorBlendAttachmentState colorBlendAttachmen`

### VkPipelineColorBlendStateCreateInfo {#vkpipelinecolorblendstatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `le = VK_FALSE;

    VkPipelineColorBlendStateCreateInfo colorBlending = {};`

### VkPipelineDepthStencilStateCreateInfo {#vkpipelinedepthstencilstatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `E; // Optional

    VkPipelineDepthStencilStateCreateInfo depthStencil = {};
`

### VkPipelineInputAssemblyStateCreateInfo {#vkpipelineinputassemblystatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `etAssemblyStateInfo(VkPipelineInputAssemblyStateCreateInfo &info) {}

void Vul`

### VkPipelineLayoutCreateInfo {#vkpipelinelayoutcreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `       = 0.0f;

    VkPipelineLayoutCreateInfo pipelineLayoutInfo `

### VkPipelineMultisampleStateCreateInfo {#vkpipelinemultisamplestatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `   = VK_FALSE;

    VkPipelineMultisampleStateCreateInfo multisampling = {};`

### VkPipelineRasterizationStateCreateInfo {#vkpipelinerasterizationstatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `   = &scissor;

    VkPipelineRasterizationStateCreateInfo rasterizer = {};
  `

### VkPipelineShaderStageCreateInfo {#vkpipelineshaderstagecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `)
{
    std::vector<VkPipelineShaderStageCreateInfo> shaderStageInfos(m`

### VkPipelineStageFlags {#vkpipelinestageflags}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `Count     = 1;

    VkPipelineStageFlags sourceStage;
    Vk`

### VkPipelineVertexInputStateCreateInfo {#vkpipelinevertexinputstatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: ` "main";
    }

    VkPipelineVertexInputStateCreateInfo vertexInputInfo = {`

### VkPipelineViewportStateCreateInfo {#vkpipelineviewportstatecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `apChainExtent;

    VkPipelineViewportStateCreateInfo viewportState = {};`

### VkPresentInfoKHR {#vkpresentinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ffer!");
    }

    VkPresentInfoKHR presentInfo   = {};`

### VkPresentModeKHR {#vkpresentmodekhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `        std::vector<VkPresentModeKHR>   &presentModes)
{`

### VkQueueFamilyProperties {#vkqueuefamilyproperties}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `);

    std::vector<VkQueueFamilyProperties> queueFamilies(queu`

### VkRect2D {#vkrect2d}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `epth   = 1.0f;

    VkRect2D scissor = {};
    s`

### VkRenderPassBeginInfo {#vkrenderpassbegininfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `
        }

        VkRenderPassBeginInfo renderPassInfo = {}`

### VkRenderPassCreateInfo {#vkrenderpasscreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `pthAttachment};
    VkRenderPassCreateInfo  renderPassInfo = {`

### VkResult {#vkresult}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `2_t imageIndex;
    VkResult result = vkAcquireN`

### VkSemaphore {#vksemaphore}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `aphores(std::vector<VkSemaphore>          &wait,
  `

### VkSemaphoreCreateInfo {#vksemaphorecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `SyncObjects()
{
    VkSemaphoreCreateInfo semaphoreInfo = {};`

### VkSemaphoreGetFdInfoKHR {#vksemaphoregetfdinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `se
    int fd;

    VkSemaphoreGetFdInfoKHR semaphoreGetFdInfoK`

### VkSemaphoreGetWin32HandleInfoKHR {#vksemaphoregetwin32handleinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `HANDLE handle;

    VkSemaphoreGetWin32HandleInfoKHR semaphoreGetWin32Ha`

### VkSemaphoreTypeCreateInfo {#vksemaphoretypecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ELINE_SEMAPHORE
    VkSemaphoreTypeCreateInfo timelineCreateInfo;`

### VkSemaphoreWaitInfo {#vksemaphorewaitinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `gnalValue = 1;

    VkSemaphoreWaitInfo semaphoreWaitInfo =`

### VkShaderModule {#vkshadermodule}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `layout!");
    }
}

VkShaderModule createShaderModule(`

### VkShaderModuleCreateInfo {#vkshadermodulecreateinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `_base::binary);
    VkShaderModuleCreateInfo createInfo = {};
  `

### VkSubmitInfo {#vksubmitinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `ommandBuffer);

    VkSubmitInfo submitInfo       = `

### VkSubpassDependency {#vksubpassdependency}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `AttachmentRef;

    VkSubpassDependency dependency = {};
  `

### VkSubpassDescription {#vksubpassdescription}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `HMENT_OPTIMAL;

    VkSubpassDescription subpass    = {};
  `

### VkSurfaceCapabilitiesKHR {#vksurfacecapabilitieskhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkSurfaceCapabilitiesKHR        &capabilitie`

### VkSurfaceFormatKHR {#vksurfaceformatkhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `        std::vector<VkSurfaceFormatKHR> &formats,
        `

### VkSurfaceKHR {#vksurfacekhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `                    VkSurfaceKHR     surface,
      `

### VkSwapchainCreateInfoKHR {#vkswapchaincreateinfokhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `       }
    }

    VkSwapchainCreateInfoKHR createInfo = {};
  `

### VkSwapchainKHR {#vkswapchainkhr}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `tionSemaphore;

    VkSwapchainKHR swapChains[] = {m_s`

### VkTimelineSemaphoreSubmitInfo {#vktimelinesemaphoresubmitinfo}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `phores.data();

    VkTimelineSemaphoreSubmitInfo timelineInfo = {};
`

### VkVertexInputAttributeDescription {#vkvertexinputattributedescription}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `        std::vector<VkVertexInputAttributeDescription> &attribDesc)
{
}

`

### VkVertexInputBindingDescription {#vkvertexinputbindingdescription}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `iptions(std::vector<VkVertexInputBindingDescription>   &bindingDesc,
  `

### VkViewport {#vkviewport}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `nputAssembly);

    VkViewport viewport = {};
    `

### VkWriteDescriptorSet {#vkwritedescriptorset}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: ` VK_WHOLE_SIZE;
    VkWriteDescriptorSet descriptorWrite = {`

### VulkanBaseApp {#vulkanbaseapp}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `com/
 */

#include "VulkanBaseApp.h"

#include <algor`


## W

### WindowsSecurityAttributes {#windowssecurityattributes}

- **Type**: type
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `class WindowsSecurityAttributes`


## Z

### ZeroMemory {#zeromemory}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `explicitAccess;
    ZeroMemory(&explicitAccess, si`


## C

### chooseSwapExtent {#chooseswapextent}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VkExtent2D chooseSwapExtent(GLFWwindow *window, const VkSurfaceCapabilitiesKHR &capabilities)
{`

### chooseSwapPresentMode {#chooseswappresentmode}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VkPresentModeKHR chooseSwapPresentMode(const std::vector<VkPresentModeKHR> &availablePresentModes)
{`

### chooseSwapSurfaceFormat {#chooseswapsurfaceformat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VkSurfaceFormatKHR chooseSwapSurfaceFormat(const std::vector<VkSurfaceFormatKHR> &availableFormats)
`

### countof {#countof}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `#define countof(x) (sizeof(x) / sizeof(*(x)))
#endif

static const char  *validationLayers[]   = {"V`

### createImage {#createimage}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `void createImage(VkPhysicalDevice      physicalDevice,
                        VkDevice             `

### createImageView {#createimageview}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VkImageView createImageView(VkDevice dev, VkImage image, VkFormat format, VkImageAspectFlags aspectF`

### createShaderModule {#createshadermodule}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VkShaderModule createShaderModule(VkDevice device, const char *filename)
{`


## D

### debugCallback {#debugcallback}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VKAPI_CALL debugCallback(VkDebugUtilsMessageSeverityFlagBitsEXT      messageSeverity,
              `


## F

### findGraphicsQueueIndicies {#findgraphicsqueueindicies}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `bool findGraphicsQueueIndicies(VkPhysicalDevice device,
                                      VkSurf`

### findMemoryType {#findmemorytype}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `uint32_t findMemoryType(VkPhysicalDevice physicalDevice, uint32_t typeFilter, VkMemoryPropertyFlags `

### findSupportedFormat {#findsupportedformat}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `VkFormat findSupportedFormat(VkPhysicalDevice             physicalDevice,
                          `


## G

### getSwapChainProperties {#getswapchainproperties}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `void getSwapChainProperties(VkPhysicalDevice                 device,
                               `


## H

### hasAllExtensions {#hasallextensions}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `bool hasAllExtensions(VkPhysicalDevice device, const std::vector<const char *> &deviceExtensions)
{`


## R

### readFile {#readfile}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `void readFile(std::istream &s, std::vector<char> &data)
{`


## S

### supportsValidationLayers {#supportsvalidationlayers}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `bool supportsValidationLayers()
{`


## T

### transitionImageLayout {#transitionimagelayout}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/simpleVulkan/VulkanBaseApp.cpp](./VulkanBaseApp.cpp_docs.md)
- **Context**: `void transitionImageLayout(VulkanBaseApp *app,
                                  VkImage        imag`

