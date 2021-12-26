# vulkan-objects

> Breif summary of vulkan objects and how they relate. The order roughly corresponds to the order of creation. The summary is based on the [Vulkan Tutorial](https://vulkan-tutorial.com/).

## Table of Contents

<details><summary>Click to expand</summary>

- [`VkInstance`](#vkinstance)
- [`VkPhysicalDevice`](#vkphysicaldevice)
- [`VkSurfaceKHR`](#vksurfacekhr)
- [`VkDevice`](#vkdevice)
- [`VkQueue`](#vkqueue)
- [`VkSwapChainKHR`](#vkswapchainkhr)
- [`VkImage`](#vkimage)
- [`VkImageView`](#vkimageview)
- [`VkRenderPass`](#vkrenderpass)
- [`VkPipelineLayout`](#vkpipelinelayout)
- [`VkPipeline`](#vkpipeline)
- [`VkFramebuffer`](#vkframebuffer)
- [`VkCommandPool`](#vkcommandpool)
- [`VkCommandBuffer`](#vkcommandbuffer)
- [`VkSemaphore`](#vksemaphore)
- [`VkFence`](#vkfence)

</details>

## [`VkInstance`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkInstance.html)

The vulkan library is initialized by creating a vulkan instance object. You give vulkan information about your application and whether you want to enable validation layers or not.

The instance object is needed when creating a `VkSurface` and when setting up a debug messenger callback.

You also need the instance object when enumerating physical devices (`VkPhysicalDevice`).

The instance object is created first and destroyed last.

- Lifespan
  - [`vkCreateInstance()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateInstance.html)
  - [`vkDestroyInstance()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyInstance.html)
- Used in
  - [`vkCreateDebugUtilsMessengerEXT()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateDebugUtilsMessengerEXT.html)
  - [`vkDestroyDebugUtilsMessengerEXT()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyDebugUtilsMessengerEXT.html)
  - [`vkDestroySurfaceKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroySurfaceKHR.html)
  - [`glfwCreateWindowSurface()`](https://www.glfw.org/docs/latest/group__vulkan.html#ga1a24536bec3f80b08ead18e28e6ae965)
  - [`vkEnumeratePhysicalDevices()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkEnumeratePhysicalDevices.html)

## [`VkPhysicalDevice`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPhysicalDevice.html)

Object representing a physical device on the machine. Normally you use `vkEnumeratePhysicalDevices()` to get a list of `VkPhysicalDevice` where you pick the one that suits your needs the best.

The physical device is used when creating the _logical_ device and when finding out which queue families a physical device has support for, e.g. if a queue family has support for graphics operations (checking for the `VK_QUEUE_GRAPHICS_BIT` bit) and if it can present images to a surface (checking presentation support with `vkGetPhysicalDeviceSurfaceSupportKHR()`)

- Used in
  - [`vkCreateDevice()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateDevice.html)
  - [`vkGetPhysicalDeviceQueueFamilyProperties()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetPhysicalDeviceQueueFamilyProperties.html)
  - [`vkGetPhysicalDeviceSurfaceSupportKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetPhysicalDeviceSurfaceSupportKHR.html)

## [`VkSurfaceKHR`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkSurfaceKHR.html)

A surface object is platform agnostic but creating it is platform specific because it depends on window system details. However, we can use a function from `GLFW` to make this easier.

Needed together with a `VkPhysicalDevice` to query swap chain support for a device (`VkSurfaceCapabilitiesKHR`, `VkSurfaceFormatKHR` and `VkPresentModeKHR`).

Needed when creating a `VkSwapchainKHR` object (together with the logical device, see below).

- Lifespan
  - [`glfwCreateWindowSurface()`](https://www.glfw.org/docs/latest/group__vulkan.html#ga1a24536bec3f80b08ead18e28e6ae965) (platform independent)
  - [`vkDestroySurfaceKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroySurfaceKHR.html)
- Used in
  - [`vkGetPhysicalDeviceSurfaceCapabilitiesKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetPhysicalDeviceSurfaceCapabilitiesKHR.html)
  - [`vkGetPhysicalDeviceSurfaceFormatsKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetPhysicalDeviceSurfaceFormatsKHR.html)
  - [`vkGetPhysicalDeviceSurfacePresentModesKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetPhysicalDeviceSurfacePresentModesKHR.html)
  - [`vkGetPhysicalDeviceSurfaceSupportKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetPhysicalDeviceSurfaceSupportKHR.html)
  - [`VkSwapChainCreateInfoKHR struct`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkSwapchainCreateInfoKHR.html)

## [`VkDevice`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkDevice.html)

Handle to a logical device for interfacing with the physical device. This is arguably one of the most used objects and is needed for many different operations, e.g. when creating a `VkSwapChainKHR` object, getting handles to queue families (`VkQueue`), getting handles to swap chain images, creating image views, creating a render pass, creating pipeline layout, creating graphics pipeline, creating a frame buffer, creating a command pool, creating command buffers, creating and operating on synchronization objects (semaphores and fences), acquiring images from the swap chain and creating shader modules.

Creating a logical device also creates the queues associated with that device.

- Lifespan
  - [`vkCreateDevice()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateDevice.html)
  - [`vkDestroyDevice()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyDevice.html)
- Used in
  - [`vkDeviceWaitIdle()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDeviceWaitIdle.html)
  - [`vkGetDeviceQueue()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetDeviceQueue.html)
  - [`vkCreateSwapChainKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateSwapchainKHR.html)
  - [`vkDestroySwapChainKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroySwapchainKHR.html)
  - [`vkGetSwapchainImagesKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetSwapchainImagesKHR.html)
  - [`vkCreateImageView()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateImageView.html)
  - [`vkDestroyImageView()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyImageView.html)
  - [`vkAcquireNextImageKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkAcquireNextImageKHR.html)
  - [`vkCreateRenderPass()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateRenderPass.html)
  - [`vkDestroyRenderPass()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyRenderPass.html)
  - [`vkCreatePipelineLayout()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreatePipelineLayout.html)
  - [`vkDestroyPipelineLayout()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyPipelineLayout.html)
  - [`vkCreateGraphicsPipelines()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateGraphicsPipelines.html)
  - [`vkDestroyPipeline()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyPipeline.html)
  - [`vkCreateShaderModule()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateShaderModule.html)
  - [`vkDestroyShaderModule()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyShaderModule.html)
  - [`vkCreateFrameBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateFramebuffer.html)
  - [`vkDestroyFrameBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyFramebuffer.html)
  - [`vkCreateCommandPool()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateCommandPool.html)
  - [`vkDestroyCommandPool()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyCommandPool.html)
  - [`vkAllocateCommandBuffers()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkAllocateCommandBuffers.html)
  - [`vkCreateSemaphore()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateSemaphore.html)
  - [`vkDestroySemaphore()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroySemaphore.html)
  - [`vkCreateFence()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateFence.html)
  - [`vkDestroyFence()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyFence.html)
  - [`vkWaitForFences()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkWaitForFences.html)
  - [`vkResetFences()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkResetFences.html)

## [`VkQueue`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkQueue.html)

A `VkQueue` object is a handle to a queue family.

- Used in
  - [`vkQueueSubmit()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkQueueSubmit.html) if the queue is a graphics queue
  - [`vkQueuePresentKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkQueuePresentKHR.html) if the queue is a presentation queue

## [`VkSwapChainKHR`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkSwapchainKHR.html)

The general purpose of the swap chain is to synchronize the presentation of images with the refresh rate of the screen.

Passed into `VkPresentInfoKHR` when presenting an image to the presentation queue by calling `vkQueuePresentKHR()`.

- Lifetime
  - [`vkCreateSwapChainKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateSwapChainKHR.html)
  - [`vkDestroySwapChainKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroySwapchainKHR.html)
- Used in
  - [`vkGetSwapchainImagesKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetSwapchainImagesKHR.html)
  - [`vkAcquireNextImageKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkAcquireNextImageKHR.html)
  - [`VkPresentInfoKHR`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPresentInfoKHR.html)

## [`VkImage`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkImage.html)

A handle to an image. There's no need for any clean up code for images owned by a swap chain.

- Used in
  - [`vkGetSwapchainImagesKHR()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkGetSwapchainImagesKHR.html)

## [`VkImageView`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkImageView.html)

A view into a `VkImage`. A `VkImageView` is sufficient to start using an image as a texture, but in order to render to it, it needs to be associated with a frame buffer. Passed into `VkFrameBufferCreateInfo` when setting up the frame buffer for a particular image in the swap chain (together with a render pass object).

- Lifetime
  - [`vkCreateImageView()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateImageView.html)
  - [`vkDestroyImageView()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyImageView.html)
- Used in
  - [`vkCreateFrameBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateFramebuffer.html)

## [`VkRenderPass`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkRenderPass.html)

Describes attachments, subpasses and subpass dependencies used in the graphics pipeline. Also specified when creating frame buffers.

- Lifetime
  - [`vkCreateRenderPass()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateRenderPass.html)
  - [`vkDestroyRenderPass()`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyRenderPass.html)
- Used in
  - [`VkGraphicsPipelineCreateInfo struct`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkGraphicsPipelineCreateInfo.html)
  - [`VkFramebufferCreateInfo struct`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkFramebufferCreateInfo.html)

## [`VkPipelineLayout`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineLayout.html)

Describes a set of resources (descriptor set layouts and push constant ranges) that can be used by a pipeline. Created together with the graphics pipeline and used with `VkRenderPass` in the `VkGraphicsPipelineCreateInfo` struct.

- Lifetime
  - [`vkCreatePipelineLayout()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreatePipelineLayout.html)
  - [`vkDestroyPipelineLayout()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyPipelineLayout.html)
- Used in
  - [`VkGraphicsPipelineCreateInfo struct`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkGraphicsPipelineCreateInfo.html)

## [`VkPipeline`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipeline.html)

Bound to a command buffer when calling `vkCmdBindPipeline()`.

- Lifetime
  - [`vkCreateGraphicsPipelines()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateGraphicsPipelines.html) NOTE that this is plural for creating many graphics pipelines in a single call
  - [`vkDestroyPipeline()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyPipeline.html)
- Used in
  - [`vkCmdBindPipeline()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdBindPipeline.html)

## [`VkFramebuffer`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkFramebuffer.html)

Passed into `VkRenderPassBeginInfo` before calling `vkCmdBeginRenderPass()` when setting up a command buffer.

- Lifetime
  - [`vkCreateFrameBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateFramebuffer.html)
  - [`vkDestroyFrameBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyFramebuffer.html)
- Used in
  - [`VkRenderPassBeginInfo struct`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkRenderPassBeginInfo.html)

## [`VkCommandPool`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkCommandPool.html)

A command pool is needed in order to create command buffers, which are allocated from the command pool.

- Lifetime
  - [`vkCreateCommandPool()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateCommandPool.html)
  - [`vkDestroyCommandPool()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyCommandPool.html)
- Used in
  - [`VkCommandBufferAllocateInfo struct`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkCommandBufferAllocateInfo.html)

## [`VkCommandBuffer`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkCommandBuffer.html)

Command buffers record commands that you want to execute. They are executed by submitting them to a device queue. A command buffer will be automatically freed when their command pool is destroyed. You need one command buffer per frame buffer.

- Lifetime
  - [`vkAllocateCommandBuffers()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkAllocateCommandBuffers.html)
- Used in
  - [`vkBeginCommandBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkBeginCommandBuffer.html)
  - [`vkCmdBeginRenderPass()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdBeginRenderPass.html)
  - [`vkCmdBindPipeline()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdBindPipeline.html)
  - [`vkCmdDraw()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdDraw.html)
  - [`vkCmdEndRenderPass()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdEndRenderPass.html)
  - [`vkEndCommandBuffer()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkEndCommandBuffer.html)

## [`VkSemaphore`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkSemaphore.html)

A semaphore is used for GPU-GPU synchronization and is created in the application but never explicitly waited for in the main thread.

- Lifetime
  - [`vkCreateSemaphore()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateSemaphore.html)
  - [`vkDestroySemaphore()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroySemaphore.html)
- Used in
  - [`vkAcquireNextImageKHR()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkAcquireNextImageKHR.html)
  - [`VkSubmitInfo struct`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkSubmitInfo.html)

## [`VkFence`](https://khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkFence.html)

A fence  is used for CPU-GPU synchronization.

- Lifetime
  - [`vkCreateFence()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateFence.html)
  - [`vkDestroyFence()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkDestroyFence.html)
- Used in
  - [`vkQueueSubmit()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkQueueSubmit.html)
  - [`vkWaitForFences()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkWaitForFences.html)
  - [`vkResetFences()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkResetFences.html)
