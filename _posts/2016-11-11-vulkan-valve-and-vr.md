---
layout: post
title:  "Vulkan、VR 和 Valve，三个 V 走到一起了"
categories: Vulkan
tags:  Vulkan VR Valve NVIDIA
---

![]({{ site.url }}/media/vulkan-valve-vr-preface.jpg)

改编自 https://developer.nvidia.com/getting-vulkan-ready-vr

原作者 James Jones, Mathias Schott

这个事情是这样了，我们都知道 Vulkan 很酷，VR 是人类的未来，那么能否加起来一起用呢？
它们的首字母都一样，合体应该简单。

![]({{ site.url }}/media/vulkan-vr-1.jpg)

![]({{ site.url }}/media/vulkan-vr-2.jpg)

![]({{ site.url }}/media/vulkan-vr-3.jpg)


不巧的是，和许多新技术一样，到底是先有鸡还是先有蛋？如果用 Vulkan 开发 VR 的人很少，那么 Valve、Oculus 等平台商没有动力去支持 Vulkan。反过来，如果你们都不支持 Vulkan，内容开发商怎么用 Vulkan 呢？

更为不幸的是，VR 头盔的运行库通常是用一种渲染 API 编写的，比方说在 Windows 上用的是 D3D11，如果内容开发商用 Vulkan，那么还得让 D3D11 和 Vulkan 通信。




WTF！！！
=
作为生态圈重要的一环，NVIDIA 急人所急，适时推出扩展，给大伙搞定了这个难题。

VK_NV_external_memory 扩展大礼包
=
这个礼包允许开发者从把另一个进程的内存导出给 Vulkan 用，同时将 D3D 的贴图转成 Vulkan 的贴图。 这样 D3D 进程和 Vulkan 进程都可以访问这个贴图。大礼包的扩展分成两组，操作系统无关的扩展和操作系统相关的扩展。


- VK_NV_external_memory_capabilities
    - 查询跨进程和跨 API 的能力
- VK_NV_external_memory
    - 平台无关的外部内存扩展
- VK_NV_external_memory_win32
    - 平台相关的外部内存扩展
- VK_NV_win32_keyed_mutex 
    - DXGI 的 mutex 对象现在可以连着 Vulkan 的 semaphore 同步访问 D3D 的内存。

下面是代码时间，推荐直接跳过，到文章的末尾给我点赞+分享。

![]({{ site.url }}/media/talk-is-cheap.jpg)

查询驱动对 D3D11 Image 的支持情况
=
先查询是否支持 HANDLE_TYPE_D3D11_IMAGE_BIT 的外部内存

```C++
 VkExternalMemoryHandleTypeFlagsNV handleType = VK_EXTERNAL_MEMORY_HANDLE_TYPE_D3D11_IMAGE_BIT_NV;
 VkResult res = vkGetPhysicalDeviceExternalImageFormatPropertiesNV(
    physicalDevice,
    VK_FORMAT_R8G8B8A8_UNORM, VK_IMAGE_TYPE_2D, VK_IMAGE_TILING_OPTIMAL, VK_IMAGE_USAGE_COLOR_ATTACHMENT_BIT,
    handleType,
    &extProperties);
```

如果不支持，再试试 HANDLE_TYPE_D3D11_IMAGE_KMT_BIT 类型

```C++
 if (res == VK_ERROR_FORMAT_NOT_SUPPORTED) {
    handleType = VK_EXTERNAL_MEMORY_HANDLE_TYPE_D3D11_IMAGE_KMT_BIT_NV;
    VkResult res = vkGetPhysicalDeviceExternalImageFormatPropertiesNV(...);
    if (res == VK_ERROR_FORMAT_NOT_SUPPORTED) {
        FAIL();
    }
 }
```

初步检查通过后，再看看这个外部内存能否导入（FEATURE_IMPORTABLE_BIT）

```C++
 if (!(extProperties.externalMemoryFeatures & VK_EXTERNAL_MEMORY_FEATURE_IMPORTABLE_BIT_NV)) {
    FAIL();
 }
```

