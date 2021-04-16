---
title: 面向 ASP.NET Web Forms 开发人员的 Blazor 简介
description: 有关 Blazor 以及使用 .NET 编写全堆栈 Web 应用的简介
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: f1967dac0f46ba7cfefab62c5528dd1db8029514
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509710"
---
# <a name="an-introduction-to-blazor-for-aspnet-web-forms-developers"></a>面向 ASP.NET Web Forms 开发人员的 Blazor 简介

自从 .NET Framework 于 2002 年首次发布以来，ASP.NET Web Forms 框架已成为 .NET Web 开发的主要部分。 当初在 Web 大部分仍处于其初级阶段时，ASP.NET Web Forms 通过采用许多用于桌面开发的模式，使构建 Web 应用变得简单而高效。 在 ASP.NET Web Forms 中，可以通过可重复使用的 UI 控件快速编制网页。 用户交互作为事件自然地进行处理。 Microsoft 和控件供应商提供了丰富的 Web Forms UI 控件生态系统。 通过控件可轻松地连接到数据源并显示丰富的数据可视化效果。 对于有视觉倾向的用户，Web Forms 设计器提供了一个简单的拖放界面来管理控件。

多年来，Microsoft 一直在推出基于 ASP.NET 的新 Web 框架来应对 Web 开发趋势。 一些这样的 Web 框架包括 ASP.NET MVC、ASP.NET 网页和最近的 ASP.NET Core。 在每个新框架推出时，有些人都会预测 ASP.NET Web Forms 即将衰退，并批评它是一种陈旧、过时的 Web 框架。 尽管有这些预测，不过许多 .NET Web 开发人员仍然发现 ASP.NET Web Forms 是一种可完成工作的简单、稳定且高效的方式。

撰写本文时，每个月几乎有五十万个 Web 开发人员使用 ASP.NET Web Forms。 ASP.NET Web Forms 框架非常稳定，十年前的文档、示例、书籍和博客文章仍然有用且相关。 对于许多 .NET Web 开发人员而言，“ASP.NET”仍是“ASP.NET Web Forms”的同义词，正如最初构想 .NET 时一样。 在 ASP.NET Web Forms 相对于其他新 .NET Web 框架的的优缺点方面的争论可能会愈演愈烈。 ASP.NET Web Forms 仍是用于创建 Web 应用的热门框架。

尽管如此，软件开发方面的创新也并未放缓。 所有软件开发人员都需要及时了解新技术和趋势。 有两个趋势特别值得考虑：

1. 向开放源代码和跨平台的转变
2. 应用逻辑到客户端的转变

## <a name="an-open-source-and-cross-platform-net"></a>开放源代码和跨平台 .NET

当 .NET 和 ASP.NET Web Forms 首次发布时，平台生态系统看起来与当前大不相同。 桌面和服务器市场由 Windows 占主导地位。 其他平台（如 macOS 和 Linux）仍在努力获得吸引力。 ASP.NET Web Forms 作为仅限于 Windows 的组件随 .NET Framework 提供，这意味着 ASP.NET Web Forms 应用只能在 Windows Server 计算机上运行。 许多新式环境现在将不同种类的平台用于服务器和开发计算机，因此面向许多用户的跨平台支持是绝对必要的。

大多数新式 Web 框架现在还开放源代码，这有一些好处。 用户不需要依赖单个项目所有者来修复 bug 和添加功能。 开放源代码项目在开发进度和即将进行的更改方面提高了透明度。 开放源代码项目可受益于整个社区的贡献，培育了一种可提供支持的开放源代码生态系统。 尽管开放源代码存在风险，不过许多使用者和参与者已找到了合适的缓解措施，使他们能够以安全合理的方式享受开放源代码生态系统的好处。 此类缓解措施的示例包括参与者许可协议、友好许可证、出处扫描和支持基础。

