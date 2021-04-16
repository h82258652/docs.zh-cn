---
title: 状态管理
description: 了解管理 ASP.NET Web Forms 和 Blazor 的状态的不同方法。
author: csharpfritz
ms.author: jefritz
ms.date: 05/15/2020
ms.openlocfilehash: 2ca811f886c6a245ac16c2bd66a68ff16e5bfc44
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "88267719"
---
# <a name="state-management"></a>状态管理

状态管理是 Web Forms 应用程序的关键概念，通过视图状态、会话状态、应用程序状态和回发等功能实现。 框架的这些有状态功能有助于隐藏应用程序所需的状态管理，并使应用程序开发人员能够专注于交付其功能。 在 ASP.NET Core 和 Blazor 中，这些功能有一些已重新定位，有一些已彻底删除。 本章介绍如何维护状态，并提供与 Blazor 的新功能相同的功能。

## <a name="request-state-management-with-viewstate"></a>通过 ViewState 请求状态管理

讨论 Web Forms 应用程序中的状态管理时，许多开发人员会立即想到 ViewState。 在 Web Forms 中，ViewState 通过向浏览器来回发送大量经过编码的文本，来管理 HTTP 请求之间的内容状态。 ViewState 字段可能会被包含许多元素的页面的内容占用，从而可能扩展到几兆字节。

通过 Blazor Server，应用可以维护与服务器的持续连接。 应用的状态（称为线路）会在连接处于活动状态时保留在服务器内存中。 仅在用户离开应用或应用的特定页面时才会释放状态。 活动组件的所有成员在与服务器的交互之间均可用。

此功能有以下优点：

- 组件状态随时可用并且不在交互之间重新生成。
- 状态不会传输到浏览器。

但需要注意内存中组件状态持久性的以下缺点：

- 如果服务器在请求之间重启，则状态将丢失。
- 应用程序 Web 服务器负载均衡解决方案必须包含粘滞会话，确保来自相同浏览器的所有请求都返回到相同的服务器。 如果请求发送到其他服务器，则状态将丢失。
- 服务器上的组件状态的持久性可能导致 Web 服务器上产生内存压力。

出于上述原因，请勿仅依赖驻留在服务器内存中的组件状态。 应用程序还应为请求之间的数据提供一些后备数据存储。 下面是有关此策略的一些简单示例：

- 在购物车应用程序中，将添加到购物车中的新项的内容保留在数据库记录中。 如果服务器的状态丢失，可以根据数据库记录重建该状态。
- 在多部分的 Web 窗体中，用户希望应用程序记住每个请求之间的值。 将用户每个帖子之间的数据写入数据存储区，以便在多部分窗体完成时，可以将这些数据提取并组合到最终的窗体响应结构中。

有关管理 Blazor 应用状态的更多详细信息，请参阅 [ASP.NET Core Blazor 状态管理](/aspnet/core/blazor/state-management)。

## <a name="maintain-state-with-session"></a>通过会话维护状态

Web Forms 开发人员可以通过 <xref:Microsoft.AspNetCore.Http.ISession?displayProperty=nameWithType> 字典对象维护当前操作用户的信息。 将带有字符串键的对象添加到 `Session` 非常简单，并且该对象稍后在用户与应用程序的交互过程中可用。 在尝试消除与 HTTP 的管理交互的过程中，通过 `Session` 对象可以轻松维护状态。

.NET Framework `Session` 对象的签名与 ASP.NET Core `Session` 对象的签名不同。 决定迁移并使用新会话状态功能之前，请参阅[新 ASP.NET Core 会话的文档](/dotnet/api/microsoft.aspnetcore.http.isession)。

会话在 ASP.NET Core 和 Blazor Server 中可用，但为了将数据适当存储在数据存储库中，不建议使用。 如果访问者出于隐私考虑而拒绝使用应用程序中的 HTTP cookie，则会话状态也不起作用。

有关 ASP.NET Core 和会话状态的配置，请参阅 [ASP.NET Core 中的会话和状态管理](/aspnet/core/fundamentals/app-state#session-state)一文。

## <a name="application-state"></a>应用程序状态

Web Forms 框架中的 `Application` 对象为与应用程序范围的配置和状态进行交互提供了一个庞大的跨请求存储库。 应用程序状态是存储各种应用程序配置属性的理想位置，这些属性由所有请求引用，而无论用户是否发出请求。 `Application` 对象的问题在于，数据不会在多个服务器之间保持不变。 应用程序对象的状态会在重启间丢失。

对于 `Session`，建议将数据移动到多个服务器实例可以访问的永久性后备存储。 如果存在要跨请求和用户访问的易失性数据，可直接将其存储在单一服务中，该服务可注入到需要此信息或交互的组件中。

维护应用程序状态及其消耗的对象结构与以下实现类似：

```csharp
public class MyApplicationState
{
    public int VisitorCounter { get; private set; } = 0;

    public void IncrementCounter() => VisitorCounter += 1;
}
```

```csharp
app.AddSingleton<MyApplicationState>();
```

```razor
@inject MyApplicationState AppState

<label>Total Visitors: @AppState.VisitorCounter</label>
```

`MyApplicationState` 对象仅在服务器上创建一次，值 `VisitorCounter` 被提取后会输出到组件的标签中。 `VisitorCounter` 值应保留，并应从后备数据存储中进行检索以实现持久性和可伸缩性。

## <a name="in-the-browser"></a>在浏览器中

应用程序数据还可以存储在用户设备的客户端，以便以后可用。 可通过以下两种浏览器功能在用户浏览器的不同范围内实现数据持久性：

- `localStorage` - 范围是用户的整个浏览器。 如果页面被重新加载，浏览器将关闭并重新打开；如果使用相同的 URL 打开另一个选项卡，浏览器将提供相同的 `localStorage`
- `sessionStorage` - 范围为用户当前的浏览器选项卡。如果该选项卡被重载，状态将保持不变。 但是，如果用户打开应用程序的另一个选项卡，或者关闭并重新打开浏览器，则状态将丢失。

你可以写入一些与这些功能进行交互的自定义 JavaScript 代码，或者使用一些提供此功能的 NuGet 包。 例如 [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage) 包。

有关使用此包与 `localStorage` 和 `sessionStorage` 进行交互的说明，请参阅 [Blazor 状态管理](/aspnet/core/blazor/state-management#protected-browser-storage-experimental-package)一文。

>[!div class="step-by-step"]
>[上一页](pages-routing-layouts.md)
>[下一页](forms-validation.md)
