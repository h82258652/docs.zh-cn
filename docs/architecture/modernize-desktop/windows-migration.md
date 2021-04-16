---
title: Windows 10 迁移
description: 深入了解打包、XAML 岛等 Windows 10 功能。
ms.date: 12/29/2020
ms.openlocfilehash: 139a8f2354803dafeb0178b4dbfb57a95c4ddb34
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98615940"
---
# <a name="windows-10-migration"></a><span data-ttu-id="52b99-103">Windows 10 迁移</span><span class="sxs-lookup"><span data-stu-id="52b99-103">Windows 10 migration</span></span>

<span data-ttu-id="52b99-104">设想一下：你有一个可正常运行的桌面应用程序，它是在 Windows 7 时代开发的。</span><span class="sxs-lookup"><span data-stu-id="52b99-104">Consider the following situation: You have a working desktop application that was developed in the Windows 7 days.</span></span> <span data-ttu-id="52b99-105">它使用的是当时提供的 WPF 技术，并且运行良好。但是，当你在 Windows 10 上运行它时，它的 UI 和行为已经过时。</span><span class="sxs-lookup"><span data-stu-id="52b99-105">It's using WPF technology available at that time and working fine but it has an outdated UI and behaviors when you run it on Windows 10.</span></span> <span data-ttu-id="52b99-106">这就像你在《黑客帝国》这样的未来主义电影中，看到 Neo 使用 Nokia 8110 设备一样。</span><span class="sxs-lookup"><span data-stu-id="52b99-106">It is like when you watch a futuristic movie like Matrix and you see Neo using the Nokia 8110 device.</span></span> <span data-ttu-id="52b99-107">这部电影在 20 年后依然可圈可点，但是如果有设备现代化技术的加持，它会表现更出色。</span><span class="sxs-lookup"><span data-stu-id="52b99-107">The film works great after 20 years but it would rather benefit from a device modernization.</span></span>

<span data-ttu-id="52b99-108">随着 Windows 10 的发布，Microsoft 推出了许多创新，以支持平板电脑和触摸设备等场景，并为用户提供有史以来最棒的 Microsoft 操作系统体验。</span><span class="sxs-lookup"><span data-stu-id="52b99-108">With the release of Windows 10, Microsoft introduced many innovations to support scenarios like tablets and touch devices and to provide the best experience for users for a Microsoft operating system ever.</span></span> <span data-ttu-id="52b99-109">例如，你能够：</span><span class="sxs-lookup"><span data-stu-id="52b99-109">For example, you can:</span></span>

- <span data-ttu-id="52b99-110">使用 Windows Hello 通过人脸登录。</span><span class="sxs-lookup"><span data-stu-id="52b99-110">Sign in with your face using Windows Hello.</span></span>
- <span data-ttu-id="52b99-111">笔写或手写可自动识别并数字化的文本。</span><span class="sxs-lookup"><span data-stu-id="52b99-111">Use a pen to draw or handwrite text that is automatically recognized and digitalized.</span></span>
- <span data-ttu-id="52b99-112">使用 WinML 在本地运行以云为基础构建的自定义 AI 模型。</span><span class="sxs-lookup"><span data-stu-id="52b99-112">Run locally customized AI models built on the cloud using WinML.</span></span>

<span data-ttu-id="52b99-113">通过 Windows 运行时 (WinRT) 库，可为 Windows 开发人员启用所有这些功能。</span><span class="sxs-lookup"><span data-stu-id="52b99-113">All these features are enabled for Windows developers through Windows Runtime (WinRT) libraries.</span></span> <span data-ttu-id="52b99-114">你可以在现有桌面应用中利用这些功能，因为 WinRT 库也同时对 .NET Framework 和 .NET 公开。</span><span class="sxs-lookup"><span data-stu-id="52b99-114">You can take advantage of these features in your existing desktop apps because the libraries are exposed to both the .NET Framework and .NET as well.</span></span> <span data-ttu-id="52b99-115">你甚至可以使用 XAML 岛来实现 UI 的现代化，并根据时代的发展来改善应用的视觉效果和行为。</span><span class="sxs-lookup"><span data-stu-id="52b99-115">You can even modernize your UI with the use of XAML Islands and improve the visuals and behavior of your apps according to the times.</span></span>

<span data-ttu-id="52b99-116">这里有一点很重要，那就是你不需要放弃 .NET Framework 技术来走这条现代化道路。</span><span class="sxs-lookup"><span data-stu-id="52b99-116">One important thing to note here is that you don't need to abandon .NET Framework technology to follow this modernization path.</span></span> <span data-ttu-id="52b99-117">你可以安全地留在原地，享受 Windows 10 带来的所有好处，而无需迁移到 .NET。</span><span class="sxs-lookup"><span data-stu-id="52b99-117">You can safely stay on there and have all the benefits of Windows 10 without the pressure to migrate to .NET.</span></span> <span data-ttu-id="52b99-118">也就是说，你拥有了强大的功能和灵活性来选择你的现代化道路。</span><span class="sxs-lookup"><span data-stu-id="52b99-118">So, you get both the power and the flexibility to choose your modernization path.</span></span>

## <a name="winrt-apis"></a><span data-ttu-id="52b99-119">WinRT API</span><span class="sxs-lookup"><span data-stu-id="52b99-119">WinRT APIs</span></span>

<span data-ttu-id="52b99-120">WinRT API 是面向对象、结构良好的应用程序编程接口 (API)，可让 Windows 10 开发人员访问操作系统必须提供的所有功能。</span><span class="sxs-lookup"><span data-stu-id="52b99-120">WinRT APIs are object-oriented, well-structured application programming interfaces (APIs) that give Windows 10 developers access to everything the operating system has to offer.</span></span> <span data-ttu-id="52b99-121">通过 WinRT API，你可以在桌面应用上集成推送通知、设备 API、Microsoft Ink 和 WinML 等功能。</span><span class="sxs-lookup"><span data-stu-id="52b99-121">Through WinRT APIs, you can integrate functionalities like Push Notifications, Device APIs, Microsoft Ink, and WinML, among others on your desktop apps.</span></span>

<span data-ttu-id="52b99-122">一般来说，WinRT API 可以从经典桌面应用中调用。</span><span class="sxs-lookup"><span data-stu-id="52b99-122">In general, WinRT APIs can be called from a classic desktop app.</span></span> <span data-ttu-id="52b99-123">但是，有两个主要 API 打破了这项规则：</span><span class="sxs-lookup"><span data-stu-id="52b99-123">However, two main areas present an exception to this rule:</span></span>

* <span data-ttu-id="52b99-124">需要包标识的 API。</span><span class="sxs-lookup"><span data-stu-id="52b99-124">APIs that require a package identity.</span></span>
* <span data-ttu-id="52b99-125">需要 XAML 或 Composition 等可视化效果的 API。</span><span class="sxs-lookup"><span data-stu-id="52b99-125">APIs that require visualization like XAML or Composition.</span></span>

### <a name="universal-windows-platform-uwp-packages"></a><span data-ttu-id="52b99-126">通用 Windows 平台 (UWP) 包</span><span class="sxs-lookup"><span data-stu-id="52b99-126">Universal Windows Platform (UWP) packages</span></span>

#### <a name="application-package-identity"></a><span data-ttu-id="52b99-127">应用程序包标识</span><span class="sxs-lookup"><span data-stu-id="52b99-127">Application Package Identity</span></span>