.NET 社区采用了跨平台支持和开放源代码。 .NET Core 是 .NET 的开放源代码跨平台实现，它可在大量平台上运行，包括 Windows、macOS 和各种 Linux 发行版。 Xamarin 提供 Mono（.NET 的开放源代码版本）。 Mono 可在 Android、iOS 和各种其他外形规格设备（包括手表和智能电视）上运行。 Microsoft 发布了 [.NET 5](https://devblogs.microsoft.com/dotnet/announcing-net-5-0/)它将 .NET Core 和 Mono 整合为“可以在任何位置使用，并且具有统一运行时行为和开发人员体验的单个 .NET 运行时和框架”。

ASP.NET Web Forms 是否会受益于向开放源代码和跨平台支持的转变？ 很遗憾，答案是否定的，或者至少与平台的其余部分不同。 .NET 团队[最近明确表示](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/)，ASP.NET Web Forms 不会移植到 .NET Core 或 .NET 5。 为什么会这样？

在 .NET Core 早期阶段，曾尝试过移植 ASP.NET Web Forms。 结果发现所需的中断性变更数量过于庞大。 这里也需要承认，即使对于 Microsoft，它可以同时支持的 Web 框架数量也是有限的。 社区中的某人可能会着手创建 ASP.NET ASP.NET Web Forms 的开放源代码跨平台版本。 [ASP.NET Web Forms 的源代码](https://github.com/microsoft/referencesource)已经以参考窗体的形式公开提供。 但是目前看来，ASP.NET Web Forms 似乎会保持为仅限于 Windows，不会有开放源代码贡献模型。 如果跨平台支持或开放源代码对于你的方案非常重要，则需要查找新方法。

这是否意味着 ASP.NET Web Forms 已失去生命力，不应再使用？ 当然不是！ 只要 .NET Framework 作为 Windows 的一部分提供，ASP.NET Web Forms 便会是受支持的框架。 对于许多 Web 窗体开发人员而言，缺少跨平台和开放源代码支持不是什么问题。 如果不需要跨平台支持、开放源代码或是 .NET Core 或 .NET 5 中的任何其他新功能，那么在 Windows 上坚持使用 ASP.NET Web Forms 会很好。 在未来的许多年里，ASP.NET Web Forms 将继续是一种编写 Web 应用的高效方式。

不过还有一个趋势值得考虑，那便是向客户端的转变。

## <a name="client-side-web-development"></a>客户端 Web 开发

所有基于 .NET 的 Web 框架（包括 ASP.NET Web Forms）在历史上都有一个共同点：它们是服务器呈现的。 在服务器呈现的 Web 应用中，浏览器向服务器发出请求，而服务器会执行一些代码（ASP.NET 应用中的 .NET 代码）以生成响应。 该响应会发送回浏览器进行处理。 在此模型中，浏览器用作精简呈现引擎。 生成 UI、运行业务逻辑和管理状态的艰难工作在服务器上进行。

但是，浏览器已成为多功能平台。 它们实现了数量日益增多的开放 Web 标准，通过这些标准可以访问用户计算机的功能。 为什么不利用客户端设备的计算能力、存储、内存和其他资源呢？ 当至少部分或完全在客户端进行处理时，UI 交互尤其可以受益于更丰富且更具交互性的外观。 应在服务器上处理的逻辑和数据仍可以在服务器端处理。 可以使用 Web API 调用，甚至是实时协议（如 WebSocket）。 如果 Web 开发人员愿意编写 JavaScript，则可以免费获得这些好处。 客户端 UI 框架（如 Angular、React 和 Vue）可简化客户端 Web 开发，并且越来越流行。 ASP.NET Web Forms 开发人员也可以从利用客户端中受益，甚至可通过集成 JavaScript 框架（如 ASP.NET AJAX）获得一些现成的支持。

但是将两个不同的平台和生态系统（.NET 和 JavaScript）整合在一起是有代价的。 需要在语言、框架和工具不同的两个平行领域中具备专业知识。 代码和逻辑无法在客户端与服务器之间轻松共享，从而导致重复工作和工程开销。 跟上 JavaScript 生态系统的步伐也十分困难，该生态系统有着飞速发展的历史。 前端框架和生成工具首选项在快速变化。 业界见证了从 Grunt 到 Gulp，再到 Webpack 等的发展过程。 前端框架（如 jQuery、Knockout、Angular、React 和 Vue）出现了同样的动荡。 但考虑到 JavaScript 在浏览器领域的垄断地位，在这件事上别无选择。 这种情况一直持续到 Web 社区聚集在一起并引起奇迹的发生！

## <a name="webassembly-fulfills-a-need"></a>WebAssembly 满足了需求

2015 年，主要浏览器供应商共同组成了 W3C 社区小组，创造了一个新的开放 Web 标准，称为 WebAssembly。 WebAssembly 是 Web 的字节代码。 如果可以将代码编译为 WebAssembly，那么它可以在任何平台上的任何浏览器中以近乎本机的速度运行。 最初的工作集中在 C/C++ 上。 结果是获得了一个无需插件即可直接在浏览器中运行本机 3D 图形引擎的生动演示。 此后，WebAssembly 已由所有主要浏览器标准化并实现。

在 WebAssembly 上运行 .NET 的工作于 2017 年底宣布，并于 2020 年发布（包括来自 .NET 5 的支持）。 直接在浏览器中运行 .NET 代码的功能可实现通过 .NET 进行全堆栈 Web 开发。

## <a name="blazor-full-stack-web-development-with-net"></a>Blazor：通过 .NET 进行全堆栈 Web 开发

就其本身而言，能够在浏览器中运行 .NET 代码并不会为创建客户端 Web 应用提供端到端体验。 这便是 Blazor 的用武之地。 Blazor 是基于 C# 而不是 JavaScript 的客户端 Web UI 框架。 Blazor 可以通过 WebAssembly 直接在浏览器中运行。 无需任何浏览器插件。 或者，Blazor 应用可以在 .NET 上运行服务器端，并通过与浏览器的实时连接来处理所有用户交互。

Blazor 在 Visual Studio 和 Visual Studio Code 中具有很好的工具支持。 该框架还包含完整的 UI 组件模型并具有用于以下方面的内置工具：

- 窗体和验证
- 依赖关系注入
- 客户端路由
- 布局
- 浏览器内调试
- JavaScript 互操作

Blazor 与 ASP.NET Web Forms 有许多共同点。 两种框架都提供基于组件、事件驱动的有状态 UI 编程模型。 主要体系结构差异是 ASP.NET Web Forms 仅在服务器上运行。 Blazor 可以在客户端上的浏览器中运行。 但是如果你具有 ASP.NET Web Forms 北京，那么会对 Blazor 中的许多内容感到熟悉。 对于在寻找一种方法来利用客户端开发和 .NET 的开放源代码跨平台未来的 ASP.NET Web Forms 开发人员而言，Blazor 是一种天生的解决方案。

本书介绍了专门面向 ASP.NET Web Forms 开发人员的 Blazor。 每个 Blazor 概念都在具有类似 ASP.NET Web Forms 功能和实践的环境中进行展示。 本书结束时，你将了解：

- 如何生成 Blazor 应用。
- Blazor 如何工作。
- Blazor 如何与 .NET 相关。
- 在合适时将现有 ASP.NET Web Forms 应用迁移到 Blazor 的合理策略。

## <a name="get-started-with-blazor"></a>Blazor 入门

Blazor 入门十分简单。 请访问 <https://blazor.net> 并按照链接安装合适的 .NET SDK 和 Blazor 项目模板。 你还会找到有关在 Visual Studio 或 Visual Studio Code 中设置 Blazor 工具的说明。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](architecture-comparison.md)
