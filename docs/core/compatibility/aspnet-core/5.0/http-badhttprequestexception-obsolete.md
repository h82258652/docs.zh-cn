---
title: 中断性变更：HTTP：Kestrel 和 IIS BadHttpRequestException 类型标记为已过时并已替换
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：HTTP：Kestrel 和 IIS BadHttpRequestException 类型标记为已过时并已替换
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 5837018def634b732b4f273f1a794267f4d17972
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498134"
---
# <a name="http-kestrel-and-iis-badhttprequestexception-types-marked-obsolete-and-replaced"></a><span data-ttu-id="0ff84-103">HTTP：Kestrel 和 IIS BadHttpRequestException 类型标记为已过时并已替换</span><span class="sxs-lookup"><span data-stu-id="0ff84-103">HTTP: Kestrel and IIS BadHttpRequestException types marked obsolete and replaced</span></span>

<span data-ttu-id="0ff84-104">`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 已标记为已过时，并已更改为从 `Microsoft.AspNetCore.Http.BadHttpRequestException` 派生。</span><span class="sxs-lookup"><span data-stu-id="0ff84-104">`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` and `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` have been marked obsolete and changed to derive from `Microsoft.AspNetCore.Http.BadHttpRequestException`.</span></span> <span data-ttu-id="0ff84-105">为了实现后向兼容性，Kestrel 和 IIS 服务器仍会引发旧的异常类型。</span><span class="sxs-lookup"><span data-stu-id="0ff84-105">The Kestrel and IIS servers still throw their old exception types for backwards compatibility.</span></span> <span data-ttu-id="0ff84-106">未来版本将删除这些过时的类型。</span><span class="sxs-lookup"><span data-stu-id="0ff84-106">The obsolete types will be removed in a future release.</span></span>

<span data-ttu-id="0ff84-107">有关讨论，请参阅 [dotnet/aspnetcore#20614](https://github.com/dotnet/aspnetcore/issues/20614)。</span><span class="sxs-lookup"><span data-stu-id="0ff84-107">For discussion, see [dotnet/aspnetcore#20614](https://github.com/dotnet/aspnetcore/issues/20614).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="0ff84-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="0ff84-108">Version introduced</span></span>

<span data-ttu-id="0ff84-109">5.0 预览版 4</span><span class="sxs-lookup"><span data-stu-id="0ff84-109">5.0 Preview 4</span></span>

## <a name="old-behavior"></a><span data-ttu-id="0ff84-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="0ff84-110">Old behavior</span></span>

<span data-ttu-id="0ff84-111">`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 派生自 <xref:System.IO.IOException?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="0ff84-111">`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` and `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` derived from <xref:System.IO.IOException?displayProperty=nameWithType>.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="0ff84-112">新行为</span><span class="sxs-lookup"><span data-stu-id="0ff84-112">New behavior</span></span>

<span data-ttu-id="0ff84-113">`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 已过时。</span><span class="sxs-lookup"><span data-stu-id="0ff84-113">`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` and `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` are obsolete.</span></span> <span data-ttu-id="0ff84-114">这些类型还派生自 `Microsoft.AspNetCore.Http.BadHttpRequestException`（派生自 `System.IO.IOException`）。</span><span class="sxs-lookup"><span data-stu-id="0ff84-114">The types also derive from `Microsoft.AspNetCore.Http.BadHttpRequestException`, which derives from `System.IO.IOException`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="0ff84-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="0ff84-115">Reason for change</span></span>

<span data-ttu-id="0ff84-116">此更改的目的是：</span><span class="sxs-lookup"><span data-stu-id="0ff84-116">The change was made to:</span></span>

* <span data-ttu-id="0ff84-117">合并重复类型。</span><span class="sxs-lookup"><span data-stu-id="0ff84-117">Consolidate duplicate types.</span></span>
* <span data-ttu-id="0ff84-118">跨服务器实现统一行为。</span><span class="sxs-lookup"><span data-stu-id="0ff84-118">Unify behavior across server implementations.</span></span>

<span data-ttu-id="0ff84-119">现在，使用 Kestrel 或 IIS 时，应用可以捕获基本异常 `Microsoft.AspNetCore.Http.BadHttpRequestException`。</span><span class="sxs-lookup"><span data-stu-id="0ff84-119">An app can now catch the base exception `Microsoft.AspNetCore.Http.BadHttpRequestException` when using either Kestrel or IIS.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="0ff84-120">建议操作</span><span class="sxs-lookup"><span data-stu-id="0ff84-120">Recommended action</span></span>

<span data-ttu-id="0ff84-121">用 `Microsoft.AspNetCore.Http.BadHttpRequestException` 替换 `Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 的用法。</span><span class="sxs-lookup"><span data-stu-id="0ff84-121">Replace usages of `Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` and `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` with `Microsoft.AspNetCore.Http.BadHttpRequestException`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="0ff84-122">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="0ff84-122">Affected APIs</span></span>

- [<span data-ttu-id="0ff84-123">Microsoft.AspNetCore.Server.IIS.BadHttpRequestException</span><span class="sxs-lookup"><span data-stu-id="0ff84-123">Microsoft.AspNetCore.Server.IIS.BadHttpRequestException</span></span>](/dotnet/api/microsoft.aspnetcore.server.iis.badhttprequestexception?view=aspnetcore-3.1)
- [<span data-ttu-id="0ff84-124">Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException</span><span class="sxs-lookup"><span data-stu-id="0ff84-124">Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.badhttprequestexception?view=aspnetcore-1.1)

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.AspNetCore.Server.IIS.BadHttpRequestException`
- `T:Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException`

-->
