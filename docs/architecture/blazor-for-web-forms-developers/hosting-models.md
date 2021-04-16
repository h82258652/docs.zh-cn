---
title: Blazor 应用托管模型
description: 了解在 Blazor 或服务器上的浏览中托管 WebAssembly 应用的不同方法。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: 18258c54cffdc538ddd11ec1393639216f86f85a
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102255452"
---
# <a name="blazor-app-hosting-models"></a>Blazor 应用托管模型

Blazor 应用可以通过以下方式托管：

- WebAssembly 上浏览器中的客户端。
- ASP.NET Core 应用中的服务器端。

## <a name="blazor-webassembly-apps"></a>Blazor WebAssembly 应用

Blazor WebAssembly 应用使用基于 WebAssembly 的 .NET 运行时在浏览器中直接执行。 Blazor WebAssembly 应用的工作方式类似于 Angular 和 React 等前端 JavaScript 框架。 但不是编写 JavaScript，而是编写 C#。 .NET 运行时与应用、应用程序集和任何必需的依赖项一起下载。 无需浏览器插件和扩展。

下载的程序集是普通的 .NET 程序集，就像在任何其他 .NET 应用中使用的程序集一样。 运行时支持 .NET Standard，因此可将现有的 .NET Standard 用于 Blazor WebAssembly 应用。 但这些程序集仍在浏览器安全沙盒中执行。 某些功能可能会引发 <xref:System.PlatformNotSupportedException>，如尝试访问文件系统或打开任意网络连接。

应用加载时，.NET 运行时会启动并指向应用程序集。 应用启动逻辑会运行，并且根组件会呈现。 Blazor 根据组件呈现的输出计算 UI 更新。 然后应用 DOM 更新。

![Blazor WebAssembly](media/hosting-models/blazor-webassembly.png)

Blazor WebAssembly 应用完全在客户端运行。 此类应用可以部署到 GitHub 网页或 Azure 静态网站托管等静态网站托管解决方案。 在服务器上根本不需要 .NET。 到应用的某些部分的深层链接通常需要服务器上的路由解决方案。 路由解决方案会将请求重定向到应用的根。 例如，可以通过使用 URL 重写 IIS 中的规则来处理此重定向。

要利用 Blazor 和全栈 .NET Web 开发的所有优势，请使用 ASP.NET Core 托管 Blazor WebAssembly 应用。 通过使用客户端和服务器上的 .NET，你可以轻松共享代码，并使用一组一致的语言、框架和工具生成应用。 Blazor 提供便捷的模板，可用于设置包含 Blazor WebAssembly 应用和 ASP.NET Core 托管项目的解决方案。 生成解决方案后，Blazor 应用中已生成的静态文件由已设置回退路由的 ASP.NET Core 应用托管。

## <a name="blazor-server-apps"></a>Blazor Server 应用

回想一下对 [Blazor 体系结构](architecture-comparison.md#blazor)的讨论，Blazor 组件会将其输出呈现为名为 `RenderTree` 的中间抽象。 Blazor 框架然后将呈现的内容与之前呈现的内容相比较。 这些差异会应用到 DOM。 Blazor 组件根据呈现的输出的应用方式进行去耦。 因此组件本身运行的进程无需与更新 UI 的进程相同。 实际上，它们甚至不必在同一计算机上运行。

在 Blazor Server 应用中，组件在服务器上运行，而不是在浏览器中的客户端运行。 浏览器中发生的 UI 事件通过实时连接发送到服务器。 事件被分派到正确的组件实例。 组件将呈现，计算的 UI 差异被序列化后发送到浏览器并在其中应用于 DOM。

![Blazor Server](media/hosting-models/blazor-server.png)

如果你使用过 ASP.NET AJAX 和 Blazor 控件，可能不会对 <xref:System.Web.UI.UpdatePanel> Server 托管模型感到陌生。 `UpdatePanel` 控件负责应用部分页面更新以响应页面上的触发事件。 触发时，`UpdatePanel` 会请求部分更新，然后应用该更新，无需刷新页面。 UI 状态使用 `ViewState` 进行管理。 Blazor Server 应用略有不同，因为该应用需要与客户端建立活动连接。 此外，服务器上的所有 UI 状态都需要进行维护。 除了这些差异之外，这两个模型在概念上类似。

## <a name="how-to-choose-the-right-blazor-hosting-model"></a>如何选择正确的 Blazor 托管模型

正如 [Blazor 托管模型文档](/aspnet/core/blazor/hosting-models)中所述，不同的 Blazor 托管模型具有不同的优缺点。

Blazor WebAssembly 托管模型具有以下优点：

- 没有 .NET 服务器端依赖项。 应用下载到客户端后即可正常运行。
- 可充分利用客户端资源和功能。
- 工作可从服务器转移到客户端。
- 无需 ASP.NET Core Web 服务器即可托管应用。 无服务器部署方案可行（例如通过 CDN 为应用提供服务的方案）。

Blazor WebAssembly 托管模型具有以下缺点：

- 浏览器功能限制了该应用。
- 需要客户端硬件和软件支持（例如 WebAssembly 支持）。
- 下载项大小较大，应用加载耗时较长。
- .NET 运行时和工具支持不够完善。 例如，[.NET Standard](../../standard/net-standard.md) 支持和调试方面存在限制。

相反，Blazor Server 托管模型具有以下优点：

- 下载大小明显小于客户端应用，并且应用的加载速度更快。
- 应用可以充分利用服务器功能，包括使用任何与 .NET 兼容的 API。
- 服务器上的 .NET 用于运行应用，因此调试等现有 .NET 工具可按预期正常工作。
- 支持瘦客户端。 例如，服务器端的应用适用于不支持 WebAssembly 以及资源受约束的设备上的浏览器。
- 应用的 .NET/C# 代码库（其中包括应用的组件代码）不适用于客户端。

Blazor Server 托管模型具有以下缺点：

- UI 延迟更高。 每次用户交互都涉及到网络跃点。
- 不支持脱机工作。 如果客户端连接失败，应用会停止工作。
- 如果具有多名用户，则应用扩缩性存在挑战。 服务器必须管理多个客户端连接并处理客户端状态。
- 需要 ASP.NET Core 服务器为应用提供服务。 不支持无服务器部署方案。 例如，无法从 CDN 提供应用。

上述优缺点列表可能会让你顾虑重重，但你可在以后更改托管模型。 无论是否选择 Blazor 托管模型，组件模型都是相同的。 原则上，相同组件可用于任一托管模型。 应用代码不会更改，但最佳做法是引入抽象，使组件与托管模型无关。 抽象使应用能够更轻松地采用不同的托管模型。

## <a name="deploy-your-app"></a>部署你的应用

ASP.NET Web Forms 应用通常在 Windows Server 计算机或群集的 IIS 上托管。 Blazor 应用还可以：

- 作为静态文件或 ASP.NET Core 应用托管在 IIS 上。
- 利用 ASP.NET Core 的灵活性托管在各种平台和服务器基础结构上。 例如，可使用 Linux 上的 [Nginx](/aspnet/core/host-and-deploy/linux-nginx) 或 [Apache](/aspnet/core/host-and-deploy/linux-apache) 托管 Blazor 应用。 有关如何发布和部署 Blazor 应用的详细信息，请参阅 Blazor [托管和部署](/aspnet/core/host-and-deploy/blazor/)文档。

在下一部分中，我们将介绍如何设置 Blazor WebAssembly 和 Blazor Server 应用的项目。

>[!div class="step-by-step"]
>[上一页](architecture-comparison.md)
>[下一页](project-structure.md)
