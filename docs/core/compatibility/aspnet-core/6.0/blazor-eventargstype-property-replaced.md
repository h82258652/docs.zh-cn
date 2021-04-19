---
title: 中断性变更：Blazor：已替换 WebEventDescriptor.EventArgsType 属性
description: 了解 ASP.NET Core 6.0 中的中断性变更，其中 WebEventDescriptor.EventArgsType 属性被替换为 EventName 属性。
ms.author: scaddie
ms.date: 02/24/2021
no-loc:
- Blazor
ms.openlocfilehash: fb82e27d7ab55b78293f40debe917da2d99239a7
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255207"
---
# <a name="blazor-no-loc-textwebeventdescriptoreventargstype-property-replaced"></a><span data-ttu-id="ea613-103">Blazor：已替换 :::no-loc text="WebEventDescriptor.EventArgsType"::: 属性</span><span class="sxs-lookup"><span data-stu-id="ea613-103">Blazor: :::no-loc text="WebEventDescriptor.EventArgsType"::: property replaced</span></span>

<span data-ttu-id="ea613-104"><xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor> 类是 Blazor 内部协议的一部分，用于将事件从 JavaScript 传递到 .NET 中。</span><span class="sxs-lookup"><span data-stu-id="ea613-104">The <xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor> class is part of Blazor's internal protocol for communicating events from JavaScript into .NET.</span></span> <span data-ttu-id="ea613-105">这个类通常不由应用代码使用，而是由平台作者使用。</span><span class="sxs-lookup"><span data-stu-id="ea613-105">This class isn't typically used by app code, but rather by platform authors.</span></span>

<span data-ttu-id="ea613-106">从 ASP.NET Core 6.0 开始，`WebEventDescriptor` 上的 <xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType%2A> 属性被新的 `EventName` 属性所替代。</span><span class="sxs-lookup"><span data-stu-id="ea613-106">Starting in ASP.NET Core 6.0, the <xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType%2A> property on `WebEventDescriptor` is being replaced by a new `EventName` property.</span></span> <span data-ttu-id="ea613-107">这一变更不太可能影响任何应用代码，因为这是一个低级别的平台实现细节。</span><span class="sxs-lookup"><span data-stu-id="ea613-107">This change is unlikely to affect any app code, as it's a low-level platform implementation detail.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="ea613-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ea613-108">Version introduced</span></span>

<span data-ttu-id="ea613-109">ASP.NET Core 6.0</span><span class="sxs-lookup"><span data-stu-id="ea613-109">ASP.NET Core 6.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="ea613-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="ea613-110">Old behavior</span></span>

<span data-ttu-id="ea613-111">在 ASP.NET Core 5.0 及更早版本中，属性 `EventArgsType` 为 DOM 事件类型组描述了一个非标准的、特定于 Blazor 的类别名称。</span><span class="sxs-lookup"><span data-stu-id="ea613-111">In ASP.NET Core 5.0 and earlier, the property `EventArgsType` describes a nonstandard, Blazor-specific category name for groups of DOM event types.</span></span> <span data-ttu-id="ea613-112">例如，`click` 和 `mousedown` 事件都映射到 `mouse` 的 `EventArgsType` 值。</span><span class="sxs-lookup"><span data-stu-id="ea613-112">For example, the `click` and `mousedown` events were both mapped to an `EventArgsType` value of `mouse`.</span></span> <span data-ttu-id="ea613-113">同样，`cut`、`copy` 和 `paste` 事件被映射到 `clipboard` 的 `EventArgsType` 值。</span><span class="sxs-lookup"><span data-stu-id="ea613-113">Similarly, `cut`, `copy`, and `paste` events are mapped to an `EventArgsType` value of `clipboard`.</span></span> <span data-ttu-id="ea613-114">这些类别名称用于确定将传入事件参数数据反序列化的 .NET 类型。</span><span class="sxs-lookup"><span data-stu-id="ea613-114">These category names are used to determine the .NET type to use for deserializing the incoming event arguments data.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="ea613-115">新行为</span><span class="sxs-lookup"><span data-stu-id="ea613-115">New behavior</span></span>

<span data-ttu-id="ea613-116">从 ASP.NET Core 6.0 开始，新属性 `EventName` 仅指定原始事件的名称。</span><span class="sxs-lookup"><span data-stu-id="ea613-116">Starting in ASP.NET Core 6.0, the new property `EventName` only specifies the original event's name.</span></span> <span data-ttu-id="ea613-117">例如，`click`、`mousedown`、`cut`、`copy` 或 `paste`。</span><span class="sxs-lookup"><span data-stu-id="ea613-117">For example, `click`, `mousedown`, `cut`, `copy`, or `paste`.</span></span> <span data-ttu-id="ea613-118">不再需要提供特定于 Blazor 的类别名称。</span><span class="sxs-lookup"><span data-stu-id="ea613-118">There's no longer a need to supply a Blazor-specific category name.</span></span> <span data-ttu-id="ea613-119">因此，旧属性 `EventArgsType` 被删除。</span><span class="sxs-lookup"><span data-stu-id="ea613-119">For that reason, the old property `EventArgsType` is removed.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="ea613-120">更改原因</span><span class="sxs-lookup"><span data-stu-id="ea613-120">Reason for change</span></span>

<span data-ttu-id="ea613-121">在拉取请求 [dotnet/aspnetcore#29993](https://github.com/dotnet/aspnetcore/pull/29993) 中，引入了对自定义事件参数类的支持。</span><span class="sxs-lookup"><span data-stu-id="ea613-121">In pull request [dotnet/aspnetcore#29993](https://github.com/dotnet/aspnetcore/pull/29993), support for custom event arguments classes was introduced.</span></span> <span data-ttu-id="ea613-122">作为此支持的一部分，框架不再依赖于所有事件都符合预定义的类别集。</span><span class="sxs-lookup"><span data-stu-id="ea613-122">As part of this support, the framework no longer relies on all events fitting into a predefined set of categories.</span></span> <span data-ttu-id="ea613-123">现在，框架只需要知道原始事件名称。</span><span class="sxs-lookup"><span data-stu-id="ea613-123">The framework now only needs to know the original event name.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="ea613-124">建议的操作</span><span class="sxs-lookup"><span data-stu-id="ea613-124">Recommended action</span></span>

<span data-ttu-id="ea613-125">应用代码应该不受影响，不需要更改。</span><span class="sxs-lookup"><span data-stu-id="ea613-125">App code should be unaffected and doesn't need to change.</span></span>

<span data-ttu-id="ea613-126">如果是构建自定义 Blazor 呈现平台，可能需要更新将事件调度到 `Renderer` 中的机制。</span><span class="sxs-lookup"><span data-stu-id="ea613-126">If building a custom Blazor rendering platform, you may need to update the mechanism for dispatching events into the `Renderer`.</span></span> <span data-ttu-id="ea613-127">用提供原始事件名称的更简单逻辑替换任何关于事件类别的硬编码规则。</span><span class="sxs-lookup"><span data-stu-id="ea613-127">Replace any hardcoded rules about event categories with simpler logic that supplies the original, raw event name.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="ea613-128">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ea613-128">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType%2A?displayProperty=nameWithType>

<!--

## Category

ASP.NET Core

## Affected APIs

`P:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType`

-->
