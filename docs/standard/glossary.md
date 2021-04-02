---
title: .NET 术语表
description: 了解 .NET 文档中所用的选定术语的含义。
ms.date: 11/16/2020
ms.openlocfilehash: 009ab0266a4479dfd8a37cb3261ca6fae7c78b8e
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873180"
---
# <a name="net-glossary"></a>.NET 术语表

此术语表的主要目的是阐明所选术语和缩写词的含义，这些词频繁出现在 .NET 文档中但没有定义。

## <a name="aot"></a>AOT

预编译器。

与 [JIT](#jit) 类似，此编译器还可将 [IL](#il) 转换为机器代码。 与 JIT 编译相比，AOT 编译在应用程序执行前进行并且通常在不同计算机上执行。 AOT 工具不会在运行时进行编译，因此它们不需要最大程度地减少编译所花费的时间。 这意味着它们可花更多的时间进行优化。 由于 AOT 的上下文是整个应用程序，因此 AOT 编译器还会执行跨模块链接和全程序分析，这意味着之后会进行所有引用并会生成单个可执行文件。

请参阅 [CoreRT](#corert) 和 [.NET Native](#net-native)。

## <a name="app-model"></a>应用模型

特定于[工作负载](#workload)的 API。 下面是一些示例：

* ASP.NET
* ASP.NET Web API
* 实体框架 (EF)
* Windows Presentation Foundation (WPF)
* Windows Communication Foundation (WCF)
* Windows Workflow Foundation (WF)
* Windows 窗体 (WinForms)

## <a name="aspnet"></a>ASP.NET

随 .NET Framework 一起提供的原始 ASP.NET 实现。

有时 ASP.NET 是一个涵盖性术语，指包含 ASP.NET Core 在内的两个 ASP.NET 实现。 该术语在任何给定实例中的含义取决于上下文。 若要指明不要使用 ASP.NET 来表示这两种实现，请参考 ASP.NET 4.x。

请参阅 [ASP.NET 文档](/aspnet/#pivot=aspnet)。

## <a name="aspnet-core"></a>ASP.NET Core

一种跨平台、高性能的开放源代码 ASP.NET 实现。

请参阅 [ASP.NET Core 文档](/aspnet/#pivot=core)。

## <a name="assembly"></a>程序集 (assembly)

.dll/.exe 文件，其中包含一组可由应用程序或其他程序集调用的 API。

程序集可以包括接口、类、结构、枚举和委托等类型。 有时，项目的 bin 文件夹中的程序集被称为二进制文件。 另请参阅[库](#library)。

## <a name="bcl"></a>BCL

基类库。

一组构成 System.\*（在一定的程度上构成 Microsoft.\*）命名空间的库。 BCL 是用于生成 ASP.NET Core 等较高级应用程序框架的较低级通用框架。

[.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的 BCL 的源代码包含在 [.NET 运行时存储库](https://github.com/dotnet/runtime)中。 这些 BCL API 中的大多数也可以在 .NET Framework 中获取，因此可将此源代码视为 .NET Framework BCL 源代码的一个分支。

以下术语通常指 BCL 引用的相同 API 集合：

- [核心 .NET 库](../core/compatibility/corefx.md)
- [框架库](#framework-libraries)
- [运行时库](#runtime)
- [共享框架](#shared-framework)

## <a name="clr"></a>CLR

公共语言运行时。

具体含义依赖于上下文。 公共语言运行时通常指 [.NET Framework](#net-framework) 或 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的运行时。

CLR 处理内存分配和管理。 CLR 也是一种虚拟机，不仅可执行应用，还可使用 [JIT](#jit) 编译器快速生成和编译代码。

适用于 .NET Framework 的 CLR 实现仅限于 Windows。

.NET 5 及更高版本的 CLR 实现（也称为 Core CLR）是基于与 .NET Framework CLR 相同的代码库而构建的。 Core CLR 最初是 Silverlight 的运行时，并且设计为在多个平台上运行，尤其是在 Windows 和 OS X 上运行。它仍是[跨平台](#cross-platform)运行时，现在包括对许多 Linux 分发的支持。

另请参阅[运行时](#runtime)。

## <a name="core-clr"></a>Core CLR

[.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的公共语言运行时。

请参阅 [CLR](#clr)

## <a name="corert"></a>CoreRT

与 [CLR](#clr) 相比，CoreRT 不是虚拟机，这意味着它不包含用于快速生成并运行代码的功能，因为它不包括 [JIT](#jit)。 但它包含 [GC](#gc) 以及运行时类型标识 (RTTI) 和反射功能。 只是由于设计有类型系统，因此并不需要元数据反射功能。 不需要元数据使它具有 [AOT](#aot) 工具链，该工具链可去除多余的元数据，更重要的是可识别应用不使用的代码。 CoreRT 正在开发中。

请参阅 [.NET Native 和 CoreRT 简介](https://github.com/dotnet/corert/blob/main/Documentation/intro-to-corert.md)。

## <a name="cross-platform"></a>跨平台

能够开发并执行可在多个不同操作系统（如 Linux、Windows 和 iOS）上使用的应用程序，而无需专门针对每个操作系统进行重新编写。 这样，可以在不同平台上的应用程序之间重复使用代码并保持一致性。

## <a name="ecosystem"></a>生态系统

所有针对给定技术生成和运行应用程序的运行时软件、开发工具和社区资源。

在所包含的第三方应用和库方面，术语“.NET 生态系统”不同于类似的“.NET 堆栈”等术语。 请看以下这一句示例：

- “推出 [.NET Standard](#net-standard) 的背后动机是要提高 .NET 生态系统中的一致性。”

## <a name="framework"></a>框架

一般指一个综合 API 集合，便于开发和部署基于特定技术的应用程序。 从此常规意义上来说，ASP.NET Core 和 Windows 窗体都是示例应用程序框架。 另请参阅[库](#library)。

“框架”一词在以下术语中有不同的含义：

- [框架库](#framework-libraries)
- [.NET Framework](#net-framework)
- [共享框架](#shared-framework)
- [目标框架](#target-framework)
- [TFM（目标框架名字对象）](#tfm)
- [依赖于框架的应用](../core/deploying/index.md#publish-framework-dependent)

有时，“框架”指的是 [.NET 实现](#implementation-of-net)。 例如，某文章可能会将 .NET 5 称为框架。

## <a name="framework-libraries"></a>框架库

含义取决于上下文。 可指适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的框架库，在这种情况下，它指 [BCL](#bcl) 引用的相同的库。 它也可指在 BCL 上构建并为 Web 应用提供其他 API 的 ASP.NET Core 框架库。

## <a name="gc"></a>GC

垃圾回收器。

垃圾回收器是自动内存管理的实现。  GC 可释放不再使用的对象占用的内存。

请参阅[垃圾回收](garbage-collection/index.md)。

## <a name="il"></a>IL

中间语言。

C# 等较高级的 .NET 语言编译为称为中间语言 (IL) 的硬件无关性指令集。 IL 有时被称为 MSIL (Microsoft IL) 或 CIL（通用 IL）。

## <a name="jit"></a>JIT

实时编译器。

与 [AOT](#aot) 类似，此编译器将 [IL](#il) 转换为处理器可理解的计算机代码。 与 AOT 不同，JIT 编译在需要运行代码的同一台计算机上按需执行。 由于 JIT 编译在应用程序的执行过程中发生，因此编译时是运行时的一部分。 因此，JIT 编译器需要平衡优化代码所花费的时间与生成代码时可节约的时间。 但 JIT 知道实际硬件，这样开发人员就无需提供不同的实现。

## <a name="implementation-of-net"></a>.NET 实现

.NET 的实现包括：

- 一个或多个运行时。 示例：[CLR](#clr)、[CoreRT](#corert)。
- 实现 .NET Standard 的某版本并且可能包含其他 API 的类库。 示例：[.NET Framework](#net-framework) 和 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的 [BCL](#bcl)。
- 可选择包含一个或多个应用程序框架。 示例：[ASP.NET](#aspnet)、Windows 窗体及 WPF 包含在 .NET Framework 和 .NET 5 中。
- 可包含开发工具。 某些开发工具在多个实现之间共享。

.NET 实现的示例：

- [.NET Framework](#net-framework)
- [.NET 5 及更高版本（包括 .NET Core 2.1-3.1）](#net-5-and-later-versions)
- [通用 Windows 平台 (UWP)](#uwp)
- [Mono](#mono)

## <a name="library"></a>库

可由应用或其他库调用的 API 集合。 .NET 库由一个或多个[程序集](#assembly)组成。

词库和[框架](#framework)通常作同义词使用。

## <a name="mono"></a>Mono

Mono 是主要在需要小型运行时使用的开放源、[跨平台](#cross-platform) .NET 实现。 它是在 Android、Mac、iOS、tvOS 和 watchOS 上为 Xamarin 应用程序提供技术支持的运行时，主要针对内存占用少的应用程序。

它支持所有当前已发布的 .NET Standard 版本。

以前，Mono 实现更大的 .NET Framework API 并模拟一些 Unix 上最常用的功能。 有时使用它运行依赖 Unix 上的这些功能的 .NET 应用程序。

Mono 通常与[实时编译器](#jit)一起使用，但它也提供在 iOS 之类的平台使用的完整[静态编译器（预先编译）](#aot)。

请参阅 [Mono 文档](https://www.mono-project.com/docs/)。

## <a name="net"></a>.NET

[.NET Standard](#net-standard) 和所有 [.NET 实现](#implementation-of-net)及工作负荷的涵盖性术语。 始终采用全大写形式，请勿使用“.Net”。

当 [.NET5](#net-5-and-later-versions)（当前处于预览状态）发布时，它将是所有新 .NET 开发的建议 .NET 实现，因此在某些上下文中，“.NET”将表示“.NET 5 及更高版本”。

请参阅 [.NET 基础知识](../fundamentals/index.yml)

## <a name="net-5-and-later-versions"></a>.NET 5 及更高版本

一种跨平台、高性能的开放源 .NET 实现。 包括公共语言运行时 ([CLR](#clr))、[AOT](#aot) 运行时（正在开发中的 [CoreRT](#corert)）、基类库 ([BCL](#bcl)) 以及 [.NET SDK](#net-sdk)。

此 .NET 实现的早期版本称为 .NET Core。 .Net 5.0 是继 .NET Core 3.1 之后的下一版本。 跳过了版本 4，以避免将此较新的 .NET 实现与称为 [.NET Framework](#net-framework) 的旧实现混淆。 .NET Framework 的当前版本为版本 4.8。

请参阅 [.NET 基础知识](../fundamentals/index.yml)。

## <a name="net-cli"></a>.NET CLI

用于开发适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的应用程序和库的跨平台工具链。 也称为 .NET Core CLI。

请参阅 [.NET CLI](../core/tools/index.md)。

## <a name="net-core"></a>.NET Core

请参阅 [.NET 5 及更高版本](#net-5-and-later-versions)。

## <a name="net-framework"></a>.NET Framework

仅在 Windows 上运行的 .NET 实现。 包括公共语言运行时 ([CLR](#clr))、基类库 ([BCL](#bcl)) 以及应用程序框架库（例如 [ASP.NET](#aspnet)、Windows 窗体和 WPF）。

请查阅 [.NET Framework 指南](../framework/index.yml)。

## <a name="net-native"></a>.NET Native

编译器工具链，可预先 ([AOT](#aot)) 生成，而非实时 ([JIT](#jit)) 生成本机代码。

编译采用与 C++ 编译器和链接器类似的工作方式在开发人员计算机上进行。 它删除了未使用的代码，留出更多时间进行优化。 它从库中提取代码，将它们合并到可执行文件中。 结果是表示整个应用的单个模块。

UWP 是 .NET Native 支持的首个应用程序框架。 现在，我们支持为 Windows、macOS 和 Linux 生成本机控制台应用。

请参阅 [.NET Native 和 CoreRT 简介](https://github.com/dotnet/corert/blob/main/Documentation/intro-to-corert.md)

## <a name="net-sdk"></a>.NET SDK

一组库和工具，开发人员可用其创建适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的 .NET Core 应用程序和库。 也称为 .NET Core SDK。

包括用于生成应用的 [.NET CLI](#net-cli)、用于生成和运行应用的 .NET 库以及用于运行 CLI 命令和运行应用程序的 dotnet 可执行文件 (dotnet.exe)。

请参阅 [.NET SDK 概述](../core/sdk.md)。

## <a name="net-standard"></a>.NET Standard

在每个 .NET 实现中都可用的 .NET API 正式规范。

.NET Standard 规范有时被称为文档中的库。 由于库不仅包括规范（接口），还包括 API 实现，所以会误将 .NET Standard 称为“库”。

请参阅 [.NET Standard](net-standard.md)。

## <a name="ngen"></a>NGEN

本机（映像）生成。

可将此方法视为永久性 [JIT](#jit) 编译器。 它通常在执行代码的计算机上编译该代码，但通常在安装时进行编译。

## <a name="package"></a>包

NuGet 包 &mdash; 或只是一个包 &mdash; 是一个 .zip 文件，其中具有一个或多个名称相同的程序集以及作者姓名等其他元数据。

.zip 文件的扩展名为 .nupkg，且可以包含在多个目标框架和版本中使用的资产（如 .dll 文件和 .xml 文件）。 在应用或库中安装时，会根据应用或库指定的目标框架选择相应的资产。 定义接口的资产位于 ref 文件夹，而定义实现的资产位于 lib文件夹。

## <a name="platform"></a>平台

操作系统以及运行它的硬件，例如 Windows、macOS、Linux、iOS 和 Android。

下面是在句子中使用的示例：

- “.NET Core 是一个跨平台 .NET 实现。”
- “PCL 配置文件代表 Microsoft 平台，而 .NET Standard 与平台无关。”

旧的 .NET 文档有时使用“.NET 平台”来表示一个 [ .NET 的实现](#implementation-of-net)或包括所有实现的 .NET [堆栈](#stack)。 这两种用法往往会与主（OS/硬件）含义混淆，因此我们要尽量避免这些用法。

“平台”在短语“开发人员平台”中有不同的含义，这里指的是提供用于生成和运行应用的工具和库的软件。 .NET 是跨平台的开放源代码开发人员平台，用于构建多种不同类型的应用程序。

## <a name="runtime"></a>Runtime — 运行时

通常是指用于托管程序的执行环境。 操作系统属于运行时环境，但不属于 .NET 运行时。 下面是此情况下 .NET 运行时的一些示例：

- 公共语言运行时 ([CLR](#clr))
- .NET Native（适用于 UWP）
- Mono 运行时

“运行时”一词在某些上下文中有不同的含义：

* [.NET 5.0 下载页](https://dotnet.microsoft.com/download/dotnet/5.0)上的 .NET 运行时。

  可下载 .NET 运行时或其他运行时，如 ASP.NET Core 运行时 。 在此用法中运行时是一组必须安装在计算机上的组件，这样才能在计算机上运行[依赖于框架](../core/deploying/index.md#publish-framework-dependent)的应用。 .NET 运行时包括 [CLR](#clr) 和 .NET [共享框架](#shared-framework)，后者提供 [BCL](#bcl)。

* .NET 运行时库

  指 [BCL](#bcl) 引用的相同的库。 但是，其他运行时（例如 ASP.NET Core 运行时）具有不同的[共享框架](#shared-framework)，并具有在 BCL 上构建的其他库。

* [运行时标识符 (RID)](../core/rid-catalog.md)。

  此处的“运行时”表示运行 .NET 应用的 OS 平台和 CPU 体系结构，例如：`linux-x64`。

* 有时在 [.NET 实现](#implementation-of-net)的意义上使用“运行时”，如以下示例中所示：

  - “各种 .NET 运行时实现特定版本的 .NET Standard。 … 每个 .NET 运行时版本都将会公布它所支持的最高 .NET Standard 版本...”
  - “计划在多个运行时上运行的库应将此框架作为目标。” （参阅 .NET Standard）

## <a name="shared-framework"></a>共享框架

含义取决于上下文。 .NET 共享框架指 [.NET 运行时](#runtime)中包含的库。 在这种情况下，适用于 [.NET 5（和 .NET Core）及更高版本](#net-5-and-later-versions)的共享框架指 [BCL](#bcl) 引用的相同的库。

也存在其他共享框架。 ASP.NET Core 共享框架指 [ASP.NET Core 运行时](#runtime)中包含的库，其中包括 BCL 以及供 Web 应用使用的其他 API。

对于[依赖于框架的应用](../core/deploying/index.md#publish-framework-dependent)，共享框架由库组成，这些库包含在运行应用的计算机上的文件夹中的程序集内。 对于[自包含应用](../core/deploying/index.md#publish-self-contained)，共享框架程序集包含在该应用中。

有关详细信息，请参阅[深入了解 .NET Core 基元，第 2 部分：共享框架](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/)。

## <a name="stack"></a>堆栈

一组编程方法，一起用于生成并运行应用程序。

“.NET 堆栈”指 .NET Standard 和所有 .NET 实现。 短语“一个 .NET 堆栈”可能指一种 .NET 实现。

## <a name="target-framework"></a>Target Framework — 目标 Framework

.NET 应用或库依赖的 API 集合。

应用或库可将某版本的 .NET Standard（例如 .NET Standard 2.0）作为目标，这是所有 .NET 实现中一组标准化 API 的规范。 应用或库还能以特定 .NET 的某版本实现为目标，这样便可获得特定于实现的 API 的访问权限。 例如，面向 Xamarin.iOS 的应用有权访问 Xamarin 提供的 iOS API 包装器。

对于某些目标框架（例如 .NET Framework），可用 API 由 .NET 实现在系统上安装的程序集定义，其中可能包括应用程序框架 API（例如 ASP.NET、WinForms）。 对于基于包的目标框架（例如 .NET Standard 和 .NET Core），框架 API 由安装在应用或库中的包定义。 在这种情况下，目标框架隐式指定一个包，该包引用一起构成框架的所有包。

请参阅[目标框架](frameworks.md)。

## <a name="tfm"></a>TFM

目标框架名字对象。

一个标准化令牌格式，用于指定 .NET 应用或库的目标框架。 目标框架通常由短名称（如 `net462`）引用。 存在长格式的 TFM（如 .NETFramework,Version=4.6.2），但通常不用来指定目标框架。

请参阅[目标框架](frameworks.md)。

## <a name="uwp"></a>UWP

通用 Windows 平台。

用于为物联网 (IoT) 生成新式触控 Windows 应用程序和软件的 .NET 实现。 它旨在统一可能想要以其为目标的不同类型的设备，包括电脑、平板电脑、电话，甚至 Xbox。 UWP 提供许多服务，如集中式应用商店、执行环境 (AppContainer) 和一组 Windows API（用于代替 Win32 (WinRT)）。 应用可采用 C++、C#、Visual Basic 和 JavaScript 编写。 使用 C# 和 Visual Basic 时，.NET API 由 .NET 5（和 .NET Core）及更高版本提供。

## <a name="workload"></a>workload

某人正在构建的一种类型的应用。 比[应用模型](#app-model)更通用。 例如，在每个 .NET 文档页（包括此页）的顶部都有一个“工作负载”下拉列表，你可以通过该列表切换到针对“Web”、“移动”、“云”、“桌面”和“机器学习 \& 数据”的文档     。

在某些上下文中，工作负载是指 Visual Studio 功能的集合，可以选择安装这些功能以支持特定类型的应用。 有关示例，请参阅[选择工作负载](../core/install/windows.md#select-a-workload)。

## <a name="see-also"></a>请参阅

- [.NET 基础知识](../fundamentals/index.yml)
- [.NET Framework 指南](../framework/index.yml)
- [ASP.NET 概述](/aspnet/index#pivot=aspnet)
- [ 概述](/aspnet/index#pivot=core)