<span data-ttu-id="52b99-128">UWP 应用有一个部署系统，操作系统在其中管理应用程序的安装和卸载。</span><span class="sxs-lookup"><span data-stu-id="52b99-128">UWP apps have a deployment system where the OS manages the installation and uninstallation of application.</span></span> <span data-ttu-id="52b99-129">这要求安装必须是声明性的，也就是说在安装过程中不执行任何用户代码。</span><span class="sxs-lookup"><span data-stu-id="52b99-129">That requires the installation to be declarative, meaning that no user code is executed during install.</span></span> <span data-ttu-id="52b99-130">而应用想要与系统集成的一切，如协议、文件类型和扩展，都会在应用程序清单中声明。</span><span class="sxs-lookup"><span data-stu-id="52b99-130">Instead, everything the app wants to integrate with the system, such as protocols, file types, and extensions, is declared in the application manifest.</span></span> <span data-ttu-id="52b99-131">在部署时，部署管道会配置这些集成点。</span><span class="sxs-lookup"><span data-stu-id="52b99-131">At deployment time, the deployment pipeline configures those integration points.</span></span> <span data-ttu-id="52b99-132">操作系统管理此功能并跟踪它的唯一方式是，让每个“包”都有一个标识，即，应用程序的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="52b99-132">The only way for the OS to manage all this functionality and keep track of it is for each 'package' to have an identity, a unique identifier for the application.</span></span>

<span data-ttu-id="52b99-133">一些 WinRT API 需要这个包标识才能按预期工作。</span><span class="sxs-lookup"><span data-stu-id="52b99-133">Some WinRT APIs require this package identity to work as expected.</span></span> <span data-ttu-id="52b99-134">但是，本机 C++ 或 .NET 应用等经典桌面应用使用不同的部署系统，这类系统不需要包标识。</span><span class="sxs-lookup"><span data-stu-id="52b99-134">However, classic desktop apps like native C++ or .NET apps, use different deployment systems that don't require a package identity.</span></span> <span data-ttu-id="52b99-135">如果你想在桌面应用程序中使用这些 WinRT API，就需要为它们提供一个包标识。</span><span class="sxs-lookup"><span data-stu-id="52b99-135">If you want to use these WinRT APIs in your desktop application, you need to provide them a package identity.</span></span>

<span data-ttu-id="52b99-136">一种方法是生成一个额外的打包项目。</span><span class="sxs-lookup"><span data-stu-id="52b99-136">One way to proceed is to build an additional packaging project.</span></span> <span data-ttu-id="52b99-137">在该打包项目中，指向原始源代码项目并指定要提供的标识信息。</span><span class="sxs-lookup"><span data-stu-id="52b99-137">Inside the packaging project, you point to the original source code project and specify the Identity information you want to provide.</span></span> <span data-ttu-id="52b99-138">如果安装包并运行已安装的应用，它会自动获取标识，使你的代码能够调用所有需要标识的 WinRT API。</span><span class="sxs-lookup"><span data-stu-id="52b99-138">If you install the package and run the installed app, it will automatically get an identify enabling your code to call all WinRT APIs requiring Identity.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">
    <Identity Name="YOUR-APP-GUID "
              Publisher="CN=YOUR COMPANY"
              Version="1.x.x.x" />
