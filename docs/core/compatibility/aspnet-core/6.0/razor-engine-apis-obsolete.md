---
title: 中断性变更：Razor：RazorEngine API 已标记为已过时
description: 了解 ASP.NET Core 6.0 中的中断性变更，其标题为：Razor：RazorEngine API 已标记为已过时
ms.author: scaddie
ms.date: 02/09/2021
ms.openlocfilehash: a312fb2fda6245e529d59d82b72ffe64ae6d6ff5
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255098"
---
# <a name="razor-razorengine-apis-marked-obsolete"></a>Razor：RazorEngine API 已标记为已过时

与 Blazor 中的 <xref:Microsoft.AspNetCore.Razor.Language.RazorEngine> 类型相关的类型已标记为已过时。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 6.0 预览版 1

## <a name="old-behavior"></a>旧行为

`RazorEngine` API 未过时。

## <a name="new-behavior"></a>新行为

`RazorEngine` API 已过时。

## <a name="reason-for-change"></a>更改原因

`RazorEngine` 类型已弃用，取而代之的是 <xref:Microsoft.AspNetCore.Razor.Language.RazorProjectEngine> 类型。

## <a name="recommended-action"></a>建议的操作

将源代码从 `RazorEngine` 类型迁移到 `RazorProjectEngine` 类型。

## <a name="affected-apis"></a>受影响的 API

- `Microsoft.AspNetCore.Mvc.Razor.Extensions.InjectDirective.Register`
- `Microsoft.AspNetCore.Mvc.Razor.Extensions.ModelDirective.Register`
- `Microsoft.AspNetCore.Mvc.Razor.Extensions.PageDirective.Register`
- [Microsoft.AspNetCore.Razor.Language.Extensions.FunctionsDirective.Register](/dotnet/api/microsoft.aspnetcore.razor.language.extensions.functionsdirective.register?view=aspnetcore-3.0&preserve-view=true)
- [Microsoft.AspNetCore.Razor.Language.Extensions.InheritsDirective.Register](/dotnet/api/microsoft.aspnetcore.razor.language.extensions.inheritsdirective.register?view=aspnetcore-3.0&preserve-view=true)
- [Microsoft.AspNetCore.Razor.Language.Extensions.SectionDirective.Register](/dotnet/api/microsoft.aspnetcore.razor.language.extensions.sectiondirective.register?view=aspnetcore-3.0&preserve-view=true)
- [Microsoft.AspNetCore.Razor.Language.IRazorEngineBuilder](/dotnet/api/microsoft.aspnetcore.razor.language.irazorenginebuilder?view=aspnetcore-3.0&preserve-view=true)

<!--

## Category

ASP.NET Core

## Affected APIs

- `Overload:Microsoft.AspNetCore.Mvc.Razor.Extensions.InjectDirective.Register`
- `Overload:Microsoft.AspNetCore.Mvc.Razor.Extensions.ModelDirective.Register`
- `Overload:Microsoft.AspNetCore.Mvc.Razor.Extensions.PageDirective.Register`
- `Overload:Microsoft.AspNetCore.Razor.Language.Extensions.FunctionsDirective.Register`
- `Overload:Microsoft.AspNetCore.Razor.Language.Extensions.InheritsDirective.Register`
- `Overload:Microsoft.AspNetCore.Razor.Language.Extensions.SectionDirective.Register`
- `T:Microsoft.AspNetCore.Razor.Language.IRazorEngineBuilder`

-->
