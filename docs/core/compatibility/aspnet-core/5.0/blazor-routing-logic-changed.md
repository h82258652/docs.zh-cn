---
title: 中断性变更：Blazor：Blazor 应用中的路由逻辑更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：Blazor 应用中的路由逻辑更改
ms.author: scaddie
ms.date: 12/14/2020
ms.openlocfilehash: cec05b26319fc8302261e41cc868dd73b46c8af0
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497822"
---
# <a name="blazor-route-precedence-logic-changed-in-blazor-apps"></a><span data-ttu-id="76aaa-103">Blazor：Blazor 应用中已更改路由优先逻辑</span><span class="sxs-lookup"><span data-stu-id="76aaa-103">Blazor: Route precedence logic changed in Blazor apps</span></span>

<span data-ttu-id="76aaa-104">Blazor 路由实现中的 bug 影响了路由优先级的确定方式。</span><span class="sxs-lookup"><span data-stu-id="76aaa-104">A bug in the Blazor routing implementation affected how the precedence of routes was determined.</span></span> <span data-ttu-id="76aaa-105">此 bug 会影响 Blazor 应用内的 catch-all 路由或带可选参数的路由。</span><span class="sxs-lookup"><span data-stu-id="76aaa-105">This bug affects catch-all routes or routes with optional parameters within your Blazor app.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="76aaa-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="76aaa-106">Version introduced</span></span>

<span data-ttu-id="76aaa-107">5.0.1</span><span class="sxs-lookup"><span data-stu-id="76aaa-107">5.0.1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="76aaa-108">旧行为</span><span class="sxs-lookup"><span data-stu-id="76aaa-108">Old behavior</span></span>

<span data-ttu-id="76aaa-109">发生错误行为时，系统将优先考虑并匹配优先级较低的路由，而不是优先级较高的路由。</span><span class="sxs-lookup"><span data-stu-id="76aaa-109">With the erroneous behavior, routes with lower precedence are considered and matched over routes with higher precedence.</span></span> <span data-ttu-id="76aaa-110">例如，它会先匹配 `/customer/{id}` 路由，后匹配 `{*slug}` 路由。</span><span class="sxs-lookup"><span data-stu-id="76aaa-110">For example, the `{*slug}` route is matched before `/customer/{id}`.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="76aaa-111">新行为</span><span class="sxs-lookup"><span data-stu-id="76aaa-111">New behavior</span></span>

<span data-ttu-id="76aaa-112">当前的行为会更精确地匹配 ASP.NET Core 应用中定义的路由。</span><span class="sxs-lookup"><span data-stu-id="76aaa-112">The current behavior more closely matches the routing behavior defined in ASP.NET Core apps.</span></span> <span data-ttu-id="76aaa-113">框架首先确定每个网段的路由优先级。</span><span class="sxs-lookup"><span data-stu-id="76aaa-113">The framework determines the route precedence for each segment first.</span></span> <span data-ttu-id="76aaa-114">路由的长度仅用作与中断相关的第二个条件。</span><span class="sxs-lookup"><span data-stu-id="76aaa-114">The route's length is used only as a second criteria to break ties.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="76aaa-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="76aaa-115">Reason for change</span></span>

<span data-ttu-id="76aaa-116">最初的行为被视为实现中的一个 bug。</span><span class="sxs-lookup"><span data-stu-id="76aaa-116">The original behavior is considered a bug in the implementation.</span></span> <span data-ttu-id="76aaa-117">作为目标，Blazor 应用中路由系统的行为方式应与其他 ASP.NET Core 中路由系统的行为方式相同。</span><span class="sxs-lookup"><span data-stu-id="76aaa-117">As a goal, the routing system in Blazor apps should behave the same way as the routing system in the rest of ASP.NET Core.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="76aaa-118">建议的操作</span><span class="sxs-lookup"><span data-stu-id="76aaa-118">Recommended action</span></span>

<span data-ttu-id="76aaa-119">如果从 Blazor 的早期版本升级到 5.x，请使用 `Router` 组件上的 `PreferExactMatches` 属性。</span><span class="sxs-lookup"><span data-stu-id="76aaa-119">If upgrading from previous versions of Blazor to 5.x, use the `PreferExactMatches` attribute on the `Router` component.</span></span> <span data-ttu-id="76aaa-120">此属性可用于选择正确的行为。</span><span class="sxs-lookup"><span data-stu-id="76aaa-120">This attribute can be used to opt into the correct behavior.</span></span> <span data-ttu-id="76aaa-121">例如：</span><span class="sxs-lookup"><span data-stu-id="76aaa-121">For example:</span></span>

```razor
<Router AppAssembly="@typeof(Program).Assembly" PreferExactMatches="true">
```

<span data-ttu-id="76aaa-122">如果将 `PreferExactMatches` 设置为 `true`，则路由匹配将优先选取精确匹配而非通配符匹配。</span><span class="sxs-lookup"><span data-stu-id="76aaa-122">When `PreferExactMatches` is set to `true`, route matching prefers exact matches over wildcards.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="76aaa-123">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="76aaa-123">Affected APIs</span></span>

<span data-ttu-id="76aaa-124">无</span><span class="sxs-lookup"><span data-stu-id="76aaa-124">None</span></span>

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis

-->
