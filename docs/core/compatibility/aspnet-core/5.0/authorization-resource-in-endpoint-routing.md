---
title: 中断性变更：授权：终结点路由中的资源为 HttpContext
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：授权：终结点路由中的资源为 HttpContext
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 90f15bcbc1fb05e67b1c6d7494938eebac7718fd
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497874"
---
# <a name="authorization-resource-in-endpoint-routing-is-httpcontext"></a>授权：终结点路由中的资源为 HttpContext

在 ASP.NET Core 3.1 中使用终结点路由时，用于授权的资源是终结点。 此方法不足以获取对路由数据 (<xref:Microsoft.AspNetCore.Routing.RouteData>) 的访问权限。 以前在 MVC 中，已传入 <xref:Microsoft.AspNetCore.Http.HttpContext> 资源，这允许访问终结点 (<xref:Microsoft.AspNetCore.Http.Endpoint>) 和路由数据。 此更改可确保传递给授权的资源始终是 `HttpContext`。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 7

## <a name="old-behavior"></a>旧行为

使用终结点路由和授权中间件 (<xref:Microsoft.AspNetCore.Authorization.AuthorizationMiddleware>) 或 [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 属性时，传递给授权的资源是匹配的终结点。

## <a name="new-behavior"></a>新行为

终结点路由将 `HttpContext` 传递给授权。

## <a name="reason-for-change"></a>更改原因

可以从 `HttpContext` 访问终结点。 但是，无法从终结点访问路由数据等内容。 非终结点路由导致了功能丢失。

## <a name="recommended-action"></a>建议操作

如果应用使用终结点资源，请对 `HttpContext` 调用 <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint%2A> 以继续访问终结点。

在 ASP.NET Core 5.0 预览版 8 及更高版本中，可以通过 <xref:System.AppContext.SetSwitch%2A> 还原到旧行为。 例如：

```csharp
AppContext.SetSwitch(
    "Microsoft.AspNetCore.Authorization.SuppressUseHttpContextAsAuthorizationResource",
    isEnabled: true);
```

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
