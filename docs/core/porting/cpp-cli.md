---
title: 将 C++/CLI 项目迁移到 .NET Core
description: 了解如何将 C++/CLI 项目移植到 .NET Core。
author: mjrousos
ms.date: 01/10/2020
ms.openlocfilehash: 60c4cc2040b5cfb7aadc2147832d3f0a763085dc
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514391"
---
# <a name="how-to-port-a-ccli-project-to-net-core"></a><span data-ttu-id="c911a-103">如何将 C++/CLI 项目移植到 .NET Core</span><span class="sxs-lookup"><span data-stu-id="c911a-103">How to port a C++/CLI project to .NET Core</span></span>

<span data-ttu-id="c911a-104">从 .NET Core 3.1 和 Visual Studio 2019 16.4 版开始，[C++/CLI 项目](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp)可面向 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="c911a-104">Beginning with .NET Core 3.1 and Visual Studio 2019 version 16.4, [C++/CLI projects](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) can target .NET Core.</span></span> <span data-ttu-id="c911a-105">这种支持使你能够将具有 C++/CLI 互操作层的 Windows 桌面应用程序移植到 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="c911a-105">This support makes it possible to port Windows desktop applications with C++/CLI interop layers to .NET Core.</span></span> <span data-ttu-id="c911a-106">本文介绍如何将 C++/CLI 项目从 .NET Framework 移植到 .NET Core 3.1。</span><span class="sxs-lookup"><span data-stu-id="c911a-106">This article describes how to port C++/CLI projects from .NET Framework to .NET Core 3.1.</span></span>

## <a name="ccli-net-core-limitations"></a><span data-ttu-id="c911a-107">C++/CLI .NET Core 限制</span><span class="sxs-lookup"><span data-stu-id="c911a-107">C++/CLI .NET Core limitations</span></span>

<span data-ttu-id="c911a-108">与其他语言相比，将 C++/CLI 项目移植到 .NET Core 具有一些重要限制：</span><span class="sxs-lookup"><span data-stu-id="c911a-108">There are some important limitations to porting C++/CLI projects to .NET Core compared to other languages:</span></span>

* <span data-ttu-id="c911a-109">.NET Core 的 C++/CLI 支持仅适用于 Windows。</span><span class="sxs-lookup"><span data-stu-id="c911a-109">C++/CLI support for .NET Core is Windows only.</span></span>
* <span data-ttu-id="c911a-110">C++/CLI 项目不能面向 .NET Standard，只能面向 .NET Core（或 .NET Framework）。</span><span class="sxs-lookup"><span data-stu-id="c911a-110">C++/CLI projects can't target .NET Standard, only .NET Core (or .NET Framework).</span></span>
* <span data-ttu-id="c911a-111">C++/CLI 项目不支持新的 SDK 样式项目文件格式。</span><span class="sxs-lookup"><span data-stu-id="c911a-111">C++/CLI projects don't support the new SDK-style project file format.</span></span> <span data-ttu-id="c911a-112">相反，即使在面向 .NET Core 时，C++/CLI 项目也使用现有的 vcxproj 文件格式。</span><span class="sxs-lookup"><span data-stu-id="c911a-112">Instead, even when targeting .NET Core, C++/CLI projects use the existing vcxproj file format.</span></span>
* <span data-ttu-id="c911a-113">C++/CLI 项目不能同时面向多个 .NET 平台。</span><span class="sxs-lookup"><span data-stu-id="c911a-113">C++/CLI projects can't multitarget multiple .NET platforms.</span></span> <span data-ttu-id="c911a-114">如果需要同时为 .NET Framework 和 .NET Core 生成 C++/CLI 项目，请使用单独的项目文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-114">If you need to build a C++/CLI project for both .NET Framework and .NET Core, use separate project files.</span></span>
* <span data-ttu-id="c911a-115">.NET Core 不支持 `-clr:pure` 或 `-clr:safe` 编译，仅支持新的 `-clr:netcore` 选项（等效于 .NET Framework 的 `-clr`）。</span><span class="sxs-lookup"><span data-stu-id="c911a-115">.NET Core doesn't support `-clr:pure` or `-clr:safe` compilation, only the new `-clr:netcore` option (which is equivalent to `-clr` for .NET Framework).</span></span>

## <a name="port-a-ccli-project"></a><span data-ttu-id="c911a-116">移植 C++/CLI 项目</span><span class="sxs-lookup"><span data-stu-id="c911a-116">Port a C++/CLI project</span></span>

