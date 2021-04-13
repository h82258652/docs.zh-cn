---
title: 中断性变更：HTTP：Kestrel 和 IIS BadHttpRequestException 类型标记为已过时并已替换
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：HTTP：Kestrel 和 IIS BadHttpRequestException 类型标记为已过时并已替换
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 5837018def634b732b4f273f1a794267f4d17972
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498134"
---
# <a name="http-kestrel-and-iis-badhttprequestexception-types-marked-obsolete-and-replaced"></a>HTTP：Kestrel 和 IIS BadHttpRequestException 类型标记为已过时并已替换

`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 已标记为已过时，并已更改为从 `Microsoft.AspNetCore.Http.BadHttpRequestException` 派生。 为了实现后向兼容性，Kestrel 和 IIS 服务器仍会引发旧的异常类型。 未来版本将删除这些过时的类型。

有关讨论，请参阅 [dotnet/aspnetcore#20614](https://github.com/dotnet/aspnetcore/issues/20614)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 4

## <a name="old-behavior"></a>旧行为

`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 派生自 <xref:System.IO.IOException?displayProperty=nameWithType>。

## <a name="new-behavior"></a>新行为

`Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 已过时。 这些类型还派生自 `Microsoft.AspNetCore.Http.BadHttpRequestException`（派生自 `System.IO.IOException`）。

## <a name="reason-for-change"></a>更改原因

此更改的目的是：

* 合并重复类型。
* 跨服务器实现统一行为。

现在，使用 Kestrel 或 IIS 时，应用可以捕获基本异常 `Microsoft.AspNetCore.Http.BadHttpRequestException`。

## <a name="recommended-action"></a>建议操作

用 `Microsoft.AspNetCore.Http.BadHttpRequestException` 替换 `Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException` 和 `Microsoft.AspNetCore.Server.IIS.BadHttpRequestException` 的用法。

## <a name="affected-apis"></a>受影响的 API

- [Microsoft.AspNetCore.Server.IIS.BadHttpRequestException](/dotnet/api/microsoft.aspnetcore.server.iis.badhttprequestexception?view=aspnetcore-3.1)
- [Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException](/dotnet/api/microsoft.aspnetcore.server.kestrel.badhttprequestexception?view=aspnetcore-1.1)

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.AspNetCore.Server.IIS.BadHttpRequestException`
- `T:Microsoft.AspNetCore.Server.Kestrel.BadHttpRequestException`

-->
