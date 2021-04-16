---
title: 适用于桌面的 .NET 有哪些新功能？
description: 了解 .NET、.NET 和 .NET Framework 之间的区别以及添加的新功能。
ms.date: 12/29/2020
ms.openlocfilehash: 03dea849ad8a797b917dca953d93be6878d7ec72
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102106176"
---
# <a name="whats-new-with-net-for-desktop"></a>适用于桌面的 .NET 有哪些新功能？

从 .NET Core 3.0 开始，.NET 支持 Windows 窗体和 WPF。 因此，对于桌面应用程序，现在可以在 .NET Framework 和 .NET 之间进行选择。 本章节将介绍什么是 .NET 及其对桌面应用程序有哪些好处。

## <a name="the-motivation-behind-net-core"></a>.NET Core 背后的动机

自 2002 年推出以来，.NET Framework 经过多年演变，可支持 Windows 窗体、ASP.NET、实体框架、Windows 应用商店等许多技术。 它们在本质上各不相同。 因此，Microsoft 通过采用 .NET Framework 的一部分并为每种技术创建不同的应用程序堆栈来实现这一演变。 这样便可以针对特定堆栈的需求自定义开发功能，从而最大程度地发挥每个平台的潜力。 这导致由不同的独立团队维护的 .NET Framework 版本呈碎片化趋势。 所有这些堆栈都有一个通用结构，其中包含一个应用模型、一个框架以及一个运行时，但它们在各个部分的实现上有所不同。

如果仅针对其中一个平台，则可以使用此模型。 但是，在许多情况下，同一解决方案可能需要针对多个目标平台。 例如，应用程序可能会有一个桌面管理部件，一个面向客户的网站（共享在服务器上运行的后端逻辑），甚至一个移动客户端。 在这种情况下，你需要能够跨越所有这些 .NET 垂直领域的统一编码体验。

