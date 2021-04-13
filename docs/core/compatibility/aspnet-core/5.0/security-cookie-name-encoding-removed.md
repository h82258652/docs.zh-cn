---
title: 中断性变更：安全性：Cookie 名称编码已删除
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：安全性：Cookie 名称编码已删除
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: a50ef0ee34b7148d13b01f96e754d1cb47d40819
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497881"
---
# <a name="security-cookie-name-encoding-removed"></a>安全性：Cookie 名称编码已删除

[HTTP Cookie 标准](https://tools.ietf.org/html/rfc6265#section-4.1.1) 仅允许在 Cookie 名称和值中使用特定字符。 为了支持不允许的字符，ASP.NET Core：

* 在创建响应 Cookie 时进行编码。
* 在读取请求 Cookie 时进行解码。

在 ASP.NET Core 5.0 中，此编码行为因安全问题发生了更改。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#23578](https://github.com/dotnet/aspnetcore/issues/23578)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 8

## <a name="old-behavior"></a>旧行为

对响应 Cookie 名称进行编码。 对请求 Cookie 名称进行解码。

## <a name="new-behavior"></a>新行为

删除了 Cookie 名称的编码和解码。 对于先前[受支持的 ASP.NET Core 版本](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)，团队计划就地缓解解码问题。 此外，使用无效的 Cookie 名称调用 <xref:Microsoft.AspNetCore.Http.IResponseCookies.Append%2A?displayProperty=nameWithType> 将引发 <xref:System.ArgumentException> 类型的异常。 Cookie 值的编码和解码保持不变。

## <a name="reason-for-change"></a>更改原因

在[多个 Web 框架](https://github.com/advisories/GHSA-j6w9-fv6q-3q52)中发现了问题。 编码和解码可以使攻击者使用诸如 `__%48ost-` 的编码值来假冒 `__Host-` 之类的保留前缀，从而绕过称为 [Cookie 前缀](https://tools.ietf.org/html/draft-ietf-httpbis-cookie-prefixes-00)的安全功能。 此攻击需要利用辅助攻击（例如跨站点脚本 (XSS) 漏洞）在网站中注入欺诈性 Cookie。 默认情况下，ASP.NET Core 或者 `Microsoft.Owin` 库或模板中不使用这些前缀。

## <a name="recommended-action"></a>建议操作

如果要将项目迁移到 ASP.NET Core 5.0 或更高版本，请确保其 Cookie 名称符合[令牌规范要求](https://tools.ietf.org/html/rfc2616#section-2.2)：ASCII 字符不包括控件和分隔符 `"(" | ")" | "<" | ">" | "@" | "," | ";" | ":" | "\" | <"> | "/" | "[" | "]" | "?" | "=" | "{" | "}" | SP | HT`。 如果在 Cookie 名称或其他 HTTP 标头中使用非 ASCII 字符，则可能导致服务器异常或客户端不正确地往返。

## <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Http.HttpRequest.Cookies%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.HttpResponse.Cookies%2A?displayProperty=nameWithType>
- `Microsoft.Owin.IOwinRequest.Cookies`
- `Microsoft.Owin.IOwinResponse.Cookies`

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.AspNetCore.Http.HttpRequest.Cookies`
- `Overload:Microsoft.AspNetCore.Http.HttpResponse.Cookies`
- `P:Microsoft.Owin.IOwinRequest.Cookies`
- `P:Microsoft.Owin.IOwinResponse.Cookies`

-->
