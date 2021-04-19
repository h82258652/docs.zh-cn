---
title: 中断性变更：Blazor：RequestImageFileAsync 方法中的参数名称已更改
description: 了解 ASP.NET Core 6.0 中的中断性变更，其标题为：Blazor：RequestImageFileAsync 方法中的参数名称已更改
ms.author: scaddie
ms.date: 02/09/2021
ms.openlocfilehash: 645b53e341507ffd9f369eea1b940232b7c14770
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255176"
---
# <a name="blazor-parameter-name-changed-in-requestimagefileasync-method"></a><span data-ttu-id="66c6c-103">Blazor：RequestImageFileAsync 方法中的参数名称已更改</span><span class="sxs-lookup"><span data-stu-id="66c6c-103">Blazor: Parameter name changed in RequestImageFileAsync method</span></span>

<span data-ttu-id="66c6c-104">`RequestImageFileAsync` 方法的 `maxWith` 参数已从 `maxWith` 重命名为 `maxWidth`。</span><span class="sxs-lookup"><span data-stu-id="66c6c-104">The `RequestImageFileAsync` method's `maxWith` parameter was renamed from `maxWith` to `maxWidth`.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="66c6c-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="66c6c-105">Version introduced</span></span>

<span data-ttu-id="66c6c-106">ASP.NET Core 6.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="66c6c-106">ASP.NET Core 6.0 Preview 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="66c6c-107">旧行为</span><span class="sxs-lookup"><span data-stu-id="66c6c-107">Old behavior</span></span>

<span data-ttu-id="66c6c-108">参数名称的拼写为 `maxWith`。</span><span class="sxs-lookup"><span data-stu-id="66c6c-108">The parameter name is spelled `maxWith`.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="66c6c-109">新行为</span><span class="sxs-lookup"><span data-stu-id="66c6c-109">New behavior</span></span>

<span data-ttu-id="66c6c-110">参数名称的拼写为 `maxWidth`。</span><span class="sxs-lookup"><span data-stu-id="66c6c-110">The parameter name is spelled `maxWidth`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="66c6c-111">更改原因</span><span class="sxs-lookup"><span data-stu-id="66c6c-111">Reason for change</span></span>

<span data-ttu-id="66c6c-112">原始参数名称存在录入错误。</span><span class="sxs-lookup"><span data-stu-id="66c6c-112">The original parameter name was a typographical error.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="66c6c-113">建议的操作</span><span class="sxs-lookup"><span data-stu-id="66c6c-113">Recommended action</span></span>

<span data-ttu-id="66c6c-114">如果使用的是 `RequestImageFile` API 中的命名参数，请将 `maxWith` 参数名称更新为 `maxWidth`。</span><span class="sxs-lookup"><span data-stu-id="66c6c-114">If you're using named parameters in the `RequestImageFile` API, update the `maxWith` parameter name to `maxWidth`.</span></span> <span data-ttu-id="66c6c-115">否则，不需要进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="66c6c-115">Otherwise, no change is necessary.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="66c6c-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="66c6c-116">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Components.Forms.BrowserFileExtensions.RequestImageFileAsync%2A?displayProperty=nameWithType>

<!--

## Category

ASP.NET Core

## Affected APIs

`Overload:Microsoft.AspNetCore.Components.Forms.BrowserFileExtensions.RequestImageFileAsync`

-->
