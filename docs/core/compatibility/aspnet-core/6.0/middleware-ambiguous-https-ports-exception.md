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
# <a name="middleware-https-redirection-middleware-throws-exception-on-ambiguous-https-ports"></a><span data-ttu-id="81ea0-103">中间件：HTTPS 重定向中间件会在 HTTPS 端口不明确时引发异常</span><span class="sxs-lookup"><span data-stu-id="81ea0-103">Middleware: HTTPS Redirection Middleware throws exception on ambiguous HTTPS ports</span></span>

<span data-ttu-id="81ea0-104">在 ASP.NET Core 6.0 中，[HTTPS 重定向中间件](xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A)会在服务器配置中发现多个 HTTPS 端口时引发 <xref:System.InvalidOperationException> 类型的异常。</span><span class="sxs-lookup"><span data-stu-id="81ea0-104">In ASP.NET Core 6.0, the [HTTPS Redirection Middleware](xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A) throws an exception of type <xref:System.InvalidOperationException> when it finds multiple HTTPS ports in the server configuration.</span></span> <span data-ttu-id="81ea0-105">异常的消息包含文本“无法从 IServerAddressesFeature 确定 https 端口，找到了多个值。</span><span class="sxs-lookup"><span data-stu-id="81ea0-105">The exception's message contains the text "Cannot determine the https port from IServerAddressesFeature, multiple values were found.</span></span> <span data-ttu-id="81ea0-106">请在 HttpsRedirectionOptions.HttpsPort 上显式设置所需的端口。”</span><span class="sxs-lookup"><span data-stu-id="81ea0-106">Set the desired port explicitly on HttpsRedirectionOptions.HttpsPort."</span></span>

<span data-ttu-id="81ea0-107">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#29222](https://github.com/dotnet/aspnetcore/issues/29222)。</span><span class="sxs-lookup"><span data-stu-id="81ea0-107">For discussion, see GitHub issue [dotnet/aspnetcore#29222](https://github.com/dotnet/aspnetcore/issues/29222).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="81ea0-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="81ea0-108">Version introduced</span></span>

<span data-ttu-id="81ea0-109">ASP.NET Core 6.0</span><span class="sxs-lookup"><span data-stu-id="81ea0-109">ASP.NET Core 6.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="81ea0-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="81ea0-110">Old behavior</span></span>

<span data-ttu-id="81ea0-111">当 HTTPS 重定向中间件未显式配置端口时，它会在第一个请求期间搜索 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>，以确定它应该重定向到的 HTTPS 端口。</span><span class="sxs-lookup"><span data-stu-id="81ea0-111">When the HTTPS Redirection Middleware isn't explicitly configured with a port, it searches <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> during the first request to determine the HTTPS port to which it should redirect.</span></span>

<span data-ttu-id="81ea0-112">如果没有 HTTPS 端口或有多个不同的端口，那么应使用哪个端口是不明确的。</span><span class="sxs-lookup"><span data-stu-id="81ea0-112">If there are no HTTPS ports or multiple distinct ports, it's unclear which port should be used.</span></span> <span data-ttu-id="81ea0-113">中间件会记录警告并禁用其自身。</span><span class="sxs-lookup"><span data-stu-id="81ea0-113">The middleware logs a warning and disables itself.</span></span> <span data-ttu-id="81ea0-114">HTTP 请求会被正常处理。</span><span class="sxs-lookup"><span data-stu-id="81ea0-114">HTTP requests are processed normally.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="81ea0-115">新行为</span><span class="sxs-lookup"><span data-stu-id="81ea0-115">New behavior</span></span>

<span data-ttu-id="81ea0-116">当 HTTPS 重定向中间件未显式配置端口时，它会在第一个请求期间搜索 `IServerAddressesFeature`，以确定它应该重定向到的 HTTPS 端口。</span><span class="sxs-lookup"><span data-stu-id="81ea0-116">When the HTTPS Redirection Middleware isn't explicitly configured with a port, it searches `IServerAddressesFeature` during the first request to determine the HTTPS port to which it should redirect.</span></span>

