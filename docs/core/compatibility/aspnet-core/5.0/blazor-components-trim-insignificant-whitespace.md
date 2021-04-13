---
title: 中断性变更：Blazor：在编译时从组件中剪裁掉无意义的空白
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：在编译时从组件中剪裁掉无意义的空白
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: d29171c43e587eba648de3f877e1cbb1c7156499
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498199"
---
# <a name="blazor-insignificant-whitespace-trimmed-from-components-at-compile-time"></a><span data-ttu-id="a99b1-103">Blazor：在编译时从组件中剪裁掉无意义的空白</span><span class="sxs-lookup"><span data-stu-id="a99b1-103">Blazor: Insignificant whitespace trimmed from components at compile time</span></span>

<span data-ttu-id="a99b1-104">从 ASP.NET Core 5.0 Preview 7 开始，Razor 编译器将在编译时省略 Blazor 组件（ *.Razor* 文件）中无意义的空白。</span><span class="sxs-lookup"><span data-stu-id="a99b1-104">Starting with ASP.NET Core 5.0 Preview 7, the Razor compiler omits insignificant whitespace in Blazor components (*.razor* files) at compile time.</span></span> <span data-ttu-id="a99b1-105">有关讨论，请参阅问题 [dotnet/aspnetcore#23568](https://github.com/dotnet/aspnetcore/issues/23568)。</span><span class="sxs-lookup"><span data-stu-id="a99b1-105">For discussion, see issue [dotnet/aspnetcore#23568](https://github.com/dotnet/aspnetcore/issues/23568).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="a99b1-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="a99b1-106">Version introduced</span></span>

<span data-ttu-id="a99b1-107">5.0 预览版 7</span><span class="sxs-lookup"><span data-stu-id="a99b1-107">5.0 Preview 7</span></span>

## <a name="old-behavior"></a><span data-ttu-id="a99b1-108">旧行为</span><span class="sxs-lookup"><span data-stu-id="a99b1-108">Old behavior</span></span>

<span data-ttu-id="a99b1-109">在 Blazor Server 和 Blazor WebAssembly 的 3.x 版本中，会在组件的源代码中采用空白。</span><span class="sxs-lookup"><span data-stu-id="a99b1-109">In 3.x versions of Blazor Server and Blazor WebAssembly, whitespace is honored in a component's source code.</span></span> <span data-ttu-id="a99b1-110">即使在没有视觉效果的情况下，只有空白的文本节点也会呈现在浏览器的文档对象模型 (DOM) 中。</span><span class="sxs-lookup"><span data-stu-id="a99b1-110">Whitespace-only text nodes render in the browser's Document Object Model (DOM) even when there's no visual effect.</span></span>

<span data-ttu-id="a99b1-111">请考虑以下 Blazor 组件代码：</span><span class="sxs-lookup"><span data-stu-id="a99b1-111">Consider the following Blazor component code:</span></span>

```razor
<ul>
    @foreach (var item in Items)
    {
        <li>
            @item.Text
        </li>
    }
</ul>
```

<span data-ttu-id="a99b1-112">前面的示例呈现了两个空白节点：</span><span class="sxs-lookup"><span data-stu-id="a99b1-112">The preceding example renders two whitespace nodes:</span></span>

* <span data-ttu-id="a99b1-113">在 `@foreach` 代码块外。</span><span class="sxs-lookup"><span data-stu-id="a99b1-113">Outside of the `@foreach` code block.</span></span>
* <span data-ttu-id="a99b1-114">围绕 `<li>` 元素。</span><span class="sxs-lookup"><span data-stu-id="a99b1-114">Around the `<li>` element.</span></span>
* <span data-ttu-id="a99b1-115">围绕 `@item.Text` 输出。</span><span class="sxs-lookup"><span data-stu-id="a99b1-115">Around the `@item.Text` output.</span></span>

<span data-ttu-id="a99b1-116">包含 100 项的一个列表造成了 402 个空白节点。</span><span class="sxs-lookup"><span data-stu-id="a99b1-116">A list containing 100 items results in 402 whitespace nodes.</span></span> <span data-ttu-id="a99b1-117">即使所有空白节点均不会对呈现的输出产生任何视觉影响，但这也超出了呈现的所有节点的一半。</span><span class="sxs-lookup"><span data-stu-id="a99b1-117">That's over half of all nodes rendered, even though none of the whitespace nodes visually affect the rendered output.</span></span>

<span data-ttu-id="a99b1-118">呈现组件的静态 HTML 时，不会保留标记中的空白。</span><span class="sxs-lookup"><span data-stu-id="a99b1-118">When rendering static HTML for components, whitespace inside a tag wasn't preserved.</span></span> <span data-ttu-id="a99b1-119">例如，查看以下组件的源：</span><span class="sxs-lookup"><span data-stu-id="a99b1-119">For example, view the source of the following component:</span></span>

```razor
<foo        bar="baz"     />
```

<span data-ttu-id="a99b1-120">不保留空白。</span><span class="sxs-lookup"><span data-stu-id="a99b1-120">Whitespace isn't preserved.</span></span> <span data-ttu-id="a99b1-121">预呈现的输出为：</span><span class="sxs-lookup"><span data-stu-id="a99b1-121">The pre-rendered output is:</span></span>

```razor
<foo bar="baz" />
```

## <a name="new-behavior"></a><span data-ttu-id="a99b1-122">新行为</span><span class="sxs-lookup"><span data-stu-id="a99b1-122">New behavior</span></span>

<span data-ttu-id="a99b1-123">除非将 `@preservewhitespace` 指令与值 `true` 结合使用，否则，如果空白节点满足以下条件，它们则将被删除：</span><span class="sxs-lookup"><span data-stu-id="a99b1-123">Unless the `@preservewhitespace` directive is used with value `true`, whitespace nodes are removed if they:</span></span>

* <span data-ttu-id="a99b1-124">在元素中是前导或尾随的空白节点。</span><span class="sxs-lookup"><span data-stu-id="a99b1-124">Are leading or trailing within an element.</span></span>
* <span data-ttu-id="a99b1-125">在 `RenderFragment` 参数中是前导或尾随的空白节点。</span><span class="sxs-lookup"><span data-stu-id="a99b1-125">Are leading or trailing within a `RenderFragment` parameter.</span></span> <span data-ttu-id="a99b1-126">例如，传递到另一个组件的子内容。</span><span class="sxs-lookup"><span data-stu-id="a99b1-126">For example, child content being passed to another component.</span></span>
* <span data-ttu-id="a99b1-127">在 C# 代码块（例如 `@if` 和 `@foreach`）之前或之后。</span><span class="sxs-lookup"><span data-stu-id="a99b1-127">Precede or follow a C# code block such as `@if` and `@foreach`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="a99b1-128">更改原因</span><span class="sxs-lookup"><span data-stu-id="a99b1-128">Reason for change</span></span>

<span data-ttu-id="a99b1-129">ASP.NET Core 5.0 中 Blazor 的目标是改善呈现和比较的性能。</span><span class="sxs-lookup"><span data-stu-id="a99b1-129">A goal for Blazor in ASP.NET Core 5.0 is to improve the performance of rendering and diffing.</span></span> <span data-ttu-id="a99b1-130">无意义的空白树节点最多消耗 40% 的基准呈现时间。</span><span class="sxs-lookup"><span data-stu-id="a99b1-130">Insignificant whitespace tree nodes consumed up to 40 percent of the rendering time in benchmarks.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="a99b1-131">建议操作</span><span class="sxs-lookup"><span data-stu-id="a99b1-131">Recommended action</span></span>

<span data-ttu-id="a99b1-132">在大多数情况下，呈现组件的视觉布局不会受影响。</span><span class="sxs-lookup"><span data-stu-id="a99b1-132">In most cases, the visual layout of the rendered component is unaffected.</span></span> <span data-ttu-id="a99b1-133">但是，在使用 CSS 规则（例如 `white-space: pre`）时，删除空白可能会影响呈现输出。</span><span class="sxs-lookup"><span data-stu-id="a99b1-133">However, the whitespace removal might affect the rendered output when using a CSS rule like `white-space: pre`.</span></span> <span data-ttu-id="a99b1-134">若要禁用此性能优化并保留空白，请执行以下任一操作：</span><span class="sxs-lookup"><span data-stu-id="a99b1-134">To disable this performance optimization and preserve the whitespace, take one of the following actions:</span></span>

* <span data-ttu-id="a99b1-135">将 `@preservewhitespace true` 指令添加到 .razor 文件的顶部，从而将首选项应用于特定组件。</span><span class="sxs-lookup"><span data-stu-id="a99b1-135">Add the `@preservewhitespace true` directive at the top of the *.razor* file to apply the preference to a specific component.</span></span>
* <span data-ttu-id="a99b1-136">将 `@preservewhitespace true` 指令添加到 _Imports.razor 文件中，从而将首选项应用于整个子目录或整个项目。</span><span class="sxs-lookup"><span data-stu-id="a99b1-136">Add the `@preservewhitespace true` directive inside an *_Imports.razor* file to apply the preference to an entire subdirectory or the entire project.</span></span>

<span data-ttu-id="a99b1-137">在大多数情况下，不需要执行任何操作，因为应用程序通常会继续正常运行（但速度会更快）。</span><span class="sxs-lookup"><span data-stu-id="a99b1-137">In most cases, no action is required, as applications will typically continue to behave normally (but faster).</span></span> <span data-ttu-id="a99b1-138">如果去除空白会导致特定组件出现任何问题，请在该组件中使用 `@preservewhitespace true` 来禁用此优化。</span><span class="sxs-lookup"><span data-stu-id="a99b1-138">If the whitespace stripping causes any problems for a particular component, use `@preservewhitespace true` in that component to disable this optimization.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="a99b1-139">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="a99b1-139">Affected APIs</span></span>

<span data-ttu-id="a99b1-140">无</span><span class="sxs-lookup"><span data-stu-id="a99b1-140">None</span></span>

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
