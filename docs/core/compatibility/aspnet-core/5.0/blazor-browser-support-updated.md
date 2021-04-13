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
# <a name="blazor-updated-browser-support"></a>Blazor：更新的浏览器支持

ASP.NET Core 5.0 引入了[新的 Blazor 功能](https://github.com/dotnet/aspnetcore/issues/21514)，其中一些与旧版浏览器不兼容。 ASP.NET Core 5.0 中 Blazor 支持的浏览器列表已相应更新。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#26475](https://github.com/dotnet/aspnetcore/issues/26475)。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="old-behavior"></a>旧行为

Blazor Server 支持具有足够填充代码的 Microsoft Internet Explorer 11。 Blazor Server 和 Blazor WebAssembly 在 [Microsoft Edge 旧版](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy)中也能正常工作。

## <a name="new-behavior"></a>新行为

Microsoft Internet Explorer 11 不支持 ASP.NET Core 5.0 中的 Blazor Server。 Blazor Server 和 Blazor WebAssembly 在 Microsoft Edge 旧版中不能完全正常工作。

## <a name="reason-for-change"></a>更改原因

ASP.NET Core 5.0 中的新 Blazor 功能与这些旧版浏览器不兼容，并且这些旧版浏览器的使用在日益减少。 有关详细信息，请参阅以下资源：

* [Windows 对旧版 Microsoft Edge 的支持也将在 2021 年 3 月 9 日终止](https://support.microsoft.com/help/4533505/what-is-microsoft-edge-legacy)
* [Microsoft 365 的应用和服务将于 2021 年 8 月 17 日终止对 Microsoft Internet Explorer 11 的支持](/lifecycle/announcements/m365-ie11-microsoft-edge-legacy)

## <a name="recommended-action"></a>建议的操作

从这些旧版浏览器升级到[基于 Chromium 的新 Microsoft Edge](https://www.microsoft.com/edge)。 对于需要支持这些旧版浏览器的 Blazor 应用，请使用 ASP.NET Core 3.1。 ASP.NET Core 3.1 中 Blazor 支持的浏览器列表尚未更改，并且记录在[支持的平台](/aspnet/core/blazor/supported-platforms?view=aspnetcore-3.1)中。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
