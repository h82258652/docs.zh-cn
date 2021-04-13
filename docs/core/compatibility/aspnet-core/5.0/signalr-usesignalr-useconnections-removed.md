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
# <a name="signalr-usesignalr-and-useconnections-methods-removed"></a><span data-ttu-id="339b3-103">SignalR：UseSignalR 和 UseConnections 方法已删除</span><span class="sxs-lookup"><span data-stu-id="339b3-103">SignalR: UseSignalR and UseConnections methods removed</span></span>

<span data-ttu-id="339b3-104">在 ASP.NET Core 3.0 中，SignalR 采用了终结点路由。</span><span class="sxs-lookup"><span data-stu-id="339b3-104">In ASP.NET Core 3.0, SignalR adopted endpoint routing.</span></span> <span data-ttu-id="339b3-105">作为此更改的一部分，<xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A>、<xref:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections%2A> 和一些相关方法均被标记为过时。</span><span class="sxs-lookup"><span data-stu-id="339b3-105">As part of that change, the <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A>, <xref:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections%2A>, and some related methods were marked as obsolete.</span></span> <span data-ttu-id="339b3-106">在 ASP.NET Core 5.0 中，删除了那些已过时的方法。</span><span class="sxs-lookup"><span data-stu-id="339b3-106">In ASP.NET Core 5.0, those obsolete methods were removed.</span></span> <span data-ttu-id="339b3-107">有关方法的完整列表，请参阅 [受影响的 API](#affected-apis)。</span><span class="sxs-lookup"><span data-stu-id="339b3-107">For the full list of methods, see [Affected APIs](#affected-apis).</span></span>

<span data-ttu-id="339b3-108">有关此问题的讨论，请参阅 [dotnet/aspnetcore#20082](https://github.com/dotnet/aspnetcore/issues/20082)。</span><span class="sxs-lookup"><span data-stu-id="339b3-108">For discussion on this issue, see [dotnet/aspnetcore#20082](https://github.com/dotnet/aspnetcore/issues/20082).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="339b3-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="339b3-109">Version introduced</span></span>

<span data-ttu-id="339b3-110">5.0 预览版 3</span><span class="sxs-lookup"><span data-stu-id="339b3-110">5.0 Preview 3</span></span>

## <a name="old-behavior"></a><span data-ttu-id="339b3-111">旧行为</span><span class="sxs-lookup"><span data-stu-id="339b3-111">Old behavior</span></span>

<span data-ttu-id="339b3-112">可以使用 `UseSignalR` 或 `UseConnections` 方法在中间件管道中注册 SignalR 集线器和连接处理程序。</span><span class="sxs-lookup"><span data-stu-id="339b3-112">SignalR hubs and connection handlers could be registered in the middleware pipeline using the `UseSignalR` or `UseConnections` methods.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="339b3-113">新行为</span><span class="sxs-lookup"><span data-stu-id="339b3-113">New behavior</span></span>

<span data-ttu-id="339b3-114">应使用 <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder> 上的 <xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub%2A> 和 <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnectionHandler%2A> 扩展方法在 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> 中注册 SignalR 集线器和连接处理程序。</span><span class="sxs-lookup"><span data-stu-id="339b3-114">SignalR hubs and connection handlers should be registered within <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> using the <xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder.MapHub%2A> and <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder.MapConnectionHandler%2A> extension methods on <xref:Microsoft.AspNetCore.Routing.IEndpointRouteBuilder>.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="339b3-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="339b3-115">Reason for change</span></span>

<span data-ttu-id="339b3-116">旧方法具有自定义路由逻辑，该逻辑不与 ASP.NET Core 中的其他路由组件交互。</span><span class="sxs-lookup"><span data-stu-id="339b3-116">The old methods had custom routing logic that didn't interact with other routing components in ASP.NET Core.</span></span> <span data-ttu-id="339b3-117">在 ASP.NET Core 3.0 中，引入了一种新的通用路由系统，称为终结点路由。</span><span class="sxs-lookup"><span data-stu-id="339b3-117">In ASP.NET Core 3.0, a new general-purpose routing system, called endpoint routing, was introduced.</span></span> <span data-ttu-id="339b3-118">借助终结点路由，SignalR 能够与其他路由组件进行交互。</span><span class="sxs-lookup"><span data-stu-id="339b3-118">Endpoint routing enabled SignalR to interact with other routing components.</span></span> <span data-ttu-id="339b3-119">切换到该模型可使用户获得终结点路由的全部优势。</span><span class="sxs-lookup"><span data-stu-id="339b3-119">Switching to this model allows users to realize the full benefits of endpoint routing.</span></span> <span data-ttu-id="339b3-120">因此，删除了旧方法。</span><span class="sxs-lookup"><span data-stu-id="339b3-120">Consequently, the old methods have been removed.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="339b3-121">建议操作</span><span class="sxs-lookup"><span data-stu-id="339b3-121">Recommended action</span></span>

<span data-ttu-id="339b3-122">删除项目的 `Startup.Configure` 方法中调用 `UseSignalR` 或 `UseConnections` 的代码。</span><span class="sxs-lookup"><span data-stu-id="339b3-122">Remove code that calls `UseSignalR` or `UseConnections` from your project's `Startup.Configure` method.</span></span> <span data-ttu-id="339b3-123">在调用 `UseEndpoints` 的主体中，分别将其替换为对 `MapHub` 或 `MapConnectionHandler` 的调用。</span><span class="sxs-lookup"><span data-stu-id="339b3-123">Replace it with calls to `MapHub` or `MapConnectionHandler`, respectively, within the body of a call to `UseEndpoints`.</span></span> <span data-ttu-id="339b3-124">例如：</span><span class="sxs-lookup"><span data-stu-id="339b3-124">For example:</span></span>

<span data-ttu-id="339b3-125">**旧代码：**</span><span class="sxs-lookup"><span data-stu-id="339b3-125">**Old code:**</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<SomeHub>("/path");
});
```

<span data-ttu-id="339b3-126">**新代码：**</span><span class="sxs-lookup"><span data-stu-id="339b3-126">**New code:**</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<SomeHub>("/path");
});
```

<span data-ttu-id="339b3-127">通常，以前的 `MapHub` 和 `MapConnectionHandler` 调用几乎不需要更改即可从 `UseSignalR` 和 `UseConnections` 的主体直接转移到 `UseEndpoints`。</span><span class="sxs-lookup"><span data-stu-id="339b3-127">In general, your previous `MapHub` and `MapConnectionHandler` calls can be transferred directly from the body of `UseSignalR` and `UseConnections` to `UseEndpoints` with little-to-no change needed.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="339b3-128">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="339b3-128">Affected APIs</span></span>

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
