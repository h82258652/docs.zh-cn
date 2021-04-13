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
# <a name="blazor-insignificant-whitespace-trimmed-from-components-at-compile-time"></a>Blazor：在编译时从组件中剪裁掉无意义的空白

从 ASP.NET Core 5.0 Preview 7 开始，Razor 编译器将在编译时省略 Blazor 组件（ *.Razor* 文件）中无意义的空白。 有关讨论，请参阅问题 [dotnet/aspnetcore#23568](https://github.com/dotnet/aspnetcore/issues/23568)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 7

## <a name="old-behavior"></a>旧行为

在 Blazor Server 和 Blazor WebAssembly 的 3.x 版本中，会在组件的源代码中采用空白。 即使在没有视觉效果的情况下，只有空白的文本节点也会呈现在浏览器的文档对象模型 (DOM) 中。

请考虑以下 Blazor 组件代码：

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

前面的示例呈现了两个空白节点：

* 在 `@foreach` 代码块外。
* 围绕 `<li>` 元素。
* 围绕 `@item.Text` 输出。

包含 100 项的一个列表造成了 402 个空白节点。 即使所有空白节点均不会对呈现的输出产生任何视觉影响，但这也超出了呈现的所有节点的一半。

呈现组件的静态 HTML 时，不会保留标记中的空白。 例如，查看以下组件的源：

```razor
<foo        bar="baz"     />
```

不保留空白。 预呈现的输出为：

```razor
<foo bar="baz" />
```

## <a name="new-behavior"></a>新行为

除非将 `@preservewhitespace` 指令与值 `true` 结合使用，否则，如果空白节点满足以下条件，它们则将被删除：

* 在元素中是前导或尾随的空白节点。
* 在 `RenderFragment` 参数中是前导或尾随的空白节点。 例如，传递到另一个组件的子内容。
* 在 C# 代码块（例如 `@if` 和 `@foreach`）之前或之后。

## <a name="reason-for-change"></a>更改原因

ASP.NET Core 5.0 中 Blazor 的目标是改善呈现和比较的性能。 无意义的空白树节点最多消耗 40% 的基准呈现时间。

## <a name="recommended-action"></a>建议操作

在大多数情况下，呈现组件的视觉布局不会受影响。 但是，在使用 CSS 规则（例如 `white-space: pre`）时，删除空白可能会影响呈现输出。 若要禁用此性能优化并保留空白，请执行以下任一操作：

* 将 `@preservewhitespace true` 指令添加到 .razor 文件的顶部，从而将首选项应用于特定组件。
* 将 `@preservewhitespace true` 指令添加到 _Imports.razor 文件中，从而将首选项应用于整个子目录或整个项目。

在大多数情况下，不需要执行任何操作，因为应用程序通常会继续正常运行（但速度会更快）。 如果去除空白会导致特定组件出现任何问题，请在该组件中使用 `@preservewhitespace true` 来禁用此优化。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
