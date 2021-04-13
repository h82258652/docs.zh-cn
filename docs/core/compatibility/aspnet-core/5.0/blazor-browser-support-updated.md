---
title: 中断性变更：Blazor：更新的浏览器支持
description: 了解 ASP.NET Core 5.0 中的中断性变更，其标题为：Blazor：更新的浏览器支持
ms.author: scaddie
ms.date: 10/01/2020
no-loc:
- Blazor
- Blazor WebAssembly
- Blazor Server
ms.openlocfilehash: 25566a4b30aaa8484ec4bcf26ab1e8fde6af1279
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497861"
---
# <a name="blazor-updated-browser-support"></a><span data-ttu-id="a3ea5-103">Blazor：更新的浏览器支持</span><span class="sxs-lookup"><span data-stu-id="a3ea5-103">Blazor: Updated browser support</span></span>

<span data-ttu-id="a3ea5-104">ASP.NET Core 5.0 引入了[新的 Blazor 功能](https://github.com/dotnet/aspnetcore/issues/21514)，其中一些与旧版浏览器不兼容。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-104">ASP.NET Core 5.0 introduces [new Blazor features](https://github.com/dotnet/aspnetcore/issues/21514), some of which are incompatible with older browsers.</span></span> <span data-ttu-id="a3ea5-105">ASP.NET Core 5.0 中 Blazor 支持的浏览器列表已相应更新。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-105">The list of browsers supported by Blazor in ASP.NET Core 5.0 has been updated accordingly.</span></span>

<span data-ttu-id="a3ea5-106">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#26475](https://github.com/dotnet/aspnetcore/issues/26475)。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-106">For discussion, see GitHub issue [dotnet/aspnetcore#26475](https://github.com/dotnet/aspnetcore/issues/26475).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="a3ea5-107">引入的版本</span><span class="sxs-lookup"><span data-stu-id="a3ea5-107">Version introduced</span></span>

<span data-ttu-id="a3ea5-108">5.0</span><span class="sxs-lookup"><span data-stu-id="a3ea5-108">5.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="a3ea5-109">旧行为</span><span class="sxs-lookup"><span data-stu-id="a3ea5-109">Old behavior</span></span>

<span data-ttu-id="a3ea5-110">Blazor Server 支持具有足够填充代码的 Microsoft Internet Explorer 11。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-110">Blazor Server supports Microsoft Internet Explorer 11 with sufficient polyfills.</span></span> <span data-ttu-id="a3ea5-111">Blazor Server 和 Blazor WebAssembly 在 [Microsoft Edge 旧版](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy)中也能正常工作。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-111">Blazor Server and Blazor WebAssembly are also functional in [Microsoft Edge Legacy](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy).</span></span>

## <a name="new-behavior"></a><span data-ttu-id="a3ea5-112">新行为</span><span class="sxs-lookup"><span data-stu-id="a3ea5-112">New behavior</span></span>

<span data-ttu-id="a3ea5-113">Microsoft Internet Explorer 11 不支持 ASP.NET Core 5.0 中的 Blazor Server。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-113">Blazor Server in ASP.NET Core 5.0 isn't supported with Microsoft Internet Explorer 11.</span></span> <span data-ttu-id="a3ea5-114">Blazor Server 和 Blazor WebAssembly 在 Microsoft Edge 旧版中不能完全正常工作。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-114">Blazor Server and Blazor WebAssembly aren't fully functional in Microsoft Edge Legacy.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="a3ea5-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="a3ea5-115">Reason for change</span></span>

<span data-ttu-id="a3ea5-116">ASP.NET Core 5.0 中的新 Blazor 功能与这些旧版浏览器不兼容，并且这些旧版浏览器的使用在日益减少。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-116">New Blazor features in ASP.NET Core 5.0 are incompatible with these older browsers, and use of these older browsers is diminishing.</span></span> <span data-ttu-id="a3ea5-117">有关详细信息，请参阅以下资源：</span><span class="sxs-lookup"><span data-stu-id="a3ea5-117">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a3ea5-118">Windows 对旧版 Microsoft Edge 的支持也将在 2021 年 3 月 9 日终止</span><span class="sxs-lookup"><span data-stu-id="a3ea5-118">Windows support for Microsoft Edge Legacy is also ending on March 9, 2021</span></span>](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy)
* [<span data-ttu-id="a3ea5-119">Microsoft 365 的应用和服务将于 2021 年 8 月 17 日终止对 Microsoft Internet Explorer 11 的支持</span><span class="sxs-lookup"><span data-stu-id="a3ea5-119">Microsoft 365 apps and services will end support for Microsoft Internet Explorer 11 by August 17, 2021</span></span>](/lifecycle/announcements/m365-ie11-microsoft-edge-legacy)

## <a name="recommended-action"></a><span data-ttu-id="a3ea5-120">建议的操作</span><span class="sxs-lookup"><span data-stu-id="a3ea5-120">Recommended action</span></span>

<span data-ttu-id="a3ea5-121">从这些旧版浏览器升级到[基于 Chromium 的新 Microsoft Edge](https://www.microsoft.com/edge)。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-121">Upgrade from these older browsers to the [new, Chromium-based Microsoft Edge](https://www.microsoft.com/edge).</span></span> <span data-ttu-id="a3ea5-122">对于需要支持这些旧版浏览器的 Blazor 应用，请使用 ASP.NET Core 3.1。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-122">For Blazor apps that need to support these older browsers, use ASP.NET Core 3.1.</span></span> <span data-ttu-id="a3ea5-123">ASP.NET Core 3.1 中 Blazor 支持的浏览器列表尚未更改，并且记录在[支持的平台](/aspnet/core/blazor/supported-platforms?view=aspnetcore-3.1)中。</span><span class="sxs-lookup"><span data-stu-id="a3ea5-123">The supported browsers list for Blazor in ASP.NET Core 3.1 hasn't changed and is documented at [Supported platforms](/aspnet/core/blazor/supported-platforms?view=aspnetcore-3.1).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="a3ea5-124">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="a3ea5-124">Affected APIs</span></span>

<span data-ttu-id="a3ea5-125">无</span><span class="sxs-lookup"><span data-stu-id="a3ea5-125">None</span></span>

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