<span data-ttu-id="81ea0-117">如果没有 HTTPS 端口，中间件仍将记录警告并禁用其自身。</span><span class="sxs-lookup"><span data-stu-id="81ea0-117">If there are no HTTPS ports, the middleware still logs a warning and disables itself.</span></span> <span data-ttu-id="81ea0-118">HTTP 请求会被正常处理。</span><span class="sxs-lookup"><span data-stu-id="81ea0-118">HTTP requests are processed normally.</span></span> <span data-ttu-id="81ea0-119">此行为支持：</span><span class="sxs-lookup"><span data-stu-id="81ea0-119">This behavior supports:</span></span>

* <span data-ttu-id="81ea0-120">不使用 HTTPS 的开发方案。</span><span class="sxs-lookup"><span data-stu-id="81ea0-120">Development scenarios without HTTPS.</span></span>
* <span data-ttu-id="81ea0-121">TLS 在到达服务器之前已终止的托管方案。</span><span class="sxs-lookup"><span data-stu-id="81ea0-121">Hosted scenarios in which TLS is terminated before reaching the server.</span></span>

<span data-ttu-id="81ea0-122">如果有多个不同的端口，那么应使用哪个端口是不明确的。</span><span class="sxs-lookup"><span data-stu-id="81ea0-122">If there are multiple distinct ports, it's unclear which port should be used.</span></span> <span data-ttu-id="81ea0-123">中间件会引发异常，并导致 HTTP 请求失败。</span><span class="sxs-lookup"><span data-stu-id="81ea0-123">The middleware throws an exception and fails the HTTP request.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="81ea0-124">更改原因</span><span class="sxs-lookup"><span data-stu-id="81ea0-124">Reason for change</span></span>

<span data-ttu-id="81ea0-125">当已知道 HTTPS 可用时，此更改可防止通过未加密的 HTTP 连接提供可能敏感的数据。</span><span class="sxs-lookup"><span data-stu-id="81ea0-125">This change prevents potentially sensitive data from being served over unencrypted HTTP connections when HTTPS is known to be available.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="81ea0-126">建议的操作</span><span class="sxs-lookup"><span data-stu-id="81ea0-126">Recommended action</span></span>

<span data-ttu-id="81ea0-127">若要在服务器具有多个不同的 HTTPS 端口时启用 HTTPS 重定向，必须在配置中指定一个端口。</span><span class="sxs-lookup"><span data-stu-id="81ea0-127">To enable HTTPS redirection when the server has multiple distinct HTTPS ports, you must specify one port in the configuration.</span></span> <span data-ttu-id="81ea0-128">有关详细信息，请参阅[端口配置](/aspnet/core/security/enforcing-ssl?view=aspnetcore-5.0&preserve-view=true#port-configuration)。</span><span class="sxs-lookup"><span data-stu-id="81ea0-128">For more information, see [Port configuration](/aspnet/core/security/enforcing-ssl?view=aspnetcore-5.0&preserve-view=true#port-configuration).</span></span>

<span data-ttu-id="81ea0-129">如果应用中不需要 HTTPS 重定向中间件，请从 Startup.cs  中删除 `UseHttpsRedirection`。</span><span class="sxs-lookup"><span data-stu-id="81ea0-129">If you don't need the HTTPS Redirection Middleware in your app, remove `UseHttpsRedirection` from *Startup.cs*.</span></span>

<span data-ttu-id="81ea0-130">如果需要动态选择正确的 HTTPS 端口，请在 GitHub 问题 [dotnet/aspnetcore#21291](https://github.com/dotnet/aspnetcore/issues/21291) 中提供反馈。</span><span class="sxs-lookup"><span data-stu-id="81ea0-130">If you need to select the correct HTTPS port dynamically, provide feedback in GitHub issue [dotnet/aspnetcore#21291](https://github.com/dotnet/aspnetcore/issues/21291).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="81ea0-131">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="81ea0-131">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A?displayProperty=nameWithType>

<!--

## Category

ASP.NET Core

## Affected APIs

`Overload:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection`

-->