</Package>
```

<span data-ttu-id="52b99-139">你可以通过检查包含 API 的类型是否标有 [DualApiPartition](xref:Windows.Foundation.Metadata.DualApiPartitionAttribute) 属性，来检查哪些 API 需要打包的应用程序标识。</span><span class="sxs-lookup"><span data-stu-id="52b99-139">You can check which APIs need a packaged application identity by inspecting if the type that contains the API is marked with the [DualApiPartition](xref:Windows.Foundation.Metadata.DualApiPartitionAttribute) attribute.</span></span> <span data-ttu-id="52b99-140">如果是，则可以从未打包的传统桌面应用中调用它。</span><span class="sxs-lookup"><span data-stu-id="52b99-140">If it is, you can call if from an unpackaged traditional desktop app.</span></span> <span data-ttu-id="52b99-141">否则，必须借助打包项目将经典桌面应用转换为 UWP。</span><span class="sxs-lookup"><span data-stu-id="52b99-141">Otherwise, you must convert your classic desktop app to a UWP with the help of a packaging project.</span></span>

<https://docs.microsoft.com/windows/desktop/apiindex/uwp-apis-callable-from-a-classic-desktop-app>

#### <a name="benefits-of-packaging"></a><span data-ttu-id="52b99-142">打包的好处</span><span class="sxs-lookup"><span data-stu-id="52b99-142">Benefits of packaging</span></span>

<span data-ttu-id="52b99-143">除了允许你访问这些 API，你还可以通过为桌面应用程序创建 Windows 应用包来获得一些额外的好处，其中包括：</span><span class="sxs-lookup"><span data-stu-id="52b99-143">Besides giving you access to these APIs, you get some additional benefits by creating a Windows App package for your desktop application including:</span></span>

* <span data-ttu-id="52b99-144">**简化的部署**。</span><span class="sxs-lookup"><span data-stu-id="52b99-144">**Streamlined deployment**.</span></span> <span data-ttu-id="52b99-145">应用具有出色的部署体验，确保用户可以放心地安装和更新应用程序。</span><span class="sxs-lookup"><span data-stu-id="52b99-145">Apps have a great deployment experience ensuring that users can confidently install an application and update it.</span></span> <span data-ttu-id="52b99-146">如果用户选择卸载应用，应用将被彻底删除，不留任何痕迹，从而防止出现 Windows rot 问题。</span><span class="sxs-lookup"><span data-stu-id="52b99-146">If a user chooses to uninstall the app, it's removed completely with no trace left behind preventing the Windows rot problem.</span></span>

* <span data-ttu-id="52b99-147">**自动更新和许可**。</span><span class="sxs-lookup"><span data-stu-id="52b99-147">**Automatic updates and licensing**.</span></span> <span data-ttu-id="52b99-148">你的应用程序可以加入 Microsoft Store 的内置许可和自动更新机制。</span><span class="sxs-lookup"><span data-stu-id="52b99-148">Your application can participate in the Microsoft Store's built-in licensing and automatic update facilities.</span></span> <span data-ttu-id="52b99-149">自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。</span><span class="sxs-lookup"><span data-stu-id="52b99-149">Automatic update is a highly reliable and efficient mechanism, because only the changed parts of files are downloaded.</span></span>

* <span data-ttu-id="52b99-150">**扩大了覆盖范围，并简化了盈利**。</span><span class="sxs-lookup"><span data-stu-id="52b99-150">**Increased reach and simplified monetization**.</span></span> <span data-ttu-id="52b99-151">你的情况可能并非如此，但如果你选择通过 Microsoft Store 分发应用程序，你将接触到数以百万计的 Windows 10 用户。</span><span class="sxs-lookup"><span data-stu-id="52b99-151">Maybe not your case but if you choose to distribute your application through the Microsoft Store you reach millions of Windows 10 users.</span></span>

* <span data-ttu-id="52b99-152">**添加 UWP 功能**。</span><span class="sxs-lookup"><span data-stu-id="52b99-152">**Add UWP features**.</span></span> <span data-ttu-id="52b99-153">你可以按照自己的进度将 UWP 功能添加到应用包中。</span><span class="sxs-lookup"><span data-stu-id="52b99-153">You can add UWP features to your app's package at your own pace.</span></span>

#### <a name="prepare-for-packaging"></a><span data-ttu-id="52b99-154">准备打包</span><span class="sxs-lookup"><span data-stu-id="52b99-154">Prepare for packaging</span></span>

<span data-ttu-id="52b99-155">在继续打包桌面应用程序之前，必须先解决一些要点问题。</span><span class="sxs-lookup"><span data-stu-id="52b99-155">Before proceeding to package your desktop application, there are some points you have to address before starting the process.</span></span> <span data-ttu-id="52b99-156">应用程序必须遵守 Microsoft Store 的任何规则和策略，并在 UWP 应用程序模型中运行。</span><span class="sxs-lookup"><span data-stu-id="52b99-156">Your application must respect any of the Microsoft Store rules and policies and run in the UWP application model.</span></span> <span data-ttu-id="52b99-157">例如，它必须在 .NET Framework 4.6.2 或更高版本上运行，并写入 `HKEY_CURRENT_USER` 注册表配置单元，AppData 文件夹将被虚拟化到用户特定的应用本地位置。</span><span class="sxs-lookup"><span data-stu-id="52b99-157">For example, it has to run on the .NET Framework 4.6.2 or later and writes to the `HKEY_CURRENT_USER` registry hive and the AppData folders will be virtualized to a user-specific app-local location.</span></span>

<span data-ttu-id="52b99-158">打包的设计目标是将应用程序状态与系统状态分离，同时保持与其他应用的兼容性。</span><span class="sxs-lookup"><span data-stu-id="52b99-158">The design goal for packaging is to separate the application state from system state while maintaining compatibility with other apps.</span></span> <span data-ttu-id="52b99-159">Windows 10 通过将应用程序放入 UWP 包来实现这一目标。</span><span class="sxs-lookup"><span data-stu-id="52b99-159">Windows 10 accomplishes this goal by placing the application inside a UWP package.</span></span> <span data-ttu-id="52b99-160">它在运行时检测并重定向文件系统和注册表的一些更改，以履行通过打包提供可信、干净的应用程序安装和卸载行为的承诺。</span><span class="sxs-lookup"><span data-stu-id="52b99-160">It detects and redirects some changes to the file system and registry at run time to fulfill the promise of a trusted and clean install and uninstall behavior of an application provided by packaging.</span></span>

<span data-ttu-id="52b99-161">你为桌面应用程序创建的包是仅适用于桌面且未沙盒化的完全信任应用程序，不过，针对对 `HKCU` 和 `AppData` 的写入，该应用程序已应用轻量级虚拟化。</span><span class="sxs-lookup"><span data-stu-id="52b99-161">Packages that you create for your desktop application are desktop-only, full-trust applications that aren't sandboxed, although there's lightweight virtualization applied to the app for writes to `HKCU` and `AppData`.</span></span> <span data-ttu-id="52b99-162">此虚拟化使它们能够像经典桌面应用程序一样与其他应用进行交互。</span><span class="sxs-lookup"><span data-stu-id="52b99-162">This virtualization allows them to interact with other apps the same way classic desktop applications do.</span></span>

##### <a name="installation"></a><span data-ttu-id="52b99-163">安装</span><span class="sxs-lookup"><span data-stu-id="52b99-163">Installation</span></span>

<span data-ttu-id="52b99-164">应用包安装在 %ProgramFiles%\\WindowsApps\\package_name 下面，具有名为 `app_name.exe` 的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="52b99-164">App packages are installed under *%ProgramFiles%\\WindowsApps\\package_name*, with the executable titled `app_name.exe`.</span></span> <span data-ttu-id="52b99-165">每个包文件夹都包含一个名为 `AppxManifest.xml` 的清单，清单中包含打包应用的特殊 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="52b99-165">Each package folder contains a manifest (named `AppxManifest.xml`) that contains a special XML namespace for packaged apps.</span></span> <span data-ttu-id="52b99-166">该清单文件内部是一个 `<EntryPoint>` 元素，该元素引用完全信任的应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-166">Inside that manifest file is an `<EntryPoint>` element, which references the full-trust app.</span></span> <span data-ttu-id="52b99-167">启动该应用程序时，它不会在应用容器内运行，而是像往常一样以用户身份运行。</span><span class="sxs-lookup"><span data-stu-id="52b99-167">When that application is launched, it doesn't run inside an app container, but instead it runs as the user as it normally would.</span></span>

<span data-ttu-id="52b99-168">部署后，软件包文件由操作系统标记为只读并严格锁定。</span><span class="sxs-lookup"><span data-stu-id="52b99-168">After deployment, package files are marked read-only and heavily locked down by the operating system.</span></span> <span data-ttu-id="52b99-169">如果这些文件遭到篡改，Windows 将阻止应用启动。</span><span class="sxs-lookup"><span data-stu-id="52b99-169">Windows prevents apps from launching if these files are tampered with.</span></span>

##### <a name="file-system"></a><span data-ttu-id="52b99-170">文件系统</span><span class="sxs-lookup"><span data-stu-id="52b99-170">File system</span></span>

<span data-ttu-id="52b99-171">操作系统根据文件夹位置的不同，支持对打包桌面应用程序执行不同级别的文件系统操作。</span><span class="sxs-lookup"><span data-stu-id="52b99-171">The OS supports different levels of file system operations for packaged desktop applications, depending on the folder location.</span></span>

<span data-ttu-id="52b99-172">尝试访问用户的 AppData 文件夹时，系统会在后台创建一个专用的每用户、每应用位置。</span><span class="sxs-lookup"><span data-stu-id="52b99-172">When trying to access the user's *AppData* folder, the system creates a private per-user, per-app location behind the scenes.</span></span> <span data-ttu-id="52b99-173">这会造成打包应用程序正在编辑真正的 AppData 的假象，而实际上它是在修改一个专用副本。</span><span class="sxs-lookup"><span data-stu-id="52b99-173">This creates the illusion that the packaged application is editing the real *AppData* when it's actually modifying a private copy.</span></span> <span data-ttu-id="52b99-174">通过以这种方式重定向写入，系统可以跟踪应用所作的所有文件修改。</span><span class="sxs-lookup"><span data-stu-id="52b99-174">By redirecting writes this way, the system can track all file modifications made by the app.</span></span> <span data-ttu-id="52b99-175">这样，它就可以在卸载时清理所有这些文件，从而减少系统“rot”，为用户提供更好的应用程序删除体验。</span><span class="sxs-lookup"><span data-stu-id="52b99-175">It can then clean all those files when uninstalling reducing system "rot" and providing a better application removal experience for the user.</span></span>

##### <a name="registry"></a><span data-ttu-id="52b99-176">注册表</span><span class="sxs-lookup"><span data-stu-id="52b99-176">Registry</span></span>

<span data-ttu-id="52b99-177">应用包包含一个 registry.dat 文件，它在逻辑上相当于实际注册表中的 `HKLM\Software`。</span><span class="sxs-lookup"><span data-stu-id="52b99-177">App packages contain a registry.dat file, which serves as the logical equivalent of `HKLM\Software` in the real registry.</span></span> <span data-ttu-id="52b99-178">在运行时，此虚拟注册表将此配置单元的内容合并到原生系统配置单元，以提供两者的单一视图。</span><span class="sxs-lookup"><span data-stu-id="52b99-178">At runtime, this virtual registry merges the contents of this hive into the native system hive to provide a singular view of both.</span></span>

<span data-ttu-id="52b99-179">所有写入都会在包升级过程中得到保留，只有在卸载应用程序时才会删除。</span><span class="sxs-lookup"><span data-stu-id="52b99-179">All writes are kept during package upgrade and only deleted when the application is uninstalled.</span></span>

##### <a name="uninstallation"></a><span data-ttu-id="52b99-180">卸载</span><span class="sxs-lookup"><span data-stu-id="52b99-180">Uninstallation</span></span>

<span data-ttu-id="52b99-181">当用户卸载包时，位于 `C:\Program Files\WindowsApps\package_name` 下的所有文件和文件夹都会被删除，在数据写入过程中捕获的对 AppData 或注册表的所有重定向写入也会被删除。</span><span class="sxs-lookup"><span data-stu-id="52b99-181">When the user uninstalls a package, all files and folders located under `C:\Program Files\WindowsApps\package_name` are removed, as well as any redirected writes to AppData or the registry that were captured during the process.</span></span>

<span data-ttu-id="52b99-182">有关打包应用程序如何处理安装、文件访问、注册表和卸载的详细信息，请参阅 <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-behind-the-scenes>。</span><span class="sxs-lookup"><span data-stu-id="52b99-182">For details about how a packaged application handles installation, file access, registry, and uninstallation, see <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-behind-the-scenes>.</span></span>

<span data-ttu-id="52b99-183">你可以在 <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-prepare> 上获取待查事项的完整列表。</span><span class="sxs-lookup"><span data-stu-id="52b99-183">You can get a complete list of things to check on <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-prepare>.</span></span>

## <a name="how-to-add-winrt-apis-to-your-desktop-project"></a><span data-ttu-id="52b99-184">如何将 WinRT API 添加到桌面项目</span><span class="sxs-lookup"><span data-stu-id="52b99-184">How to add WinRT APIs to your desktop project</span></span>

<span data-ttu-id="52b99-185">在本部分中，你可以找到有关如何在现有 WPF 应用程序中集成 Toast 通知的演练。</span><span class="sxs-lookup"><span data-stu-id="52b99-185">In this section, you can find a walkthrough on how to integrate Toast Notifications in an existing WPF application.</span></span> <span data-ttu-id="52b99-186">虽然从代码的角度来看很简单，但它有助于阐释整个过程。</span><span class="sxs-lookup"><span data-stu-id="52b99-186">Although it's simple from the code perspective, it helps illustrate the whole process.</span></span> <span data-ttu-id="52b99-187">通知是众多可用 WinRT API 之一，可用于 .NET 应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-187">Notifications are one of the many available WinRT APIs available that you can use in .NET app.</span></span> <span data-ttu-id="52b99-188">在本例中，该 API 需要一个包标识。</span><span class="sxs-lookup"><span data-stu-id="52b99-188">In this case, the API requires a Package Identity.</span></span> <span data-ttu-id="52b99-189">如果 API 不需要包标识，此过程会更简单。</span><span class="sxs-lookup"><span data-stu-id="52b99-189">This process is more straightforward if the APIs don't require Package Identity.</span></span>

<span data-ttu-id="52b99-190">让我们以一个现有的 WPF 示例应用为例，该应用可读取文件并在屏幕上显示其内容。</span><span class="sxs-lookup"><span data-stu-id="52b99-190">Let's take an existing WPF sample app that reads files and shows its contents on the screen.</span></span> <span data-ttu-id="52b99-191">我们的目标是在应用程序启动时显示 Toast 通知。</span><span class="sxs-lookup"><span data-stu-id="52b99-191">The goal is to display a Toast Notification when the application starts.</span></span>

![示例应用程序运行的屏幕截图](./media/windows-migration/sample-application.png)

<span data-ttu-id="52b99-193">首先，你应该在下面的链接中检查你将使用的 Windows 10 API 是否需要包标识：</span><span class="sxs-lookup"><span data-stu-id="52b99-193">First, you should check in the following link whether the Windows 10 API that you'll use requires a Package Identity:</span></span>

<https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-supported-api>

<span data-ttu-id="52b99-194">我们的示例将使用需要打包标识的 <xref:Windows.UI.Notifications.Notification?displayProperty=nameWithType> API：</span><span class="sxs-lookup"><span data-stu-id="52b99-194">Our sample will use the <xref:Windows.UI.Notifications.Notification?displayProperty=nameWithType> API that requires a packaged identity:</span></span>

![Microsoft 文档中的 Notification 类](./media/windows-migration/notification-class-documentation.png)

<span data-ttu-id="52b99-196">若要访问该 WinRT API，请添加对 `Microsoft.Windows.SDK.Contracts` NuGet 包的引用，此包将在后台发挥神奇作用（详见 <https://blogs.windows.com/windowsdeveloper/2019/04/30/calling-windows-10-apis-from-a-desktop-application-just-got-easier/>）。</span><span class="sxs-lookup"><span data-stu-id="52b99-196">To access the WinRT API, add a reference to the `Microsoft.Windows.SDK.Contracts` NuGet package and this package will do the magic behind the scenes (see details at <https://blogs.windows.com/windowsdeveloper/2019/04/30/calling-windows-10-apis-from-a-desktop-application-just-got-easier/>).</span></span>

<span data-ttu-id="52b99-197">现在可以开始添加一些代码。</span><span class="sxs-lookup"><span data-stu-id="52b99-197">You're now prepared to start adding some code.</span></span>

<span data-ttu-id="52b99-198">创建将在应用程序启动时调用的 `ShowToastNotification` 方法。</span><span class="sxs-lookup"><span data-stu-id="52b99-198">Create a `ShowToastNotification` method that will be called on application startup.</span></span> <span data-ttu-id="52b99-199">它将从 XML 模式生成一个 toast 通知：</span><span class="sxs-lookup"><span data-stu-id="52b99-199">It just builds a toast notification from an XML pattern:</span></span>

```csharp
private void ShowNotification(string title, string content, string image)
{
    string xmlString = $@"<toast><visual><binding template = 'ToastGeneric'><text>{title}</text><text>{content}</text><image src=>'{image}'</image></binding></visual></toast>";
    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);
    ToastNotification toast = new ToastNotification(toastXml);
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

