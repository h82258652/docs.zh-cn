---
title: 中断性变更：本地化：请求本地化中间件中删除了已过时的构造函数
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：本地化：请求本地化中间件中删除了已过时的构造函数
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 8c04e1735c5a35d1539b6fcb2506dbe37183ff79
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497978"
---
# <a name="localization-obsolete-constructor-removed-in-request-localization-middleware"></a>本地化：请求本地化中间件中删除了已过时的构造函数

缺少 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 参数的 <xref:Microsoft.AspNetCore.Localization.RequestLocalizationMiddleware> 构造函数[在此提交中](https://github.com/dotnet/aspnetcore/commit/ba8c6ccf6fd3eeb7fc42a159d362b15eae4fb3a0)被标记为已过时。 ASP.NET Core 5.0 中删除了已过时的构造函数。 有关讨论，请参阅 [dotnet/aspnetcore#23785](https://github.com/dotnet/aspnetcore/issues/23785)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 8

## <a name="old-behavior"></a>旧行为

存在已过时的 `RequestLocalizationMiddleware.ctor(RequestDelegate, IOptions<RequestLocalizationOptions>)` 构造函数。

## <a name="new-behavior"></a>新行为

不存在已过时的 `RequestLocalizationMiddleware.ctor(RequestDelegate, IOptions<RequestLocalizationOptions>)` 构造函数。

## <a name="reason-for-change"></a>更改原因

此更改可确保请求本地化中间件始终有权访问记录器。

## <a name="recommended-action"></a>建议操作

在手动构造 `RequestLocalizationMiddleware` 实例时，在构造函数中传递 `ILoggerFactory` 实例。 如果在该上下文中没有可用的有效 `ILoggerFactory` 实例，请考虑为中间件构造函数传递 <xref:Microsoft.Extensions.Logging.Abstractions.NullLoggerFactory> 实例。

## <a name="affected-apis"></a>受影响的 API

[RequestLocalizationMiddleware.ctor(RequestDelegate, IOptions\<RequestLocalizationOptions>)](/dotnet/api/microsoft.aspnetcore.localization.requestlocalizationmiddleware.-ctor?view=aspnetcore-3.1#Microsoft_AspNetCore_Localization_RequestLocalizationMiddleware__ctor_Microsoft_AspNetCore_Http_RequestDelegate_Microsoft_Extensions_Options_IOptions_Microsoft_AspNetCore_Builder_RequestLocalizationOptions__)

<!--

### Category

ASP.NET Core

### Affected APIs

`M:Microsoft.AspNetCore.Localization.RequestLocalizationMiddleware.#ctor(Microsoft.AspNetCore.Http.RequestDelegate,Microsoft.Extensions.Options.IOptions{Microsoft.AspNetCore.Builder.RequestLocalizationOptions})`

-->
