---
title: 中断性变更：SignalR：UseSignalR 和 UseConnections 方法已删除
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：SignalR：UseSignalR 和 UseConnections 方法已删除
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 3b6e301ee8414a503f7b3697d3b1a01174a1cefb
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497094"
---
# <a name="signalr-usesignalr-and-useconnections-methods-removed"></a>SignalR：UseSignalR 和 UseConnections 方法已删除

在 ASP.NET Core 3.0 中，SignalR 采用了终结点路由。 作为此更改的一部分，<xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A>、<xref:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections%2A> 和一些相关方法均被标记为过时。 在 ASP.NET Core 5.0 中，删除了那些已过时的方法。 有关方法的完整列表，请参阅 [受影响的 API](#affected-apis)。

有关此问题的讨论，请参阅 [dotnet/aspnetcore#20082](https://github.com/dotnet/aspnetcore/issues/20082)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 3

## <a name="old-behavior"></a>旧行为

可以使用 `UseSignalR` 或 `UseConnections` 方法在中间件管道中注册 SignalR 集线器和连接处理程序。

## <a name="new-behavior"></a>新行为

应使用 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder> 上的 <xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub%2A> 和 <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnectionHandler%2A> 扩展方法在 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> 中注册 SignalR 集线器和连接处理程序。

## <a name="reason-for-change"></a>更改原因

旧方法具有自定义路由逻辑，该逻辑不与 ASP.NET Core 中的其他路由组件交互。 在 ASP.NET Core 3.0 中，引入了一种新的通用路由系统，称为终结点路由。 借助终结点路由，SignalR 能够与其他路由组件进行交互。 切换到该模型可使用户获得终结点路由的全部优势。 因此，删除了旧方法。

## <a name="recommended-action"></a>建议操作

删除项目的 `Startup.Configure` 方法中调用 `UseSignalR` 或 `UseConnections` 的代码。 在调用 `UseEndpoints` 的主体中，分别将其替换为对 `MapHub` 或 `MapConnectionHandler` 的调用。 例如：

**旧代码：**

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<SomeHub>("/path");
});
```

**新代码：**

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<SomeHub>("/path");
});
```

通常，以前的 `MapHub` 和 `MapConnectionHandler` 调用几乎不需要更改即可从 `UseSignalR` 和 `UseConnections` 的主体直接转移到 `UseEndpoints`。

## <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnections%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnectionHandler%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections`
- `Overload:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR`
- `Overload:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnections`
- `Overload:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnectionHandler`
- `Overload:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub`

-->