<span data-ttu-id="52b99-200">虽然成功生成项目，但出现了错误，因为通知 API 需要包标识，而你并未提供。</span><span class="sxs-lookup"><span data-stu-id="52b99-200">Although the project builds, there are errors because the Notifications API requires a Package Identity and you didn't provide it.</span></span> <span data-ttu-id="52b99-201">向解决方案添加 Windows 打包项目可以解决此问题：</span><span class="sxs-lookup"><span data-stu-id="52b99-201">Adding a Windows Packaging Project to the solution will fix the issue:</span></span>

![Visual Studio 中的“添加新项目”对话框的屏幕截图](./media/windows-migration/add-packaging-project.png)

<span data-ttu-id="52b99-203">选择你要支持的最低 Windows 版本和目标版本。</span><span class="sxs-lookup"><span data-stu-id="52b99-203">Select the minimum Windows version you want to support and the version you're targeting.</span></span> <span data-ttu-id="52b99-204">并非所有 Windows 10 版本都支持所有 WinRT API。</span><span class="sxs-lookup"><span data-stu-id="52b99-204">Not all the WinRT APIs are supported in all Windows 10 versions.</span></span> <span data-ttu-id="52b99-205">每个 Windows 10 更新都会添加新的 API，这些 API 只能从这个版本中获得；下级支持不可用。</span><span class="sxs-lookup"><span data-stu-id="52b99-205">Each Windows 10 update adds new APIs that are only available from this version; down-level support isn't available.</span></span>

![选择最低 Windows 版本](./media/windows-migration/select-versions.png)

<span data-ttu-id="52b99-207">下一步是通过添加项目引用，将 WPF 应用程序添加到 Windows 打包项目中：</span><span class="sxs-lookup"><span data-stu-id="52b99-207">Next step is to add the WPF application to the Windows Packaging Project by adding a project reference:</span></span>

![将 WPF 应用程序添加到 Windows 打包项目中](./media/windows-migration/add-application.png)

![引用管理器](./media/windows-migration/reference-manager.png)

<span data-ttu-id="52b99-210">一个 Windows 打包项目可以打包多个应用，因此应设置将哪个应用作为入口点：</span><span class="sxs-lookup"><span data-stu-id="52b99-210">A Windows Packaging Project can package several apps so you should set which one is the Entry Point:</span></span>

![设置入口点](./media/windows-migration/set-entry-point.png)

<span data-ttu-id="52b99-212">下一步是在解决方案配置中将 WPF 项目设置为启动项目。</span><span class="sxs-lookup"><span data-stu-id="52b99-212">Next step is to set the WPF Project as the startup Project in the solution configuration.</span></span> <span data-ttu-id="52b99-213">可以按 F5 键进行编译和生成，并查看结果。</span><span class="sxs-lookup"><span data-stu-id="52b99-213">You can press F5 to compile and build and see the results.</span></span>

![示例应用程序运行并显示结果](./media/windows-migration/sample-app-result.png)

<span data-ttu-id="52b99-215">让我们生成包，以便安装应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-215">Let's generate the package so you can install your app.</span></span> <span data-ttu-id="52b99-216">右键单击“Store” > “创建应用包”。</span><span class="sxs-lookup"><span data-stu-id="52b99-216">Right click on **Store** > **Create App Packages**.</span></span>

![“创建应用包”对话框](./media/windows-migration/create-app-packages.png)

<span data-ttu-id="52b99-218">选择旁加载选项以从计算机部署应用：</span><span class="sxs-lookup"><span data-stu-id="52b99-218">Select the sideloading option to deploy the app from your machine:</span></span>