在 Windows 8 发布时，可移植类库 (PCL) 的概念便应运而生。 最初，.NET Framework 根据其始终作为单个单元部署的假设进行设计，因此无需考虑[分解](https://wikipedia.org/wiki/Decomposition_(computer_science))的问题。 若要解决垂直领域之间的代码共享问题，驱动力在于如何重构框架。 协定的理念旨在提供构造良好的 API 外围应用。 协定是用作编译依据的程序集，并在设计时考虑了用于处理它们之间的依赖关系的适当分解技术。

这将导致在程序集级别（与之前的单个 API 级别相反）对垂直领域之间的 API 差异进行推理。 这一点实现了面向多个垂直领域的类库体验，也称为可移植类库。

![.NET Framework 版本历史记录](./media/whats-new-dotnet-core/release-history.png)

借助 PCL，可以根据 API 形状跨垂直领域统一开发体验。 还解决了创建在不同垂直领域运行的库的最迫切需求。 但是，存在一个巨大挑战，即仅当实现跨所有垂直领域向前移动时，API 才是可移植的。

更好的方法是通过提供构造良好的实现（而不是构造良好的视图）来统一跨垂直领域的实现。 让每个拥有特定组件的团队考虑自己的 API 如何跨所有垂直领域工作要比尝试在顶层以追溯方式提供一致的 API 堆栈要简单得多。 .NET Standard 可在此时派上用场。 有关详细信息，请参阅下一部分。

另一个重大挑战与 .NET Framework 的部署方式有关。 .NET Framework 是计算机范围的框架。 对其所做的任何更改都会影响依赖它的所有应用程序。 尽管此部署模型具有许多优点（例如减少占用的磁盘空间和集中访问服务），但也存在一些缺陷。

首先，应用程序开发人员很难依赖于最近发布的框架。 他们要么必须依赖于最新的操作系统，要么必须提供随应用程序安装 .NET Framework 的应用程序安装程序。 如果你是 Web 开发人员，随着 IT 部门确立服务器支持的版本，你甚至可能没有这个选项。

即使你愿意花费心思在 .NET Framework 安装中提供用于链接的安装程序，也可能会发现升级 .NET Framework 可能中断其他应用程序。

尽管已努力提供该框架的后向兼容版本，但仍有一些兼容性更改可能中断应用程序。 例如，将接口添加到现有类型可能更改此类型的序列化方式，并导致基于现有代码的中断问题。 因为 .NET Framework 的安装基础庞大，所以应对这些中断情况会减慢 .NET Framework 内部的创新步伐。

为了解决所有这些问题，Microsoft 开发了 .NET Core 来应对 .NET 平台的演变。

## <a name="introduction-to-net-core"></a>.NET Core 简介

.NET Core 是 Microsoft .NET 技术向模块化、跨平台、开源和云就绪平台的演变。 它可在 Windows、macOS 和 Linux 上运行，并且还计划在基于 ARM 的体系结构（如 Android 和 IoT）上运行。

.NET Core 旨在为所有类型的应用程序（包括 Windows、跨平台和移动应用程序）提供统一的平台。 [.NET Standard](../../standard/net-standard.md) 通过提供每个应用程序模型都需要的共享基础 API，并排除所有应用程序模型特定的 API 来实现此目的。

此框架在效率和性能方面为应用程序带来了很多好处，并简化了在不同受支持平台中的打包和部署。

.NET Core 带来的好处来自以下三个特征：

- **跨平台：** 它允许在不同平台（Windows、macOS 和 Linux）上执行应用程序。
- **开源：** .NET Core 平台为开源平台，可通过 GitHub 获取，促进了透明度提升和社区贡献。
- **受支持：** Microsoft 正式支持 .NET Core。

从 .NET Core 3.0 开始，除了对 Web 和云的现有支持之外，还支持桌面、IoT 和 AI 域。 此框架的目标令人印象深刻：面向现在和将来的所有类型的 .NET 开发。 Microsoft 计划在 2020 年底通过 .NET 5 完成这一愿景。 移除了名称中的“Core”，以加强其在 .NET 世界中的唯一性。

## <a name="net-5-is-net-core-vnext"></a>.NET 5 是 .NET Core vNext

.NET 5 是 .NET Core 要迈出的下一步。 .NET 5.0 旨在通过一些关键方法来改进 .NET：

- 生成可随处使用且提供统一运行时行为和开发人员体验的单一 .NET 运行时和框架。
- 通过充分利用 .NET Core、.NET Framework、Xamarin 和 Mono 来扩展 .NET 的功能。
- 根据单个基本代码构建开发人员（Microsoft 和社区）可处理且协同扩展，同时可改善所有方案的产品。

这一新版本和发展方向对 .NET 而言改变了游戏规则。 借助 .NET 5，无论你正在生成哪种类型的应用，代码和项目文件看起来都一样。 你将能够访问每个应用的相同运行时、API 和语言功能。 这包括几乎每天都会提交到运行时的新[性能改进](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5/)。 有关更多详细信息，请参阅 [.NET 5 中的新增功能](../../core/dotnet-five.md)。

![.NET 5 的所有域](./media/whats-new-dotnet-core/all-domains-of-dotnet5.png)

## <a name="net-framework-vs-net-5"></a>.NET Framework 与 .NET 5

你现在已了解 .NET 的相关性，接下来可能会想知道 .NET Framework 会发生什么情况。 你可能会问以下问题：必须放弃吗？ 它会消失吗？ 在现代化 .NET Framework 上的应用程序时，我有哪些选择？

2019 年，发布了最新版本的 .NET Framework，即版本 4.8。 它包括针对桌面应用程序的三项重大改进：

- **新式浏览器和媒体控件**：现今，.NET 桌面应用程序使用 Internet Explorer 和 Windows Media Player 来显示 HTML 及播放媒体文件。 由于这些旧控件不显示最新的 HTML，也不播放最新的媒体文件，因此添加了新控件，以利用 Microsoft Edge 和支持最新标准的较新媒体播放器。
- **UWP 控件访问权限**：UWP 包含可利用最新 Windows 功能和触摸屏的新控件。 无需重写应用程序即可使用这些新功能和控件，因此可以在现有的 WPF 或 Windows 窗体代码中使用这些新功能。
- **高 DPI 改进**：显示器的分辨率提高到 4K 和 8K 分辨率。 因此，.NET Framework 4.8 添加了新的 HDPI 改进，以确保现有 Windows 窗体和 WPF 应用程序在这些新显示器中看起来很棒。

由于 .NET Framework 已在数百万台计算机上安装，因此 Microsoft 将继续支持它，但不会添加新功能。

.NET 5 是 .NET 系列的开源、跨平台且快速演变的版本。 由于其并行性质，可以进行更改而不必担心中断任何应用程序。 这意味着，随着时间的推移，.NET 将获得新的 API 和语言功能，而 .NET Framework 不会。 此外，.NET 已经具有 .NET Framework 无法实现的功能，例如：

- **支持 Windows 窗体和 WPF 的 .NET 的并行版本**：这解决了更新计算机的框架版本的副作用问题。 可以在同一台计算机上安装多个 .NET 版本，并且每个应用程序都指定应使用的 .NET 版本。 现在甚至还可以在 .NET 之上开发和运行 Windows 窗体及 WPF。
- **将 .NET 直接嵌入应用程序**：可以将 .NET 作为应用程序包的一部分部署。 这使你可以利用最新版本、功能和 API，而不必等待在计算机上安装特定版本。
- **利用 .NET 功能**：.NET Core 是 .NET 的快速演变的开源版本。 其并行特性使得可以快速引入新的创新 API 和基类库 (BCL) 改进，而不会有破坏兼容性的风险。 现在，Windows 窗体和 WPF 应用程序可以利用最新的 .NET 功能，其还包括针对运行时性能、高 DPI 支持等的更多基本修补程序。

Microsoft 路线图的一个重要部分是简化开发人员将应用程序迁移到 .NET Core 以及稍后迁移到 .NET 5 的过程。 但是，如果已有现有的 .NET Framework 应用程序，则对于迁移到 .NET 5 不应感觉有压力。 .NET Framework 将完全受支持，并将始终是 Windows 的一部分。 但是，如果将来要使用最新的语言功能和 API，则需要将应用程序迁移到 .NET。

对于全新的桌面应用程序，我们建议直接从 .NET 5 入手。 它是轻量级的并且跨平台，可以并行运行，具有高性能，并且非常适合容器和微服务体系结构。

![可以使用最新的 .NET Framework 版本更新 .NET Framework 应用程序，也可以将应用程序移植到 .NET Core](./media/whats-new-dotnet-core/framework-vs-core.png)

## <a name="net-standard-vs-pcl"></a>.NET Standard 与 PCL

[.NET Standard](../../standard/net-standard.md) 是一套正式的 .NET API 规范，有望在所有 .NET 实现中推出。 推出 .NET Standard 的背后动机是要提高 .NET 生态系统中的一致性。 .NET Standard 是构成协定统一集（这些协定是编写代码的依据）的 .NET API 规范。 这些协定在每种 .NET 风格中实现，因此可实现跨不同 .NET 实现的可移植性。

.NET Standard 可实现以下重要情境：

- 为要实现的所有 .NET 实现定义与工作负载无关的基类库 API 统一集。
- 使开发人员能够通过同一组 API 生成可在各种 .NET 实现中使用的可移植库。

.NET Standard 是 PCL 的发展，下面的列表显示 .NET Standard 和 PCL 之间的基本区别：

- .NET Standard 是 Microsoft 挑选的一组特选 API。 PCL 不是。
- PCL 包含的 API 取决于创建时选择的目标平台。 这使得 PCL 仅可共享给选择的特定目标。
- .NET Standard 与平台无关，它可以在 Windows、macOS、Linux 等任何位置运行。
- PCL 也可以跨平台运行，但是范围受到更多限制。 PCL 只能面向一组有限的平台。

## <a name="new-desktop-features-in-net-core"></a>.NET Core 中的新桌面功能

### <a name="support-for-windows-forms-and-wpf"></a>Windows 窗体和 WPF 支持

自版本 3.0 以来，Windows 窗体和 WPF 成为 .NET Core 的一部分。 两种表示框架均仅适用于 Windows，因此它们不可跨平台。 你可以将 WPF 视为 DirectX 上的富层，将 Windows 窗体视为 GDI+ 上的瘦层。 WPF 和 Windows 窗体在公开和执行 Windows 中的许多桌面应用程序功能方面做得很好。 因此，Windows 窗体和 WPF 可用于 .NET Core 和 .NET Framework。 现在，你可以启动面向 .NET Core 的新桌面应用程序，并将现有桌面应用程序从 .NET Framework 迁移到 .NET Core。

.NET Standard 的新版本 2.1 版与 .NET Core 3.0 同时发布。 与预期一样，.NET Core 3.x 版本支持 .NET Standard 2.1 和更早版本。

另外，请务必注意，.NET Core 的 Windows 窗体和 WPF 实现都是开源的。

### <a name="xaml-islands"></a>XAML 岛

[XAML 岛](/windows/apps/desktop/modernize/xaml-islands)是一组组件，供开发人员在其当前的 WPF、Windows 窗体和本机 Win32 应用（如 MFC）中使用新的 Windows 10 控件（UWP XAML 控件）。 可以在 Win32 应用中的任何位置拥有 UWP XAML 控件“岛”。

可实现这些 XAML 岛是因为 Windows 10 版本 1903 引入了一组 API，这些 API 允许使用窗口处理程序 (HWnd) 在 Win32 窗口中托管 UWP XAML 内容。 请注意，仅在 Windows 10 1903 及更高版本上运行的应用才能使用 XAML 岛。

为了 Windows 窗体和 WPF 开发人员能够更轻松地创建 XAML 岛，Windows 社区工具包在多个 NuGet 包中引入了一组 .NET 包装器。 这些包装器是包装和托管控件：

- [WebView](/windows/communitytoolkit/controls/wpf-winforms/webview)、[WebViewCompatible](/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible)、[InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)、[MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) 和 [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) 包装控件将一些 UWP XAML 控件包装到 Windows 窗体或 WPF 控件中，为这些开发人员隐藏 UWP 概念。
- Windows 窗体和 WPF 的 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件允许其他未经包装的 UWP XAML 控件和自定义控件加载到 XAML 岛中。

### <a name="access-to-all-windows-10-apis"></a>所有 Windows 10 API 的访问权限

Windows 10 具有大量可供开发人员使用的 API。 借助这些 API 可以访问各种功能，如身份验证、蓝牙、约会和联系人。 现在，这些 API 通过 .NET Core 公开，并为 Windows 开发人员提供利用 Windows 10 上的功能创建功能强大的桌面应用的机会。

### <a name="side-by-side-support-and-self-contained-exes"></a>并行支持和自包含 EXE

.NET Core 部署模型是 Windows 桌面开发人员将通过 .NET Core 享有的最大好处之一。 全局安装 .NET Core 的能力提供与 .NET Framework 的集中安装和服务优势相同的好处，且无需就地更新。

发布新的 .NET Core 版本时，可以根据需要更新计算机上的每个应用，而不必担心会影响其他应用程序。 新的 .NET Core 版本安装在其自己的目录中，并且彼此“并行”存在。

如果需要隔离部署，则可以随应用程序一起部署 .NET Core。 .NET Core 会将应用与 .NET Core 运行时捆绑，就像在单个可执行文件中一样。

很长时间以来，开发人员一直在请求提供这些部署选项，但使用 .NET Framework 难以实现。 .NET Core 使用的模块化体系结构使这些灵活部署选项成为可能。

### <a name="performance"></a>性能

自发布以来，.NET Core 就面向 Web 和云工作负载，性能已刻入其 DNA 中。 服务器端代码必须具有足够的性能才能满足高并发方案的要求，.NET Core 如今已成为市面上性能优异的 Web 平台。

使用 .NET Core 生成下一代桌面应用程序时，可以利用这些性能改进。

## <a name="benefits-of-open-source"></a>开源的好处

再说说开源 .NET Core。 生成跨平台堆栈很复杂，需要每个目标平台上的专业团队进行交互。 这项工作需要 Microsoft 内部和外部进行大量协作。 通过使其开源并面向公共协作开放，你将获得最敏捷的开发风格，从而提高质量标准，因为问题将被庞大且活跃的开发人员社区发现。

这是 .NET Core 取得成功的关键因素，并将继续加快之前提到的路线图：成为任何开发人员生成任何应用程序将需要用到的单一 .NET 平台。

>[!div class="step-by-step"]
>[上一页](why-modern-applications.md)
>[下一页](migrate-modern-applications.md)
