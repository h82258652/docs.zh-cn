---
title: .NET 简介和概述
description: 了解用于构建多种应用的免费开源开发平台 .NET。
author: tdykstra
ms.date: 11/16/2020
ms.custom: updateeachrelease
ms.openlocfilehash: 7aa84abdf4653960503699ea7cb239279d748c4e
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106496691"
---
# <a name="introduction-to-net"></a>.NET 简介

.NET 是一种用于构建多种应用的免费开源开发平台，例如：

* [Web 应用、Web API 和微服务](/aspnet/core/introduction-to-aspnet-core#recommended-learning-path)
* [云中的无服务器函数](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-csharp)
* [云原生应用](../architecture/cloud-native/index.md)
* [移动应用](https://dotnet.microsoft.com/learn/xamarin/hello-world-tutorial/intro)
* 桌面应用
  * [Windows WPF](/dotnet/desktop/wpf/)
  * [Windows 窗体](/dotnet/desktop/winforms/)
  * [通用 Windows 平台 (UWP)](/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* [游戏](https://dotnet.microsoft.com/apps/games)
* [物联网 (IoT)](../iot/index.yml)
* [机器学习](../machine-learning/index.yml)
* [控制台应用](tutorials/with-visual-studio-code.md)
* [Windows 服务](/aspnet/core/host-and-deploy/windows-service)

使用[类库](../standard/class-libraries.md)在不同应用和应用类型中共享功能。

使用 .NET 时，无论你正在构建哪种类型的应用，代码和项目文件看起来都一样。 可以访问每个应用的相同运行时、API 和语言功能。

## <a name="cross-platform"></a>跨平台

可以为许多操作系统创建 .NET 应用，包括：

* Windows
* macOS
* Linux
* Android
* iOS
* tvOS
* watchOS

支持的处理器体系结构包括：

* X64
* x86
* ARM32
* ARM64

通过 .NET，可以使用特定于平台的功能，如操作系统 API。 例如 Windows 上的 Windows 窗体和 WPF，以及从 Xamarin 到每个移动平台的原生绑定。

有关详细信息，请参阅[支持的 OS 生命周期策略](https://github.com/dotnet/core/blob/main/os-lifecycle-policy.md)和 [.NET RID 目录](rid-catalog.md)。

## <a name="open-source"></a>开源

.NET 是开放源代码，使用 [MIT 和 Apache 2 许可证](https://github.com/dotnet/runtime/blob/main/LICENSE.TXT)。 .NET 是 [.NET Foundation](https://dotnetfoundation.org/) 的项目。

有关详细信息，请参阅 [GitHub.com 上的项目存储库列表](https://github.com/dotnet/core/blob/main/Documentation/core-repos.md)。

## <a name="support"></a>支持

Microsoft 支持在 Windows、macOS 和 Linux 上使用 .NET。 它会定期更新以保证安全和质量（每月的第二个星期二）。

Microsoft 的 .NET 二进制发行版在 Azure 中的 Microsoft 维护服务器上进行生成和测试，并遵循 Microsoft 的工程和安全实践。

[Red Hat 支持在 Red Hat Enterprise Linux (RHEL) 上使用 .NET](https://developers.redhat.com/topics/dotnet/)。 Red Hat 和 Microsoft 开展协作，共同确保 .NET Core 能够在 RHEL 上正常运行。

[Tizen 支持在 Tizen 平台上使用 .NET](https://developer.tizen.org/development/training/.net-application)。

有关详细信息，请参阅 [.NET Core 和 .NET 5 的版本和支持](releases-and-support.md)。

## <a name="tools-and-productivity"></a>工具与工作效率

.NET 为用户提供了各种语言、集成开发环境 (IDE) 和其他工具的选择。

### <a name="programming-languages"></a>编程语言

.NET 支持三种编程语言：

* [C#](../csharp/index.yml)

  C#（读作“See Sharp”）是一种新式编程语言，不仅面向对象，还类型安全。 C# 源于 C 语言系列，C、C++、Java 和 JavaScript 程序员很快就可以上手使用。

* [F#](../fsharp/index.yml)

  F# 语言支持函数式、命令式、面向对象的编程模式。

* [Visual Basic](../visual-basic/index.yml)

  在 .NET 语言中，Visual Basic 的语法最接近于人类的普通用语，因此更易于学习。 不同于 C# 和 F#（Microsoft 正在积极为 C# 和 F# 开发新功能），Visual Basic 语言是稳定的。 Visual Basic 不受 Web 应用支持，但受 Web API 支持。

下面是 .NET 语言支持的一些功能：

* [类型安全](../standard/base-types/common-type-system.md)
* 类型推理 - [C#](../csharp/programming-guide/types/index.md#specifying-types-in-variable-declarations)、[F#](../fsharp/language-reference/type-inference.md)、[Visual Basic](../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
* [泛型类型](../standard/generics.md)
* [委托](../standard/delegates-lambdas.md)
* [Lambda](../standard/delegates-lambdas.md)
* [事件](../standard/events/index.md)
* [异常](../standard/exceptions/index.md)
* [特性](../standard/attributes/index.md)
* [异步代码](../standard/async.md)
* [并行编程](../standard/parallel-programming/index.md)
* [代码分析器](../fundamentals/code-analysis/overview.md)

### <a name="ides"></a>IDE

.NET 的集成开发环境包括：

* [Visual Studio](https://visualstudio.microsoft.com/vs/)

  仅在 Windows 上运行。 具有广泛的内置功能，设计为可以与 .NET 一起使用。 社区版对学生、开放源代码贡献者和个人免费。

* [Visual Studio Code](https://code.visualstudio.com/)

  在 Windows、macOS 和 Linux 上运行。 免费且开源。 扩展可用于使用 .NET 语言。

* [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)

  仅在 macOS 上运行。 用于开发适用于 iOS、Android 和 Web 的 .NET 应用和游戏。

* [GitHub Codespaces](https://github.com/features/codespaces)

  联机 Visual Studio Code 环境，当前为 beta 版本。

### <a name="sdk-and-runtimes"></a>SDK 和运行时

[.NET SDK](sdk.md) 是一组用于开发和运行 .NET 应用程序的库和工具。

[下载 .NET](https://dotnet.microsoft.com/download/dotnet) 时，可以选择 SDK 或 *运行时*，例如 .NET 运行时或 ASP.NET Core 运行时。 在要准备运行 .NET 应用的计算机上安装运行时。 在要用于开发的计算机上安装 SDK。 下载 SDK 时，将自动获取运行时。

SDK 下载包括以下组件：

* [.NET CLI](tools/index.md)。 可用于本地开发和持续集成脚本的命令行工具。
* `dotnet` [驱动程序](tools/index.md#driver)。 用于运行依赖于框架的应用的 CLI 命令。
* [Roslyn](https://github.com/dotnet/roslyn) 和 [F#](https://github.com/microsoft/visualfsharp) 编程语言编译器。
* [MSBuild](/visualstudio/msbuild/msbuild) 生成引擎。
* [.NET 运行时](#clr)。 提供类型系统、程序集加载、垃圾回收器、本机互操作和其他基本服务。
* [运行时库](#runtime-libraries)。 提供基元数据类型和基本实用程序。
* ASP.NET Core 运行时。 为连接 Internet 的应用（如 Web 应用、IoT 应用和移动后端）提供基本服务。
* 桌面运行时。 为 Windows 桌面应用（包括 Windows 窗体和 WPF）提供基本服务。

运行时下载包括以下组件：

* （可选）桌面或 ASP.NET Core 运行时。
* [.NET 运行时](#clr)。 提供类型系统、程序集加载、垃圾回收器、本机互操作和其他基本服务。
* [运行时库](#runtime-libraries)。 提供基元数据类型和基本实用程序。
* `dotnet` [驱动程序](tools/index.md#driver)。 用于运行依赖于框架的应用的 CLI 命令。

有关详细信息，请参阅以下资源：

* [.NET SDK 概述](sdk.md)
* [.NET CLI 概述](tools/index.md)
* [dotnet 命令](tools/dotnet.md)

### <a name="project-system-and-msbuild"></a>项目系统和 MSBuild

.NET 应用是使用 [MSBuild](/visualstudio/msbuild/msbuild) 从源代码中生成的。 项目文件（.csproj、.fsproj 或 .vbproj）指定[目标](/visualstudio/msbuild/msbuild-targets)和负责编译、打包和发布代码的关联[任务](/visualstudio/msbuild/msbuild-tasks)  。 有引用目标和任务的标准集合的 SDK 标识符。 使用这些标识符有助于使项目文件较小且易于使用。 例如，下面是控制台应用的一个项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>
</Project>
```

下面是 Web 应用的一个项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>
</Project>
```

在这些示例中，`Project` 元素的 `Sdk` 属性指定一组 MSBuild 目标和生成项目的任务。  `TargetFramework` 元素指定应用所依赖的 .NET 版本。 可以编辑项目文件以添加特定于项目的其他目标和任务。

有关详细信息，请参阅 [.NET 项目 SDK 概述](project-sdk/overview.md)和[目标框架](../standard/frameworks.md)。

### <a name="cicd"></a>CI/CD

MSBuild 和 .NET CLI 可与各种连续集成工具和环境一起使用，例如：

* [GitHub Actions](https://github.com/features/actions)
* [Azure DevOps](/azure/devops/user-guide/what-is-azure-devops)
* [CAKE](https://cakebuild.net/)
* [FAKE](https://fake.build/)

有关详细信息，请参阅[在持续集成 (CI) 中使用 .NET SDK 和工具](tools/using-ci-with-cli.md)

### <a name="nuget"></a>NuGet

[NuGet](/nuget/what-is-nuget) 是为 .NET 设计的开源包管理器。 NuGet 包是具有 `.nupkg` 扩展的 .zip 文件，此扩展包含编译代码 (DLL)、与该代码相关的其他文件以及描述性清单（包含包版本号等信息）。 使用代码的开发人员共享创建包，并将其发布到 [nuget.org](https://nuget.org) 或专用主机。 希望使用共享代码的开发人员将包添加到其项目中，然后可以在项目代码中调用包公开的 API。

有关详细信息，请参阅 [NuGet 文档](/nuget/)。

### <a name="net-interactive"></a>.NET Interactive

.NET Interactive 是一组 CLI 工具和 API，使用户能够跨 Web、markdown 和笔记本创建交互式体验。

有关详细信息，请参阅以下资源：

* [.NET 浏览器内教程](https://dotnet.microsoft.com/learn/dotnet/in-browser-tutorial/1)
* [在计算机上将 .NET 笔记本与 Jupyter 配合使用](https://github.com/dotnet/interactive/blob/main/docs/NotebooksLocalExperience.md)
* [.NET Interactive 文档](https://github.com/dotnet/interactive/blob/main/docs/README.md)

## <a name="execution-models"></a>执行模型

.NET 应用在称为“公共语言运行时 (CLR)”的运行时环境中运行[托管代码](../standard/managed-code.md)。

### <a name="clr"></a>CLR

.NET [CLR](../standard/clr.md) 是包含 Windows、macOS 和 Linux 支持的跨平台运行时。 CLR 处理内存分配和管理。 CLR 也是一个虚拟机，不仅可执行应用，还可使用实时 JIT 编译器生成和编译代码。

有关详细信息，请参阅[公共语言运行时 (CLR) 概述](../standard/clr.md)。

### <a name="jit-compiler-and-il"></a>JIT 编译器和 IL

C# 等较高级的 .NET 语言编译为称为中间语言 (IL) 的硬件无关性指令集。 应用运行时，JIT 编译器将 IL 转换为处理器可理解的计算机代码。 JIT 编译发生在要运行代码的同一台计算机上。

由于 JIT 编译在应用程序的执行过程中发生，因此编译时间是运行时的一部分。 因此，JIT 编译器需要平衡优化代码所花费的时间与生成代码时可节约的时间。 但 JIT 编译器知道实际硬件，这样开发人员就无需为不同平台提供不同的实现。

.NET JIT 编译器可以执行分层编译，这意味着它可以在运行时重新编译各个方法。 通过此功能，它可以快速编译，同时仍然能够为常用方法生成高度优化的代码版本。

有关详细信息，请参阅[托管执行过程](../standard/managed-execution-process.md)和[分层编译](whats-new/dotnet-core-3-0.md#tiered-compilation)。

### <a name="aot-compiler"></a>AOT 编译器

大多数 .NET 工作负载的默认体验是 JIT 编译器，但 .NET 提供两种形式的预先 (AOT) 编译：

* 某些场景需要 100% AOT 编译。 例如 [iOS](/xamarin/ios/)。
* 在其他情况下，应用的大多数代码都是 AOT 编译的，但有些代码是 JIT 编译的。 某些代码模式不适用于 AOT（如泛型）。 这种形式的 AOT 编译的示例为[准备运行](whats-new/dotnet-core-3-0.md#readytorun-images)发布选项。 这种形式的 AOT 具有 AOT 的优点并且没有 AOT 的缺点。

### <a name="automatic-memory-management"></a>自动内存管理

垃圾回收器 (GC) 管理应用程序的内存分配和释放。 每当代码新建对象时，CLR 都会从[托管堆](../standard/garbage-collection/fundamentals.md#the-managed-heap)为对象分配内存。 只要托管堆中有地址空间，运行时就会继续为新对象分配空间。 没有足够的可用地址空间时，GC 将检查托管堆中应用程序不再使用的对象。 然后回收该内存。

GC 是一种有助于确保内存安全的 CLR 服务。 如果某个程序仅访问分配的内存，则该程序就是内存安全的。 例如，运行时可确保应用不会访问超过数组边界的未分配内存。

有关详细信息，请参阅[自动内存管理](../standard/automatic-memory-management.md)和[垃圾回收的基础知识](../standard/garbage-collection/fundamentals.md)。

### <a name="working-with-unmanaged-resources"></a>处理未托管的资源

有时，代码需要引用 *非托管资源*。 未托管的资源是指不由 .NET 运行时自动维护的资源。 例如，文件句柄就是未托管的资源。 <xref:System.IO.FileStream> 对象是一个托管对象，但它引用未托管的文件句柄。 用完 <xref:System.IO.FileStream> 之后，需要显式释放文件句柄。

在 .NET 中，引用未托管资源的对象会实现 <xref:System.IDisposable> 接口。 用完对象后，需调用此对象的 <xref:System.IDisposable.Dispose> 方法，该方法会释放所有托管资源。 .NET 语言提供一种方便的 `using` 语句（[C#](../csharp/language-reference/keywords/using.md)、[F#](../fsharp/language-reference/resource-management-the-use-keyword.md)、[VB](../visual-basic/language-reference/statements/using-statement.md)），确保调用 `Dispose` 方法。

有关详细信息，请参阅[清理非托管资源](../standard/garbage-collection/unmanaged.md)。

## <a name="deployment-models"></a>部署模型

可以在两种不同模式下发布 .NET 应用：

* 将应用作为独立应用，生成的可执行文件将包含 .NET [运行时](#sdk-and-runtimes)和[库](#runtime-libraries)，以及该应用程序及其依赖项。 应用程序的用户可以在未安装 .NET 运行时的计算机上运行该应用程序。 独立应用是特定于平台的，可以使用 [AOT 编译](#aot-compiler)形式进行选择性发布。

* 将应用作为依赖于框架的应用发布会生成一个可执行文件和多个二进制文件（.dll 文件），其中仅包括应用程序本身及其依赖项 。 应用程序的用户必须单独安装 .NET [运行时](#sdk-and-runtimes)。 可执行文件是特定于平台的，但依赖于框架的应用程序的 .dll 文件是跨平台的。

  可以并行安装多个版本的运行时，以运行针对不同运行时版本的依赖于框架的应用。 有关详细信息，请参阅[目标框架](../standard/frameworks.md)。

可执行文件是针对特定目标平台生成的，你可以通过[运行时标识符 (RID)](rid-catalog.md) 指定。

有关详细信息，请参阅 [.NET 应用程序发布概述](deploying/index.md)和 [.NET 和 Docker 简介](docker/introduction.md)。

## <a name="runtime-libraries"></a>运行时库

.NET 具有一组广泛的标准类库，称为[运行时库](../standard/glossary.md#runtime)、[框架库](../standard/glossary.md#framework-libraries)或[基类库 (BCL)](../standard/glossary.md#bcl)。 这些库为许多常规用途类型和特定于工作负载的类型和实用工具功能提供实现。

下面是在 .NET 运行时库中定义的一些类型示例：

* 基元类型，如 <xref:System.Boolean?displayProperty=nameWithType> 和 <xref:System.Int32?displayProperty=nameWithType>。
* 集合，例如 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>。
* 数据类型，例如 <xref:System.Data.DataSet?displayProperty=nameWithType> 和 <xref:System.Data.DataTable?displayProperty=nameWithType>。
* 网络实用程序类型，如 <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>。
* [文件和流 I/O](../standard/io/index.md) 实用程序类型，如 <xref:System.IO.FileStream?displayProperty=nameWithType> 和 <xref:System.IO.TextWriter?displayProperty=nameWithType>。
* [序列化](../standard/serialization/index.md)实用程序类型，例如 <xref:System.Text.Json.JsonSerializer?displayProperty=nameWithType> 和 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>。
* 高性能类型，例如 <xref:System.Span%601?displayProperty=nameWithType>、<xref:System.Numerics.Vector?displayProperty=nameWithType> 和 [Pipelines](../standard/io/pipelines.md)。

有关详细信息，请参阅[运行时库概述](../standard/runtime-libraries-overview.md)。 库的源代码位于 [GitHub dotnet/运行时存储库](https://github.com/dotnet/runtime/tree/main/src/libraries)中。

### <a name="extensions-to-the-runtime-libraries"></a>运行时库的扩展

某些常用应用程序功能的库没有包含在运行时库中，但在 NuGet 包中提供，如下所示：

| NuGet 包 | 文档 |
|---------|---------|
| [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) | [应用程序生存期管理（通用主机）](extensions/generic-host.md) |
| [Microsoft.Extensions.DependencyInjection](https://www.nuget.org/packages/Microsoft.Extensions.DependencyInjection) | [依赖关系注入 (DI)](extensions/dependency-injection.md)
| [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) | [配置](extensions/configuration.md) |
| [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging) | [Logging](extensions/logging.md) |
| [Microsoft.Extensions.Options](https://www.nuget.org/packages/Microsoft.Extensions.Options) | [选项模式](extensions/options.md) |

有关详细信息，请参阅 [GitHub 上的 dotnet/extensions 存储库](https://github.com/dotnet/extensions)。

## <a name="data-access"></a>数据访问

.NET 提供了对象/关系映射器 (ORM) 和一种在代码中编写 SQL 查询的方法。

### <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core 是一种可用作 ORM 的[开源](https://github.com/aspnet/EntityFrameworkCore)和跨平台的数据访问技术。 借助 EF Core，可以通过引用代码中的 .NET 对象来处理数据库。 它减少了需要编写和测试的数据访问代码的数量。 EF Core 支持许多数据库引擎。

有关详细信息，可参阅 [Entity Framework Core](/ef/core/) 和[数据库提供程序](/ef/core/providers/)。

### <a name="linq"></a>LINQ

通过语言集成查询 (LINQ)，可以编写用于数据操作的声明性代码。 数据可采用多种形式（例如，内存中对象、SQL 数据库或 XML 文档），但针对每个数据源编写的 LINQ 代码往往没有差别。

有关详细信息，请参阅 [LINQ（语言集成查询）概述](../standard/linq/index.md)。

## <a name="net-terminology"></a>.NET 术语

了解一些术语在一段时期内的用法变化有利于理解 .NET 文档。

### <a name="net-core-and-net-5"></a>.NET Core 和 .NET 5

在 2002 年，Microsoft 发布了 [.NET Framework](../framework/get-started/overview.md)，这是用于创建 Windows 应用的开发平台。 目前 .NET Framework 的版本为 4.8，并且仍[由 Microsoft 支持](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)。

2014 年，Microsoft 开始编写 .NET Framework 的跨平台开源后续产品。 .NET 的这个新实现被命名为 .NET Core，直到发展到版本 3.1。 .NET Core 3.1 之后的下一个版本是 .NET 5.0，当前处于预览状态。 版本号 4 被跳过，以避免 .NET 的此实现和 .NET Framework 4.8 混淆。 删除名称“Core”以表明这是现在 .NET 的主要实现。

本文是关于 .NET 5 的，但 .NET 5 文档的许多内容仍然引用“.NET Core”或“.NET Framework”。 此外，“Core”在名称 [ASP.NET Core](/aspnet/core/) 和 [Entity Framework Core](/ef/core/) 中保留。

该文档还引用 .NET Standard。 [.NET Standard](../standard/net-standard.md) 是一种 API 规范，可让你为 .NET 的多个实现开发类库。

有关详细信息，请参阅 [.NET 体系结构组件](../standard/components.md)。

### <a name="overloaded-terms"></a>重载术语

.NET 的一些术语可能会令人困惑，因为同一个术语在不同的上下文中的用法不同。 下面是一些较明显的例子：

* **运行时**

  |上下文  |“运行时”含义 |
  |---------|---------|
  | [公共语言运行时 (CLR)](#clr)| 用于托管程序的执行环境。 OS 属于运行时环境，但不属于 .NET 运行时。 |
  | [.Net 下载页上的 .NET 运行时](https://dotnet.microsoft.com/download/dotnet) | [CLR](#clr) 和[运行时库](#runtime-libraries)，它们一起提供了对运行[依赖于框架](#deployment-models)的应用的支持。 此页还提供 ASP.NET Core 服务器应用和 Windows 桌面应用的运行时选项。 |
  | [运行时标识符 (RID)](rid-catalog.md) | 运行 .NET 应用的 OS 平台和 CPU 体系结构。 例如：Windows x64、Linux x64。 |

* **框架**

  |上下文  | “框架”含义 |
  |---------|---------------------|
  | .NET framework | .NET 的原始、仅限 Windows 的实现。 “框架”首字母大写。 |
  | Target Framework — 目标 Framework | .NET 应用或库依赖的 API 集合。 示例：.NET Core 3.1、.NET Standard 2.0 |
  | 目标框架名字对象 (TFM)  | TFM 是一种标准化令牌格式，用于指定 .NET 应用或库的目标框架。 示例：`net462`（对于 .NET Framework 4.6.2）。 |
  | 依赖于框架的应用 | 只能在从 [.NET 下载页](https://dotnet.microsoft.com/download/dotnet)安装了运行时的计算机上运行的应用。 此用法中的“框架”与你从 .NET 下载页下载的“运行时”是相同的。 |
  | 框架库 | 有时用作[运行时库](#runtime-libraries)的同义词。 |

* **SDK**

  |上下文  | “SDK”含义 |
  |---------|---------------|
  | [.NET 下载页上的 SDK](#sdk-and-runtimes)  | 工具和库的集合，你下载和安装它们以开发和运行 .NET 应用。 包括 CLI、MSBuild、.NET 运行时和其他组件。|
  | [SDK 样式项目](#project-system-and-msbuild) | 一组 MSBuild 目标和任务，用于指定如何为特定应用类型生成项目。 这种情况下的 SDK 是使用项目文件中 `Project` 元素的 `Sdk` 属性指定的。 |

* **平台**

  |上下文  | “平台”含义 |
  |---------|--------------------|
  | 跨平台 | 在此术语中，“平台”表示操作系统以及运行它的硬件，例如 Windows、macOS、Linux、iOS 和 Android。 |
  | .NET 平台 | 用法有所不同。 引用可以是针对 .NET （如 .NET Framework 或 .NET 5）的一种实现，也可以是针对包括所有实现的 .NET 的总体概念。 |

有关 .NET 术语的详细信息，请参阅 [.NET 术语表](../standard/glossary.md)。

## <a name="advanced-scenarios"></a>高级方案

以下各部分介绍在一些高级场景中非常有用的 .NET 功能。

### <a name="native-interop"></a>原生互操作

每个操作系统都有一个应用程序编程接口 (API)，用于提供系统服务。 .NET 提供多种方式来调用这些 API。

与原生 API 进行互操作的主要方式是使用“平台调用”，简称 P/Invoke。 跨 Linux 和 Windows 平台支持 P/Invoke。 进行互操作的一种仅限 Windows 的方式称为“COM 互操作”，用于在托管代码中操作 [COM 组件](/cpp/atl/introduction-to-com)。 这种方式建立在 P/Invoke 基础结构之上，但工作原理略有不同。

有关详细信息，请参阅[原生互操作性](../standard/native-interop/index.md)。

### <a name="unsafe-code"></a>不安全代码

根据语言支持，CLR 可通过 `unsafe` 代码访问本机内存和执行指针算术运算。 某些算法和系统互操作性需要这些操作。 尽管不安全代码的功能强大，但除非有必要与系统 API 互操作或实现最高效的算法，否则不建议使用。 在不同的环境中，不安全代码的执行方式可能不同，使用它还会丧失垃圾回收器和类型安全带来的好处。 建议尽可能地限制和集中化使用不安全代码，并全面测试该代码。

有关详细信息，请参阅[不安全代码和指针](../csharp/language-reference/unsafe-code.md)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [选择 .NET 教程](tutorials/index.md)

> [!div class="nextstepaction"]
> [在浏览器中试用 .NET](../csharp/tutorials/intro-to-csharp/numbers-in-csharp.yml)

> [!div class="nextstepaction"]
> [C# 导览](../csharp/tour-of-csharp/index.md)

> [!div class="nextstepaction"]
> [F# 导览](../fsharp/tour.md)
