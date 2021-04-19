---
title: 中断性变更：中间件：HTTPS 重定向中间件会在 HTTPS 端口不明确时引发异常
description: 了解 ASP.NET Core 6.0 中的中断性变更，其标题为：中间件：HTTPS 重定向中间件会在 HTTPS 端口不明确时引发异常
ms.author: scaddie
ms.date: 02/04/2021
ms.openlocfilehash: e299fc7d2be295d723f5389a2b4cd6896078a752
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255072"
---
# <a name="middleware-https-redirection-middleware-throws-exception-on-ambiguous-https-ports"></a>中间件：HTTPS 重定向中间件会在 HTTPS 端口不明确时引发异常

在 ASP.NET Core 6.0 中，[HTTPS 重定向中间件](xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A)会在服务器配置中发现多个 HTTPS 端口时引发 <xref:System.InvalidOperationException> 类型的异常。 异常的消息包含文本“无法从 IServerAddressesFeature 确定 https 端口，找到了多个值。 请在 HttpsRedirectionOptions.HttpsPort 上显式设置所需的端口。”

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#29222](https://github.com/dotnet/aspnetcore/issues/29222)。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 6.0

## <a name="old-behavior"></a>旧行为

当 HTTPS 重定向中间件未显式配置端口时，它会在第一个请求期间搜索 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>，以确定它应该重定向到的 HTTPS 端口。

如果没有 HTTPS 端口或有多个不同的端口，那么应使用哪个端口是不明确的。 中间件会记录警告并禁用其自身。 HTTP 请求会被正常处理。

## <a name="new-behavior"></a>新行为

当 HTTPS 重定向中间件未显式配置端口时，它会在第一个请求期间搜索 `IServerAddressesFeature`，以确定它应该重定向到的 HTTPS 端口。

如果没有 HTTPS 端口，中间件仍将记录警告并禁用其自身。 HTTP 请求会被正常处理。 此行为支持：

* 不使用 HTTPS 的开发方案。
* TLS 在到达服务器之前已终止的托管方案。

如果有多个不同的端口，那么应使用哪个端口是不明确的。 中间件会引发异常，并导致 HTTP 请求失败。

## <a name="reason-for-change"></a>更改原因

当已知道 HTTPS 可用时，此更改可防止通过未加密的 HTTP 连接提供可能敏感的数据。

## <a name="recommended-action"></a>建议的操作

若要在服务器具有多个不同的 HTTPS 端口时启用 HTTPS 重定向，必须在配置中指定一个端口。 有关详细信息，请参阅[端口配置](/aspnet/core/security/enforcing-ssl?view=aspnetcore-5.0&preserve-view=true#port-configuration)。

如果应用中不需要 HTTPS 重定向中间件，请从 Startup.cs  中删除 `UseHttpsRedirection`。

如果需要动态选择正确的 HTTPS 端口，请在 GitHub 问题 [dotnet/aspnetcore#21291](https://github.com/dotnet/aspnetcore/issues/21291) 中提供反馈。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A?displayProperty=nameWithType>

<!--

## Category

ASP.NET Core

## Affected APIs

`Overload:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection`

-->
