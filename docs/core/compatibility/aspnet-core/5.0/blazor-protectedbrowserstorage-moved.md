---
title: 中断性变更：Blazor：ProtectedBrowserStorage 功能已移动到共享框架
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：ProtectedBrowserStorage 功能已移动到共享框架
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 36ba6c16ac970aa8acb0d0faab42b151addae888
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497835"
---
# <a name="blazor-protectedbrowserstorage-feature-moved-to-shared-framework"></a>Blazor：ProtectedBrowserStorage 功能已移动到共享框架

作为 ASP.NET Core 5.0 RC2 版本的一部分，`ProtectedBrowserStorage` 功能已移动到 ASP.NET Core 共享框架。

## <a name="version-introduced"></a>引入的版本

5.0 RC2

## <a name="old-behavior"></a>旧行为

在 ASP.NET Core 5.0 预览版 8 中，此功能作为 [Microsoft.AspNetCore.Components.Web.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web.Extensions) 包的一部分提供，但只能在 Blazor WebAssembly 中使用。

在 ASP.NET Core 5.0 RC1 中，该功能作为 [Microsoft.AspNetCore.Components.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.ProtectedBrowserStorage) 包的一部分提供，此包引用 `Microsoft.AspNetCore.App` 共享框架。

## <a name="new-behavior"></a>新行为

在 ASP.NET Core 5.0 RC2 中，引用和使用该功能时，不再需要 NuGet 包引用。

## <a name="reason-for-change"></a>更改原因

移动到共享框架后，可以更好地提供客户预期的用户体验。

## <a name="recommended-action"></a>建议操作

如果从 ASP.NET Core 5.0 RC1 升级，请完成以下步骤：

1. 从项目中删除 `Microsoft.AspNetCore.Components.ProtectedBrowserStorage` 包引用。
1. 将 `using Microsoft.AspNetCore.Components.ProtectedBrowserStorage;` 替换为 `using Microsoft.AspNetCore.Components.Server.ProtectedBrowserStorage;`。
1. 从 `Startup` 类中删除对 `AddProtectedBrowserStorage` 的调用。

如果从 ASP.NET Core 5.0 预览版 8 升级，请完成以下步骤：

1. 从项目中删除 `Microsoft.AspNetCore.Components.Web.Extensions` 包引用。
1. 将 `using Microsoft.AspNetCore.Components.Web.Extensions;` 替换为 `using Microsoft.AspNetCore.Components.Server.ProtectedBrowserStorage;`。
1. 从 `Startup` 类中删除对 `AddProtectedBrowserStorage` 的调用。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