![选择旁加载选项](./media/windows-migration/select-sideloading-option.png)

<span data-ttu-id="52b99-220">选择应用的应用程序体系结构：</span><span class="sxs-lookup"><span data-stu-id="52b99-220">Select the application architecture of your app:</span></span>

![选择应用程序体系结构](./media/windows-migration/select-app-architecture.png)

<span data-ttu-id="52b99-222">最后，单击“创建”创建包。</span><span class="sxs-lookup"><span data-stu-id="52b99-222">Finally, create the package by clicking on **Create**.</span></span>

## <a name="xaml-islands"></a><span data-ttu-id="52b99-223">XAML 岛</span><span class="sxs-lookup"><span data-stu-id="52b99-223">XAML Islands</span></span>

<span data-ttu-id="52b99-224">XAML 岛是一组组件，它们使 Windows 桌面开发人员能够在现有 Win32 应用程序（包括 Windows 窗体和 WPF）上使用 UWP XAML 控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-224">XAML Islands are a set of components that enable Windows desktop developers to use UWP XAML controls on their existing Win32 applications, including Windows Forms and WPF.</span></span>

![XAML 岛的结构](./media/windows-migration/xaml-islands.png)

<span data-ttu-id="52b99-226">设想一下，你的 Win32 应用上有许多标准控件，其中有一个 UWP UI“岛”，包含现代世界中的控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-226">You can image your Win32 app with your standard controls and among them an "island" of UWP UI containing controls from the modern world.</span></span> <span data-ttu-id="52b99-227">这个概念类似于网页中有一个 iFrame，用于显示来自 `different page.` 的内容。</span><span class="sxs-lookup"><span data-stu-id="52b99-227">The concept is similar as having an iFrame inside a web page that shows content from a `different page.`</span></span>

<span data-ttu-id="52b99-228">除了从 Windows 10 API 添加功能外，你还可以使用 XAML 岛在应用内添加 UWP XAML 片段。</span><span class="sxs-lookup"><span data-stu-id="52b99-228">Besides adding functionality from the Windows 10 APIs, you can add pieces of UWP XAML inside of your app using XAML Islands.</span></span>

<span data-ttu-id="52b99-229">Windows 10 1903 更新引入了一组 API，可用于在 Win32 窗口中托管 UWP XAML 内容。</span><span class="sxs-lookup"><span data-stu-id="52b99-229">Windows 10 1903 update introduces a set of APIs that allows hosting UWP XAML content in Win32 windows.</span></span> <span data-ttu-id="52b99-230">只有在 Windows 10 1903 上运行的应用才能使用 XAML 岛。</span><span class="sxs-lookup"><span data-stu-id="52b99-230">Only apps running on Windows 10 1903 can use XAML Islands.</span></span>

### <a name="the-road-to-xaml-islands"></a><span data-ttu-id="52b99-231">通往 XAML 岛之路</span><span class="sxs-lookup"><span data-stu-id="52b99-231">The road to XAML Islands</span></span>

<span data-ttu-id="52b99-232">通往 XAML 岛之路始于 2012 年，当时 Microsoft 引入 WinRT API 作为框架，以实现 Win32 应用（Windows 窗体、WPF 和本机 Win32 应用）的现代化。</span><span class="sxs-lookup"><span data-stu-id="52b99-232">The road to XAML Islands started in 2012 when Microsoft introduced the WinRT APIs as a framework to modernize the Win32 apps (Windows Forms, WPF, and native Win32 apps).</span></span> <span data-ttu-id="52b99-233">但是，WinRT 中的新 UI 控件可用于新应用程序，但不能用于现有应用程序。</span><span class="sxs-lookup"><span data-stu-id="52b99-233">However, the new UI controls inside WinRT were available for new applications but not for existing ones.</span></span>

<span data-ttu-id="52b99-234">2015 年，伴随 Windows 10 诞生的还有 UWP。</span><span class="sxs-lookup"><span data-stu-id="52b99-234">In 2015, along with Windows 10, UWP was born.</span></span> <span data-ttu-id="52b99-235">UWP 允许创建跨 Windows 设备（如 XBox、移动和桌面）工作的应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-235">UWP allows you to create apps that work across Windows devices like XBox, Mobile, and Desktop.</span></span> <span data-ttu-id="52b99-236">一年后，Microsoft 推出了 Desktop Bridge（原名 Project Centennial）。</span><span class="sxs-lookup"><span data-stu-id="52b99-236">One year later, Microsoft announced Desktop Bridge (formerly known as Project Centennial).</span></span> <span data-ttu-id="52b99-237">Desktop Bridge 是让开发人员能够将现有 Win32 应用引入 Microsoft Store 的一组工具。</span><span class="sxs-lookup"><span data-stu-id="52b99-237">Desktop Bridge is a set of tools that allowed developers to bring their existing Win32 apps to the Microsoft Store.</span></span> <span data-ttu-id="52b99-238">它在 2017 年添加了更多功能，使开发人员能够利用一些新的 Windows 10 API（例如实时磁贴和操作中心通知）来增强 Win32 应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-238">More capabilities were added in 2017, allowing developers to enhance their Win32 apps leveraging some of the new Windows 10 APIs, like live tiles and notifications on the action center.</span></span> <span data-ttu-id="52b99-239">但是，还是没有新的 UI 控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-239">But still, no new UI controls.</span></span>

<span data-ttu-id="52b99-240">在 Build 2018 大会上，Microsoft 宣布了一种方法，让开发人员能够在当前的 Win32 应用中使用新的 Windows 10 XAML 控件，而无需将应用完全迁移到 UWP。</span><span class="sxs-lookup"><span data-stu-id="52b99-240">At Build 2018, Microsoft announced a way for developers to use the new Windows 10 XAML controls into their current Win32 apps, without fully migrating their apps to UWP.</span></span> <span data-ttu-id="52b99-241">这种方法称作 UWP XAML 岛。</span><span class="sxs-lookup"><span data-stu-id="52b99-241">It was branded as UWP XAML Islands.</span></span>

### <a name="how-it-works"></a><span data-ttu-id="52b99-242">工作原理</span><span class="sxs-lookup"><span data-stu-id="52b99-242">How it works</span></span>

<span data-ttu-id="52b99-243">Windows 10 1903 更新引入了几个 XAML 托管 API。</span><span class="sxs-lookup"><span data-stu-id="52b99-243">The Windows 10 1903 update introduces several XAML hosting APIs.</span></span> <span data-ttu-id="52b99-244">其中两个是 `WindowsXamlManager` 和 `DesktopWindowXamlSource`。</span><span class="sxs-lookup"><span data-stu-id="52b99-244">Two of them are `WindowsXamlManager` and `DesktopWindowXamlSource`.</span></span>

<span data-ttu-id="52b99-245">`WindowsXamlManager` 类处理 UWP XAML 框架。</span><span class="sxs-lookup"><span data-stu-id="52b99-245">The `WindowsXamlManager` class handles the UWP XAML Framework.</span></span> <span data-ttu-id="52b99-246">它的 `InitializeForCurrentThread` 方法可在 Win32 应用的当前线程内加载 UWP XAML 框架。</span><span class="sxs-lookup"><span data-stu-id="52b99-246">Its `InitializeForCurrentThread` method loads the UWP XAML Framework inside the current thread of the Win32 app.</span></span>

<span data-ttu-id="52b99-247">`DesktopWindowXamlSource` 是 XAML 岛内容的实例。</span><span class="sxs-lookup"><span data-stu-id="52b99-247">The `DesktopWindowXamlSource` is the instance of your XAML Island content.</span></span> <span data-ttu-id="52b99-248">它具有 `Content` 属性，你负责实例化和设置该属性。</span><span class="sxs-lookup"><span data-stu-id="52b99-248">It has the `Content` property, which you're responsible for instantiating and setting.</span></span> <span data-ttu-id="52b99-249">`DesktopWindowXamlSource` 从 HWND 呈现并获取其输入。</span><span class="sxs-lookup"><span data-stu-id="52b99-249">The `DesktopWindowXamlSource` renders and gets its input from an HWND.</span></span> <span data-ttu-id="52b99-250">它需要知道将 XAML 岛的 HWND 附加到哪个 HWND，你负责调整父 HWND 的大小和位置。</span><span class="sxs-lookup"><span data-stu-id="52b99-250">It needs to know to which other HWND it will attach the XAML Island's one, and you're responsible for sizing and positioning the parent's HWND.</span></span>

