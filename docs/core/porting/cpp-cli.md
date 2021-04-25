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
# <a name="how-to-port-a-ccli-project-to-net-core"></a>如何将 C++/CLI 项目移植到 .NET Core

从 .NET Core 3.1 和 Visual Studio 2019 16.4 版开始，[C++/CLI 项目](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp)可面向 .NET Core。 这种支持使你能够将具有 C++/CLI 互操作层的 Windows 桌面应用程序移植到 .NET Core。 本文介绍如何将 C++/CLI 项目从 .NET Framework 移植到 .NET Core 3.1。

## <a name="ccli-net-core-limitations"></a>C++/CLI .NET Core 限制

与其他语言相比，将 C++/CLI 项目移植到 .NET Core 具有一些重要限制：

* .NET Core 的 C++/CLI 支持仅适用于 Windows。
* C++/CLI 项目不能面向 .NET Standard，只能面向 .NET Core（或 .NET Framework）。
* C++/CLI 项目不支持新的 SDK 样式项目文件格式。 相反，即使在面向 .NET Core 时，C++/CLI 项目也使用现有的 vcxproj 文件格式。
* C++/CLI 项目不能同时面向多个 .NET 平台。 如果需要同时为 .NET Framework 和 .NET Core 生成 C++/CLI 项目，请使用单独的项目文件。
* .NET Core 不支持 `-clr:pure` 或 `-clr:safe` 编译，仅支持新的 `-clr:netcore` 选项（等效于 .NET Framework 的 `-clr`）。

## <a name="port-a-ccli-project"></a>移植 C++/CLI 项目

要将 C++/CLI 项目移植到 .NET Core，请对 vcxproj 文件进行以下更改。 这些迁移步骤不同于其他项目类型所需的步骤，因为 C++/CLI 项目不使用 SDK 样式的项目文件。

1. 将 `<CLRSupport>true</CLRSupport>` 属性替换为 `<CLRSupport>NetCore</CLRSupport>`。 此属性通常位于特定于配置的属性组中，因此你可能需要在多处替换它。
2. 将 `<TargetFrameworkVersion>` 属性替换为 `<TargetFramework>netcoreapp3.1</TargetFramework>`。
3. 删除任何 .NET Framework 引用（例如 `<Reference Include="System" />`）。 使用 `<CLRSupport>NetCore</CLRSupport>` 时，将自动引用 .NET Core SDK 程序集。
4. 根据需要更新 cpp 文件中的 API 使用情况，以删除 .NET Core 不可使用的 API。 由于 C++/CLI 项目通常是非常精简的互操作层，因此通常不需要进行诸多更改。 [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)可用于识别 C++/CLI 二进制文件使用的不受支持的 .NET API，就像使用纯托管二进制文件一样。 [库移植指南](./libraries.md#determine-portability)中提供了用于确定代码可移植性和更新项目以使用 .NET Core API 的指南。

### <a name="wpf-and-windows-forms-usage"></a>WPF 和 Windows 窗体使用情况

.NET Core C++/CLI 项目可以使用 Windows 窗体和 WPF API。 要使用这些 Windows 桌面 API，需要将显式框架引用添加到 UI 库。 使用 Windows 桌面 API 的 SDK 样式项目通过使用 `Microsoft.NET.Sdk.WindowsDesktop` SDK 来自动引用必要的框架库。 由于 C++/CLI 项目不使用 SDK 样式的项目格式，因此它们在面向 .NET Core 时需要添加显式框架引用。

要使用 Windows 窗体 API，请将此引用添加到 vcxproj 文件：

```xml
<!-- Reference all of Windows Forms -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WindowsForms" />
```

要使用 WPF API，请将此引用添加到 vcxproj 文件：

```xml
<!-- Reference all of WPF -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WPF" />
```

要同时使用 Windows 窗体和 WPF API，请将此引用添加到 vcxproj 文件：

```xml
<!-- Reference the entirety of the Windows desktop framework:
     Windows Forms, WPF, and the types that provide integration between them -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App" />
```

目前，不能使用 Visual Studio 的引用管理器来添加这些引用。 而是改为手动更新项目文件。 可通过在 Visual Studio 中卸载项目，然后编辑项目文件来完成此更新。 还可以使用其他编辑器，例如 VS Code。

## <a name="build-without-msbuild"></a>在不使用 MSBuild 的情况下生成

还可以在不使用 MSBuild 的情况下生成 C++/CLI 项目。 请按照以下步骤，使用 cl.exe 和 link.exe 直接生成适用于 .NET Core 的 C++/CLI 项目：

1. 编译时，将 `-clr:netcore` 传递给 cl.exe。
2. 引用必要的 .NET Core 引用程序集。
3. 链接时，提供 .NET Core 应用主机目录作为 `LibPath`（以便可以找到 ijwhost.lib）。
4. 将 ijwhost.dll（从 .NET Core 应用主机目录）复制到项目的输出目录。
5. 确保将运行托管代码的应用程序的第一个组件存在 [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) 文件。 如果应用程序具有托管入口点，则将自动创建并复制 `runtime.config` 文件。 不过，如果应用程序具有本机入口点，则需要为第一个 C++/CLI 库创建 `runtimeconfig.json` 文件以使用 .NET Core 运行时。

## <a name="known-issues"></a>已知问题

在处理面向 .NET Core 的 C++/CLI 项目时，需要注意几个已知问题。

* .NET Core C++/CLI 项目中的 WPF 框架引用目前会导致一些有关无法导入符号的无关警告。 可以安全忽略这些警告，并且应该尽快解决它们。
* 如果应用程序具有本机入口点，则首次执行托管代码的 C++/CLI 库需要 [runtimeconfig.json](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) 文件。 此配置文件在 .NET Core 运行时启动时使用。 C++/CLI 项目不会在生成时自动创建 `runtimeconfig.json` 文件，因此必须手动生成该文件。 如果从托管入口点调用 C++/CLI 库，则 C++/CLI 库不需要 `runtimeconfig.json` 文件（因为入口点程序集将具有一个在启动运行时时使用的该文件）。 下面显示了一个简单的示例 `runtimeconfig.json` 文件。 有关详细信息，请参阅 [GitHub 上的规范](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md)。

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

* 在 Windows 7 上，当入口应用程序为原生时，加载 .NET Core C++/CLI 程序集可能会表现出失败的行为。 这一失败行为是由于 Windows 7 加载程序不遵循 C++/CLI 程序集的非 `mscoree.dll` 入口点。 建议的操作方式是将入口应用程序转换为托管代码。 Windows 7 上的所有情况都尤其不支持涉及线程本地存储 (TLS) 的场景。