创建 D3D11 的导出贴图
=
这段代码可以放在 Vulkan 进程里，也可以放在 D3D11 进程里，总之生成的 d3dTexture 在两个进程间是可以共享的。

```C++
 D3D11_TEXTURE2D_DESC descColor;
 memset(&descColor, 0, sizeof(descColor));
 descColor.Width = 512;
 descColor.Height = 512;
 descColor.MipLevels = 1;
 descColor.ArraySize = 1;
 descColor.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
 descColor.SampleDesc.Count = 1;
 descColor.SampleDesc.Quality = 0;
 descColor.Usage = D3D11_USAGE_DEFAULT;
 descColor.BindFlags = D3D11_BIND_RENDER_TARGET;
 descColor.CPUAccessFlags = 0;

 if (handleType == VK_EXTERNAL_MEMORY_HANDLE_TYPE_D3D11_IMAGE_BIT_NV) {
     descColor.MiscFlags = D3D11_RESOURCE_MISC_SHARED_NTHANDLE | D3D11_RESOURCE_MISC_SHARED_KEYEDMUTEX;
 } else {
     descColor.MiscFlags = D3D11_RESOURCE_MISC_SHARED_KEYEDMUTEX;
 }

 d3dDevice->CreateTexture2D(&descColor, 0, &d3dTexture);

 d3dTextureSharedHandle = 0;
 if (handleType == VK_EXTERNAL_MEMORY_HANDLE_TYPE_D3D11_IMAGE_BIT_NV) {
     IDXGIResource1* tempResource = NULL;
     d3dTexture->QueryInterface(__uuidof(IDXGIResource1), (void**)&tempResource);
     tempResource->CreateSharedHandle(NULL,
                                      DXGI_SHARED_RESOURCE_READ | DXGI_SHARED_RESOURCE_WRITE,
                                      NULL,
                                      &d3dTextureSharedHandle);
     tempResource->Release();
 } else {
     IDXGIResource* tempResource = NULL;
     d3dTexture->QueryInterface(__uuidof(IDXGIResource), (void**)&tempResource);
     tempResource->GetSharedHandle(&d3dTextureSharedHandle);
     tempResource->Release();
 }
```

创建 Vulkan 进程的导入贴图
=

handleType 和 D3D 端的一致

```C++
 VkExternalMemoryImageCreateInfoNV extMemoryImageInfo = {
     VK_STRUCTURE_TYPE_EXTERNAL_MEMORY_IMAGE_CREATE_INFO_NV,
     0,
     handleType,
 };
```

如果外部内存拥有 DEDICATED_ONLY_BIT 属性，开发者必须使用 VK_NV_dedicated_allocation 扩展。

```C++
VkDedicatedAllocationImageCreateInfoNV dedicatedImageCreateInfo = {
     VK_STRUCTURE_TYPE_DEDICATED_ALLOCATION_IMAGE_CREATE_INFO_NV,
     0,
     VK_FALSE
 };

 if (extProperties.externalMemoryFeatures & VK_EXTERNAL_MEMORY_FEATURE_DEDICATED_ONLY_BIT_NV) {
     externalMemoryImageInfo.pNext = &dedicatedImageCreateInfo;
     dedicatedImageCreateInfo.dedicatedAllocation = VK_TRUE;
 }
```

然后是创建贴图

```C++
 VkImageCreateInfo imageCreateInfo = {
     VK_STRUCTURE_TYPE_IMAGE_CREATE_INFO,
     &externalMemoryImageInfo
 };

 imageCreateInfo.imageType = VK_IMAGE_TYPE_2D;
 imageCreateInfo.format = VK_FORMAT_R8G8B8A8_UNORM;
 iamgeCreateInfo.extent.width = 512;
 imageCreateInfo.extent.height = 512;
 imageCreateInfo.extent.depth = 1;
 imageCreateInfo.mipLevels = 1;
 imageCreateInfo.arrayLayers = 1;
 imageCreateInfo.samples = VK_SAMPLE_COUNT_1_BIT;
 imageCreateInfo.tiling = VK_IMAGE_TILING_OPTIMAL;
 imageCreateInfo.usage = VK_IMAGE_USAGE_COLOR_ATTACHMENT_BIT;
 imageCreateInfo.flags = 0;
 imageCreateInfo.sharingMode = VK_SHARING_MODE_EXCLUSIVE;


 vkCreateImage(device, &imageCreateInfo, NULL, &image);
```