<span data-ttu-id="52b99-251">WPF 或 Windows 窗体开发人员通常不会在代码中处理 HWND，因此，可能很难理解和处理 HWND 指针以及用于在 Win32 和 UWP 之间建立通信的底层布线。</span><span class="sxs-lookup"><span data-stu-id="52b99-251">WPF or Windows Forms developers don't usually deal with HWND inside their code, so it may be hard to understand and handle HWND pointers and the underlying wiring stuff to communicate Win32 and UWP worlds.</span></span>

#### <a name="the-xaml-islands-net-wrappers"></a><span data-ttu-id="52b99-252">XAML 岛 .NET 包装器</span><span class="sxs-lookup"><span data-stu-id="52b99-252">The XAML Islands .NET Wrappers</span></span>

<span data-ttu-id="52b99-253">Windows 社区工具包有一套适用于 WPF 或 Windows 窗体的 XAML 岛 .NET 包装器，可以简化 XAML 岛的使用。</span><span class="sxs-lookup"><span data-stu-id="52b99-253">The Windows Community Toolkit has a set the XAML Islands .NET wrappers for WPF or Windows Forms that make easier to use XAML Islands.</span></span> <span data-ttu-id="52b99-254">这些包装器可管理 HWND、焦点管理等。</span><span class="sxs-lookup"><span data-stu-id="52b99-254">These wrappers manage the HWNDs, the focus management, among other things.</span></span> <span data-ttu-id="52b99-255">Windows 窗体和 WPF 开发人员应使用这些包装器。</span><span class="sxs-lookup"><span data-stu-id="52b99-255">Windows Forms and WPF developers should use these wrappers.</span></span>

<span data-ttu-id="52b99-256">Windows 社区工具包提供两种类型的控件：包装控件和托管控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-256">The Windows Community Toolkit offers two types of controls: Wrapped Controls and Hosting Controls.</span></span>

##### <a name="wrapped-controls"></a><span data-ttu-id="52b99-257">包装控件</span><span class="sxs-lookup"><span data-stu-id="52b99-257">Wrapped Controls</span></span>

<span data-ttu-id="52b99-258">这些包装控件将一些 UWP 控件包装到 Windows 窗体或 WPF 控件中，为这些开发人员隐藏 UWP 概念。</span><span class="sxs-lookup"><span data-stu-id="52b99-258">These wrapped controls wrap some UWP controls into Windows Forms or WPF controls, hiding UWP concepts for those developers.</span></span> <span data-ttu-id="52b99-259">这些控件包括：</span><span class="sxs-lookup"><span data-stu-id="52b99-259">These controls are:</span></span>

* <span data-ttu-id="52b99-260">WebView 和 WebViewCompatible</span><span class="sxs-lookup"><span data-stu-id="52b99-260">WebView and WebViewCompatible</span></span>
* <span data-ttu-id="52b99-261">InkCanvas 和 InkToolbar</span><span class="sxs-lookup"><span data-stu-id="52b99-261">InkCanvas and InkToolbar</span></span>
* <span data-ttu-id="52b99-262">MediaPlayerElement</span><span class="sxs-lookup"><span data-stu-id="52b99-262">MediaPlayerElement</span></span>
* <span data-ttu-id="52b99-263">MapControl</span><span class="sxs-lookup"><span data-stu-id="52b99-263">MapControl</span></span>

<span data-ttu-id="52b99-264">将 `Microsoft.Toolkit.Wpf.UI.Controls` 包添加到项目中，添加对命名空间的引用，然后开始使用它们。</span><span class="sxs-lookup"><span data-stu-id="52b99-264">Add the `Microsoft.Toolkit.Wpf.UI.Controls` package to your project, include the reference to the namespace, and start using them.</span></span>

```xml
<Window
        ...
        xmlns:uwpControls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls">
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="\*"/>
    </Grid.RowDefinitions>
    <uwpControls:InkToolbar TargetInkCanvas="{x:Reference Name=inkCanvas}"/>
    <uwpControls:InkCanvas Grid.Row="1" x:Name="inkCanvas" />
</Grid>
```

##### <a name="hosting-controls"></a><span data-ttu-id="52b99-265">托管控件</span><span class="sxs-lookup"><span data-stu-id="52b99-265">Hosting controls</span></span>

<span data-ttu-id="52b99-266">XAML 岛的强大功能延伸到大多数第一方控件、第三方控件以及为 UWP 开发的自定义控件，这些控件可以作为“岛”集成到 Windows 窗体和 WPF 中，并具有功能完备的 UI。</span><span class="sxs-lookup"><span data-stu-id="52b99-266">The power of XAML Islands extends to most first-party controls, third-party controls, and custom controls developed for UWP, which can be integrated into Windows Forms and WPF as "Islands" with fully functional UI.</span></span> <span data-ttu-id="52b99-267">WPF 和 Windows 窗体的 `WindowsXamlHost` 控件允许这样做。</span><span class="sxs-lookup"><span data-stu-id="52b99-267">The `WindowsXamlHost` control for WPF and Windows Forms allows doing this.</span></span>

<span data-ttu-id="52b99-268">例如，若要使用 WPF 中的 `WindowsXamlHost` 控件，请添加对 Windows 社区工具包提供的 `Microsoft.Toolkit.Wpf.UI.XamlHost` 包的引用。</span><span class="sxs-lookup"><span data-stu-id="52b99-268">For example, to use the `WindowsXamlHost` control in WPF, add a reference to the `Microsoft.Toolkit.Wpf.UI.XamlHost` package provided by the Windows Community Toolkit.</span></span>

<span data-ttu-id="52b99-269">一旦将 `WindowsXamlHost` 放入 UI 代码后，就可以指定要加载的 UWP 类型。</span><span class="sxs-lookup"><span data-stu-id="52b99-269">Once you've placed your `WindowsXamlHost` into your UI code, specify which UWP type you want to load.</span></span> <span data-ttu-id="52b99-270">你可以选择使用像 `Button` 这样的包装控件，也可以使用由几个不同控件组成的更复杂的控件，即自定义 UWP 控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-270">You can choose to use a wrapped control like a `Button` or a more complex one composed by several different controls, which are a custom UWP control.</span></span>

<span data-ttu-id="52b99-271">以下示例演示如何添加 UWP `Button`：</span><span class="sxs-lookup"><span data-stu-id="52b99-271">The following example shows how to add a UWP `Button`:</span></span>

```xml
<Window
        ...
        xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost">

<xamlhost:WindowsXamlHost x:Name="myUwpButton"
                          InitialTypeName="Windows.UI.Xaml.Controls.Button" />
```

<span data-ttu-id="52b99-272">对于如何实现此操作，有一个明确的建议：最好是使用一个包含自定义复合控件的较大的 XAML 岛，而不是使用几个岛，每个岛上一个控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-272">There's a clear recommendation on how to approach this and it's better to have one single and bigger XAML Island containing a custom composite control than to have several islands with one control on each.</span></span>