<span data-ttu-id="c911a-117">要将 C++/CLI 项目移植到 .NET Core，请对 vcxproj 文件进行以下更改。</span><span class="sxs-lookup"><span data-stu-id="c911a-117">To port a C++/CLI project to .NET Core, make the following changes to the vcxproj file.</span></span> <span data-ttu-id="c911a-118">这些迁移步骤不同于其他项目类型所需的步骤，因为 C++/CLI 项目不使用 SDK 样式的项目文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-118">These migration steps differ from the steps needed for other project types because C++/CLI projects don't use SDK-style project files.</span></span>

1. <span data-ttu-id="c911a-119">将 `<CLRSupport>true</CLRSupport>` 属性替换为 `<CLRSupport>NetCore</CLRSupport>`。</span><span class="sxs-lookup"><span data-stu-id="c911a-119">Replace `<CLRSupport>true</CLRSupport>` properties with `<CLRSupport>NetCore</CLRSupport>`.</span></span> <span data-ttu-id="c911a-120">此属性通常位于特定于配置的属性组中，因此你可能需要在多处替换它。</span><span class="sxs-lookup"><span data-stu-id="c911a-120">This property is often in configuration-specific property groups, so you may need to replace it in multiple places.</span></span>
2. <span data-ttu-id="c911a-121">将 `<TargetFrameworkVersion>` 属性替换为 `<TargetFramework>netcoreapp3.1</TargetFramework>`。</span><span class="sxs-lookup"><span data-stu-id="c911a-121">Replace `<TargetFrameworkVersion>` properties with `<TargetFramework>netcoreapp3.1</TargetFramework>`.</span></span>
3. <span data-ttu-id="c911a-122">删除任何 .NET Framework 引用（例如 `<Reference Include="System" />`）。</span><span class="sxs-lookup"><span data-stu-id="c911a-122">Remove any .NET Framework references (like `<Reference Include="System" />`).</span></span> <span data-ttu-id="c911a-123">使用 `<CLRSupport>NetCore</CLRSupport>` 时，将自动引用 .NET Core SDK 程序集。</span><span class="sxs-lookup"><span data-stu-id="c911a-123">.NET Core SDK assemblies are automatically referenced when using `<CLRSupport>NetCore</CLRSupport>`.</span></span>
4. <span data-ttu-id="c911a-124">根据需要更新 cpp 文件中的 API 使用情况，以删除 .NET Core 不可使用的 API。</span><span class="sxs-lookup"><span data-stu-id="c911a-124">Update API usage in cpp files, as necessary, to remove APIs unavailable to .NET Core.</span></span> <span data-ttu-id="c911a-125">由于 C++/CLI 项目通常是非常精简的互操作层，因此通常不需要进行诸多更改。</span><span class="sxs-lookup"><span data-stu-id="c911a-125">Because C++/CLI projects tend to be fairly thin interop layers, there are often not many changes needed.</span></span> <span data-ttu-id="c911a-126">[.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)可用于识别 C++/CLI 二进制文件使用的不受支持的 .NET API，就像使用纯托管二进制文件一样。</span><span class="sxs-lookup"><span data-stu-id="c911a-126">The [.NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md) can be used to identify unsupported .NET APIs used by C++/CLI binaries just as with purely managed binaries.</span></span> <span data-ttu-id="c911a-127">[库移植指南](./libraries.md#determine-portability)中提供了用于确定代码可移植性和更新项目以使用 .NET Core API 的指南。</span><span class="sxs-lookup"><span data-stu-id="c911a-127">Guidelines for determining code portability and updating projects to work with .NET Core APIs are available in the [library porting guidance](./libraries.md#determine-portability).</span></span>

### <a name="wpf-and-windows-forms-usage"></a><span data-ttu-id="c911a-128">WPF 和 Windows 窗体使用情况</span><span class="sxs-lookup"><span data-stu-id="c911a-128">WPF and Windows Forms usage</span></span>

<span data-ttu-id="c911a-129">.NET Core C++/CLI 项目可以使用 Windows 窗体和 WPF API。</span><span class="sxs-lookup"><span data-stu-id="c911a-129">.NET Core C++/CLI projects can use Windows Forms and WPF APIs.</span></span> <span data-ttu-id="c911a-130">要使用这些 Windows 桌面 API，需要将显式框架引用添加到 UI 库。</span><span class="sxs-lookup"><span data-stu-id="c911a-130">To use these Windows desktop APIs, you need to add explicit framework references to the UI libraries.</span></span> <span data-ttu-id="c911a-131">使用 Windows 桌面 API 的 SDK 样式项目通过使用 `Microsoft.NET.Sdk.WindowsDesktop` SDK 来自动引用必要的框架库。</span><span class="sxs-lookup"><span data-stu-id="c911a-131">SDK-style projects that use Windows desktop APIs reference the necessary framework libraries automatically by using the `Microsoft.NET.Sdk.WindowsDesktop` SDK.</span></span> <span data-ttu-id="c911a-132">由于 C++/CLI 项目不使用 SDK 样式的项目格式，因此它们在面向 .NET Core 时需要添加显式框架引用。</span><span class="sxs-lookup"><span data-stu-id="c911a-132">Because C++/CLI projects don't use the SDK-style project format, they need to add explicit framework references when targeting .NET Core.</span></span>

<span data-ttu-id="c911a-133">要使用 Windows 窗体 API，请将此引用添加到 vcxproj 文件：</span><span class="sxs-lookup"><span data-stu-id="c911a-133">To use Windows Forms APIs, add this reference to the vcxproj file:</span></span>

```xml
<!-- Reference all of Windows Forms -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WindowsForms" />
```

<span data-ttu-id="c911a-134">要使用 WPF API，请将此引用添加到 vcxproj 文件：</span><span class="sxs-lookup"><span data-stu-id="c911a-134">To use WPF APIs, add this reference to the vcxproj file:</span></span>

```xml
<!-- Reference all of WPF -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WPF" />
```

<span data-ttu-id="c911a-135">要同时使用 Windows 窗体和 WPF API，请将此引用添加到 vcxproj 文件：</span><span class="sxs-lookup"><span data-stu-id="c911a-135">To use both Windows Forms and WPF APIs, add this reference to the vcxproj file:</span></span>

```xml
<!-- Reference the entirety of the Windows desktop framework:
     Windows Forms, WPF, and the types that provide integration between them -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App" />
```

<span data-ttu-id="c911a-136">目前，不能使用 Visual Studio 的引用管理器来添加这些引用。</span><span class="sxs-lookup"><span data-stu-id="c911a-136">Currently, it's not possible to add these references using Visual Studio's reference manager.</span></span> <span data-ttu-id="c911a-137">而是改为手动更新项目文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-137">Instead, update the project file manually.</span></span> <span data-ttu-id="c911a-138">可通过在 Visual Studio 中卸载项目，然后编辑项目文件来完成此更新。</span><span class="sxs-lookup"><span data-stu-id="c911a-138">This update can be done in Visual Studio by unloading the project and then editing the project file.</span></span> <span data-ttu-id="c911a-139">还可以使用其他编辑器，例如 VS Code。</span><span class="sxs-lookup"><span data-stu-id="c911a-139">You can also use another editor like VS Code.</span></span>

## <a name="build-without-msbuild"></a><span data-ttu-id="c911a-140">在不使用 MSBuild 的情况下生成</span><span class="sxs-lookup"><span data-stu-id="c911a-140">Build without MSBuild</span></span>

<span data-ttu-id="c911a-141">还可以在不使用 MSBuild 的情况下生成 C++/CLI 项目。</span><span class="sxs-lookup"><span data-stu-id="c911a-141">It's also possible to build C++/CLI projects without using MSBuild.</span></span> <span data-ttu-id="c911a-142">请按照以下步骤，使用 cl.exe 和 link.exe 直接生成适用于 .NET Core 的 C++/CLI 项目：</span><span class="sxs-lookup"><span data-stu-id="c911a-142">Follow these steps to build a C++/CLI project for .NET Core directly with *cl.exe* and *link.exe*:</span></span>

1. <span data-ttu-id="c911a-143">编译时，将 `-clr:netcore` 传递给 cl.exe。</span><span class="sxs-lookup"><span data-stu-id="c911a-143">When compiling, pass `-clr:netcore` to *cl.exe*.</span></span>
2. <span data-ttu-id="c911a-144">引用必要的 .NET Core 引用程序集。</span><span class="sxs-lookup"><span data-stu-id="c911a-144">Reference necessary .NET Core reference assemblies.</span></span>
3. <span data-ttu-id="c911a-145">链接时，提供 .NET Core 应用主机目录作为 `LibPath`（以便可以找到 ijwhost.lib）。</span><span class="sxs-lookup"><span data-stu-id="c911a-145">When linking, provide the .NET Core app host directory as a `LibPath` (so that *ijwhost.lib* can be found).</span></span>
4. <span data-ttu-id="c911a-146">将 ijwhost.dll（从 .NET Core 应用主机目录）复制到项目的输出目录。</span><span class="sxs-lookup"><span data-stu-id="c911a-146">Copy *ijwhost.dll* (from the .NET Core app host directory) to the project's output directory.</span></span>
5. <span data-ttu-id="c911a-147">确保将运行托管代码的应用程序的第一个组件存在 [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) 文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-147">Make sure a [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) file exists for the first component of the application that will run managed code.</span></span> <span data-ttu-id="c911a-148">如果应用程序具有托管入口点，则将自动创建并复制 `runtime.config` 文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-148">If the application has a managed entry point, a `runtime.config` file will be created and copied automatically.</span></span> <span data-ttu-id="c911a-149">不过，如果应用程序具有本机入口点，则需要为第一个 C++/CLI 库创建 `runtimeconfig.json` 文件以使用 .NET Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="c911a-149">If the application has a native entry point, though, you need to create a `runtimeconfig.json` file for the first C++/CLI library to use the .NET Core runtime.</span></span>

## <a name="known-issues"></a><span data-ttu-id="c911a-150">已知问题</span><span class="sxs-lookup"><span data-stu-id="c911a-150">Known issues</span></span>

<span data-ttu-id="c911a-151">在处理面向 .NET Core 的 C++/CLI 项目时，需要注意几个已知问题。</span><span class="sxs-lookup"><span data-stu-id="c911a-151">There are a few known issues to look out for when working with C++/CLI projects targeting .NET Core.</span></span>

* <span data-ttu-id="c911a-152">.NET Core C++/CLI 项目中的 WPF 框架引用目前会导致一些有关无法导入符号的无关警告。</span><span class="sxs-lookup"><span data-stu-id="c911a-152">A WPF framework reference in .NET Core C++/CLI projects currently causes some extraneous warnings about being unable to import symbols.</span></span> <span data-ttu-id="c911a-153">可以安全忽略这些警告，并且应该尽快解决它们。</span><span class="sxs-lookup"><span data-stu-id="c911a-153">These warnings can be safely ignored and should be fixed soon.</span></span>
* <span data-ttu-id="c911a-154">如果应用程序具有本机入口点，则首次执行托管代码的 C++/CLI 库需要 [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) 文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-154">If the application has a native entry point, the C++/CLI library that first executes managed code needs a [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) file.</span></span> <span data-ttu-id="c911a-155">此配置文件在 .NET Core 运行时启动时使用。</span><span class="sxs-lookup"><span data-stu-id="c911a-155">This config file is used when the .NET Core runtime starts.</span></span> <span data-ttu-id="c911a-156">C++/CLI 项目不会在生成时自动创建 `runtimeconfig.json` 文件，因此必须手动生成该文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-156">C++/CLI projects don't create `runtimeconfig.json` files automatically at build time yet, so the file must be generated manually.</span></span> <span data-ttu-id="c911a-157">如果从托管入口点调用 C++/CLI 库，则 C++/CLI 库不需要 `runtimeconfig.json` 文件（因为入口点程序集将具有一个在启动运行时时使用的该文件）。</span><span class="sxs-lookup"><span data-stu-id="c911a-157">If a C++/CLI library is called from a managed entry point, then the C++/CLI library doesn't need a `runtimeconfig.json` file (since the entry point assembly will have one that is used when starting the runtime).</span></span> <span data-ttu-id="c911a-158">下面显示了一个简单的示例 `runtimeconfig.json` 文件。</span><span class="sxs-lookup"><span data-stu-id="c911a-158">A simple sample `runtimeconfig.json` file is shown below.</span></span> <span data-ttu-id="c911a-159">有关详细信息，请参阅 [GitHub 上的规范](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md)。</span><span class="sxs-lookup"><span data-stu-id="c911a-159">For more information, see the [spec on GitHub](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md).</span></span>

    ```json
    {
          "runtimeOptions": {
             "tfm": "netcoreapp3.1",
             "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "3.1.0"
             }
          }
    }
    ```

* <span data-ttu-id="c911a-160">在 Windows 7 上，当入口应用程序为原生时，加载 .NET Core C++/CLI 程序集可能会表现出失败的行为。</span><span class="sxs-lookup"><span data-stu-id="c911a-160">On Windows 7, loading a .NET Core C++/CLI assembly when the entry application is native may exhibit failing behavior.</span></span> <span data-ttu-id="c911a-161">这一失败行为是由于 Windows 7 加载程序不遵循 C++/CLI 程序集的非 `mscoree.dll` 入口点。</span><span class="sxs-lookup"><span data-stu-id="c911a-161">This failing behavior is due to the Windows 7 loader not respecting non-`mscoree.dll` entry points for C++/CLI assemblies.</span></span> <span data-ttu-id="c911a-162">建议的操作方式是将入口应用程序转换为托管代码。</span><span class="sxs-lookup"><span data-stu-id="c911a-162">The recommended course of action is to convert the entry application to managed code.</span></span> <span data-ttu-id="c911a-163">Windows 7 上的所有情况都尤其不支持涉及线程本地存储 (TLS) 的场景。</span><span class="sxs-lookup"><span data-stu-id="c911a-163">Scenarios involving Thread Local Storage (TLS) are specifically unsupported in all cases on Windows 7.</span></span>