绑定内存和 Vulkan 贴图
=
在 Vulkan 里，贴图占用的显存是需要开发者调用 vkAllocateMemory 手动创建的，还需要调用 vkBindImageMemory 来绑定内存和贴图。当然，如果用了 VK_NV_dedicated_allocation 扩展，可以省略第二步。

```C++
 VkMemoryRequirements memReqs;
 vkGetImageMemoryRequirements(m_device, m_buffer[i].image, &memReqs);

 uint32_t memoryTypeIndex = getMemoryTypeIndex(memReqs.memoryTypeBits, VK_MEMORY_PROPERTY_DEVICE_LOCAL_BIT);

 VkImportMemoryWin32HandleInfoNV importMemInfoNV = { 
     VK_STRUCTURE_TYPE_IMPORT_MEMORY_WIN32_HANDLE_INFO_NV, // sType
     0, // pNext
     handleType, // handleTypes
     d3dTextureSharedHandle // handle
 };

 VkMemoryAllocateInfo memInfo = {
     VK_STRUCTURE_TYPE_MEMORY_ALLOCATE_INFO, // sType
     &importMemInfoNV, // pNext
     memReqs.size, // allocationSize
     memoryTypeIndex, // memoryTypeIndex
 };

 VkDedicatedAllocationMemoryAllocateInfoNV dedicatedAllocationInfo = {
     VK_STRUCTURE_TYPE_DEDICATED_ALLOCATION_MEMORY_ALLOCATE_INFO_NV // sType
 };

 if (extProperties.externalMemoryFeatures & VK_EXTERNAL_MEMORY_FEATURE_DEDICATED_ONLY_BIT_NV) {
     dedicatedAllocationInfo.image = image;
     importMemInfo.pNext = &dedicatedAllocationInfo;
 }

 vkAllocateMemory(device, &memInfo, 0, &mem);

 if (!(extProperties.externalMemoryFeatures & VK_EXTERNAL_MEMORY_FEATURE_DEDICATED_ONLY_BIT_NV)) {
     vkBindImageMemory(device, image, mem, 0);
 }
```

跨进程、跨 API 的同步
=
在 Vulkan 中，队列（Queue）是传递命令给 GPU 的通道，这里与 D3D 贴图关联的 keyed mutex 就发挥了同步的作用。

```C++
 VkSubmitInfo submitInfo = {
     VK_STRUCTURE_TYPE_SUBMIT_INFO,
     0
 };

 submitInfo.commandBufferCount = 1;
 submitInfo.pCommandBuffers = cmdBuf;

 VkWin32KeyedMutexAcquireReleaseInfoNV keyedMutex = { 
     VK_STRUCTURE_TYPE_WIN32_KEYED_MUTEX_ACQUIRE_RELEASE_INFO_NV, 
     0, // pNext
     1, // acquireCount
     &mem, // pAcquireSyncs
     &acquireKey, // pAcquireKeys
     &timeout, // pAcquireTimeoutMilliseconds
     1, // releaseCount
     &mem, // pReleaseSyncs
     &releaseKey // pReleaseKeys
 };

 submitInfo.pNext = &keyedMutex;
 vkQueueSubmit(queue, 1, &submitInfo, fence);
```

绕这么个大圈子，就是为了大伙儿用 Vulkan （在 N 卡上）开发 VR 内容，容易麽？

为啥你没提 Valve？
=
Valve 支持 Vulkan 是大家都知道的事情，放在标题里是为了凑数。


----
http://vinjn.com - 关注人工智能+视觉计算