<span data-ttu-id="52b99-273">XAML 的核心功能之一是绑定，它适用于 Win32 代码和岛。</span><span class="sxs-lookup"><span data-stu-id="52b99-273">One of the core features of XAML is binding and it works between your Win32 code and the island.</span></span> <span data-ttu-id="52b99-274">例如，你可以将 Win32 `Textbox` 与 UWP `Textbox` 绑定。</span><span class="sxs-lookup"><span data-stu-id="52b99-274">So, you can bind, for instance, a Win32 `Textbox` with a UWP `Textbox`.</span></span> <span data-ttu-id="52b99-275">你需要考虑的一个重要问题是，这些绑定是从 UWP 到 Win32 的单向绑定，因此，如果更新 XAML 岛中的 `Textbox`，Win32 Textbox 也会更新，反过来却不会。</span><span class="sxs-lookup"><span data-stu-id="52b99-275">One important thing to consider is that these bindings are one-way bindings, from UWP to Win32, so if you update the `Textbox` inside the XAML Island the Win32 Textbox will be updated, but not the other way around.</span></span>

<span data-ttu-id="52b99-276">若要查看有关如何使用 XAML 岛的演练，请参阅：</span><span class="sxs-lookup"><span data-stu-id="52b99-276">To see a walkthrough about how to use XAML Islands, see:</span></span>

<https://docs.microsoft.com/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands>

#### <a name="adding-uwp-xaml-custom-controls"></a><span data-ttu-id="52b99-277">添加 UWP XAML 自定义控件</span><span class="sxs-lookup"><span data-stu-id="52b99-277">Adding UWP XAML custom controls</span></span>

<span data-ttu-id="52b99-278">XAML 自定义控件是由你或第三方创建的控件（或用户控件），其中包括 WinUI 2.x 控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-278">A XAML custom control is a control (or user control) created by you or by third parties (including WinUI 2.x controls).</span></span> <span data-ttu-id="52b99-279">若要在 Windows 窗体或 WPF 应用中托管自定义 UWP 控件，你需要：</span><span class="sxs-lookup"><span data-stu-id="52b99-279">To host a custom UWP control in a Windows Forms or WPF app, you'll need:</span></span>

- <span data-ttu-id="52b99-280">在 .NET 应用中使用 `WindowsXamlHost` UWP 控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-280">To use the `WindowsXamlHost` UWP control in your .NET app.</span></span>
- <span data-ttu-id="52b99-281">创建一个 UWP 应用项目来定义 `XamlApplication` 对象。</span><span class="sxs-lookup"><span data-stu-id="52b99-281">To create a UWP app project that defines a `XamlApplication` object.</span></span>

<span data-ttu-id="52b99-282">WPF 或 Windows 窗体项目必须有权访问 Windows 社区工具包提供的 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 类的实例。</span><span class="sxs-lookup"><span data-stu-id="52b99-282">Your WPF or Windows Forms project must have access to an instance of the `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` class provided by the Windows Community Toolkit.</span></span> <span data-ttu-id="52b99-283">此对象充当根元数据提供程序，用于为应用程序当前目录中的程序集中的自定义 UWP XAML 类型加载元数据。</span><span class="sxs-lookup"><span data-stu-id="52b99-283">This object acts as a root metadata provider for loading metadata for custom UWP XAML types in assemblies in the current directory of your application.</span></span> <span data-ttu-id="52b99-284">若要实现此目的，建议的方法是向 WPF 或 Windows 窗体项目所在的解决方案添加一个空白应用（通用 Windows）项目，并修改此项目中的默认 App 类。</span><span class="sxs-lookup"><span data-stu-id="52b99-284">The recommended way to do this is to add a Blank App (Universal Windows) project to the same solution as your WPF or Windows Forms project and revise the default App class in this project.</span></span>

<span data-ttu-id="52b99-285">自定义 UWP XAML 控件可以包含在此 UWP 应用中，也可以包含在独立的 UWP 类库项目中，你可以在 WPF 或 Windows 窗体项目所在的解决方案中引用该项目。</span><span class="sxs-lookup"><span data-stu-id="52b99-285">The custom UWP XAML control can be included on this UWP app or in an independent UWP Class Library project that you reference in the same solution as your WPF or Windows Forms project.</span></span>

<span data-ttu-id="52b99-286">你可以在以下位置查看详细的分步过程说明：</span><span class="sxs-lookup"><span data-stu-id="52b99-286">You can check a detailed step-by-step process description at:</span></span>

<https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands>

#### <a name="the-windows-ui-library-winui-2"></a><span data-ttu-id="52b99-287">Windows UI 库 (WinUI 2)</span><span class="sxs-lookup"><span data-stu-id="52b99-287">The Windows UI Library (WinUI 2)</span></span>

<span data-ttu-id="52b99-288">除了操作系统自带的收件箱 Windows 10 控件外，同一 UWP XAML 团队还在 Windows UI 库 (WinUI 2) 中提供了其他控件。</span><span class="sxs-lookup"><span data-stu-id="52b99-288">Besides the inbox Windows 10 controls that comes with the OS, the same UWP XAML team also deliver additional controls in the Windows UI Library (**WinUI 2**).</span></span> <span data-ttu-id="52b99-289">WinUI 2 为 Windows UWP 应用提供官方的本机 Microsoft UI 控件和功能，这些控件可以在 XAML 岛内使用。</span><span class="sxs-lookup"><span data-stu-id="52b99-289">WinUI 2 provides official native Microsoft UI controls and features for Windows UWP apps and these controls can be used inside of XAML Islands.</span></span>

<span data-ttu-id="52b99-290">WinUI 2 是一个开源项目，你可以在 <https://github.com/microsoft/microsoft-ui-xaml> 中找到相关信息。</span><span class="sxs-lookup"><span data-stu-id="52b99-290">WinUI 2 is open source and you can find information at <https://github.com/microsoft/microsoft-ui-xaml>.</span></span>

<span data-ttu-id="52b99-291">下面的文章演示了如何托管 WinUI 2 库中的 UWP XAML 控件：<https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands></span><span class="sxs-lookup"><span data-stu-id="52b99-291">The following article demonstrates how to host a UWP XAML control from the WinUI 2 library: <https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands></span></span>

### <a name="do-you-need-xaml-islands"></a><span data-ttu-id="52b99-292">是否需要 XAML 岛</span><span class="sxs-lookup"><span data-stu-id="52b99-292">Do you need XAML Islands</span></span>

<span data-ttu-id="52b99-293">XAML 岛适用于现有的 Win32 应用，这些应用希望利用新的 UWP 控件和行为来改善用户体验，而无需完全重写应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-293">XAML Islands are intended for existing Win32 apps that want to improve their user experience by leveraging new UWP controls and behaviors without a full rewrite of the app.</span></span> <span data-ttu-id="52b99-294">你已经可以[使用 Windows 10 API](/windows/uwp/porting/desktop-to-uwp-enhance)，但在 XAML 岛出现之前，你只能使用非 UI 相关的 API。</span><span class="sxs-lookup"><span data-stu-id="52b99-294">You could already [leverage Windows 10 APIs](/windows/uwp/porting/desktop-to-uwp-enhance), but up until XAML Islands, only non-UI related APIs.</span></span>

<span data-ttu-id="52b99-295">如果要开发新的 Windows 应用，[UWP 应用](/windows/uwp/get-started/universal-application-platform-guide)可能是正确的方法。</span><span class="sxs-lookup"><span data-stu-id="52b99-295">If you're developing a new Windows App, a [UWP App](/windows/uwp/get-started/universal-application-platform-guide) is probably the right approach.</span></span>

### <a name="the-road-ahead-xaml-islands-winui-30"></a><span data-ttu-id="52b99-296">通往 XAML 岛之路：WinUI 3.0</span><span class="sxs-lookup"><span data-stu-id="52b99-296">The road ahead XAML Islands: WinUI 3.0</span></span>

<span data-ttu-id="52b99-297">自 Windows 8 以来，Windows UI 平台（包括 XAML UI 框架、视觉对象组合层和输入处理）已作为 Windows 不可或缺的一部分提供。</span><span class="sxs-lookup"><span data-stu-id="52b99-297">Since Windows 8, the Windows UI platform, including the XAML UI framework, visual composition layer, and input processing has been shipped as an integral part of Windows.</span></span> <span data-ttu-id="52b99-298">这意味着，若要从 UI 技术的最新改进中获益，就必须升级到 UI 的最新版本，这样会放慢应用开发的创新步伐。</span><span class="sxs-lookup"><span data-stu-id="52b99-298">This means that to benefit from the latest improvements on UI technologies, you must upgrade to the latest version of the UI, slowing down the pace of innovation when you develop your apps.</span></span> <span data-ttu-id="52b99-299">为了分离这两个发展周期，促进创新，Microsoft 正在积极开发 WinUI 项目。</span><span class="sxs-lookup"><span data-stu-id="52b99-299">To decouple these two evolution cycles and foster innovation, Microsoft is actively working on the WinUI project.</span></span>

<span data-ttu-id="52b99-300">从 2018 年的 WinUI 2 开始，Microsoft 开始将一些新的 XAML UI 控件和功能作为构建在 UWP SDK 之上的独立 NuGet 包发布。</span><span class="sxs-lookup"><span data-stu-id="52b99-300">Starting with WinUI 2 in 2018, Microsoft started shipping some new XAML UI controls and features as separate NuGet packages that build on top of the UWP SDK.</span></span>

![WinUI 2.0 的结构](./media/windows-migration/winui2.png)

<span data-ttu-id="52b99-302">WinUI 3 正在积极开发中，它将大大扩展 WinUI 的范围，以包括完整的 UI 平台（将与 UWP SDK 完全分离）：</span><span class="sxs-lookup"><span data-stu-id="52b99-302">WinUI 3 is under active development and will greatly expand the scope of WinUI to include the full UI platform, which will be fully decoupled from the UWP SDK:</span></span>

![WinUI 3.0 的结构](./media/windows-migration/winui3.png)

<span data-ttu-id="52b99-304">XAML 框架现在将在 GitHub 上开发，并以 [NuGet](/nuget/what-is-nuget) 包的形式进行带外发布。</span><span class="sxs-lookup"><span data-stu-id="52b99-304">XAML framework will now be developed on GitHub and shipped out of band as [NuGet](/nuget/what-is-nuget) packages.</span></span>

<span data-ttu-id="52b99-305">作为 OS 的一部分发布的现有 UWP XAML API 将不会再收到新的功能更新。</span><span class="sxs-lookup"><span data-stu-id="52b99-305">The existing UWP XAML APIs that ship as part of the OS will no longer receive new feature updates.</span></span> <span data-ttu-id="52b99-306">它们会在 Windows 10 支持生命周期内继续收到安全更新程序和关键修复程序。</span><span class="sxs-lookup"><span data-stu-id="52b99-306">They will still receive security updates and critical fixes according to the Windows 10 support lifecycle.</span></span>

<span data-ttu-id="52b99-307">通用 Windows 平台不仅包含 XAML 框架，还包含其他功能（例如应用程序和安全模型、媒体管道、Xbox 和 Windows 10 shell 集成、广泛的设备支持），并且还将继续发展。</span><span class="sxs-lookup"><span data-stu-id="52b99-307">The Universal Windows Platform contains more than just the XAML framework (for example, application and security model, media pipeline, Xbox and Windows 10 shell integrations, broad device support) and will continue to evolve.</span></span> <span data-ttu-id="52b99-308">所有新的 XAML 功能都将作为 WinUI 的一部分进行开发和交付。</span><span class="sxs-lookup"><span data-stu-id="52b99-308">All new XAML features will just be developed and ship as part of WinUI instead.</span></span>

#### <a name="winui-3-in-desktop-app-and-winui-xaml-islands"></a><span data-ttu-id="52b99-309">桌面应用中的 WinUI 3 和 WinUI XAML 岛</span><span class="sxs-lookup"><span data-stu-id="52b99-309">WinUI 3 in desktop app and WinUI XAML Islands</span></span>

<span data-ttu-id="52b99-310">如你所见，WinUI 3 是由 UWP XAML 演变而来，它可以在 UWP 应用模型及其所有必备组件（MSIX 打包 ID、沙盒、CoreWindow 等）中自然运行。</span><span class="sxs-lookup"><span data-stu-id="52b99-310">As you can see, WinUI 3 is the evolution of UWP XAML and it works naturally within the UWP app model and all its requirements (MSIX packaged ID, sandbox, CoreWindow, and so on.</span></span> <span data-ttu-id="52b99-311">若要在 Win32 应用模型中仅使用 WinUI 3，应由另一个 UI 框架（Windows 窗体、WPF 等）使用 WinUI XAML 岛托管 WinUI 内容。</span><span class="sxs-lookup"><span data-stu-id="52b99-311">To use just WinUI 3 in a Win32 app model, the WinUI content should be hosted by another UI Framework (Windows Forms, WPF, and so on) using **WinUI XAML Islands**.</span></span> <span data-ttu-id="52b99-312">若要改进应用并实现技术混用，这是正确的做法。</span><span class="sxs-lookup"><span data-stu-id="52b99-312">This is the right path if you want to evolve your app and mix technologies.</span></span> <span data-ttu-id="52b99-313">但是，若要为 WinUI 替换掉整个旧 UI，应用不应加载仅用于托管 WinUI 的 UI 框架。</span><span class="sxs-lookup"><span data-stu-id="52b99-313">However, if you want to replace your entire old UI for WinUI, your app shouldn't load UI Frameworks for just hosting WinUI.</span></span>

<span data-ttu-id="52b99-314">WinUI 3 将解决此关键反馈，在桌面应用中添加 WinUI。</span><span class="sxs-lookup"><span data-stu-id="52b99-314">WinUI 3 will address this critical feedback adding **WinUI in desktop apps**.</span></span> <span data-ttu-id="52b99-315">这将允许 Win32 应用使用 WinUI 3 作为独立的 UI 框架；无需加载 Windows 窗体或 WPF。</span><span class="sxs-lookup"><span data-stu-id="52b99-315">This will allow that Win32 apps can use WinUI 3 as standalone UI Framework; no need to load Windows Forms or WPF.</span></span>

<span data-ttu-id="52b99-316">在此聚合内，WinUI 3 将让开发人员轻松混搭出合适的组合：</span><span class="sxs-lookup"><span data-stu-id="52b99-316">Within this aggregation, WinUI 3 will let developers easily mix and match the right combination of:</span></span>

* <span data-ttu-id="52b99-317">应用模型：UWP、Win32</span><span class="sxs-lookup"><span data-stu-id="52b99-317">App model: UWP, Win32</span></span>
* <span data-ttu-id="52b99-318">平台：.NET 或本机</span><span class="sxs-lookup"><span data-stu-id="52b99-318">Platform: .NET or Native</span></span>
* <span data-ttu-id="52b99-319">语言：.NET（C\#、Visual Basic）、标准 C++</span><span class="sxs-lookup"><span data-stu-id="52b99-319">Language: .NET (C\#, Visual Basic), standard C++</span></span>
* <span data-ttu-id="52b99-320">打包：MSIX、用于 Microsoft Store 的 AppX、未打包</span><span class="sxs-lookup"><span data-stu-id="52b99-320">Packaging: MSIX, AppX for the Microsoft Store, unpackaged</span></span>
* <span data-ttu-id="52b99-321">互操作：使用 WinUI 3 通过 WinUI XAML 岛扩展现有 WPF、WinForms 和 MFC 应用。</span><span class="sxs-lookup"><span data-stu-id="52b99-321">Interop: use WinUI 3 to extend existing WPF, WinForms, and MFC apps using WinUI XAML Islands.</span></span>

<span data-ttu-id="52b99-322">若要了解更多详细信息，Microsoft 正在 <https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md> 中分享此路线图。</span><span class="sxs-lookup"><span data-stu-id="52b99-322">If you want to know more details, Microsoft is sharing this roadmap in <https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md>.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="52b99-323">[上一页](migrate-modern-applications.md)
>[下一页](example-migration.md)</span><span class="sxs-lookup"><span data-stu-id="52b99-323">[Previous](migrate-modern-applications.md)
[Next](example-migration.md)</span></span>
