---
title: 中断性变更：IIS：保留 UrlRewrite 中间件查询字符串
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：IIS：保留 UrlRewrite 中间件查询字符串
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 3439844f04c89059a027c1264401efc46cd0d57e
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497654"
---
# <a name="iis-urlrewrite-middleware-query-strings-are-preserved"></a>IIS：保留 UrlRewrite 中间件查询字符串

IIS UrlRewrite 中间件缺陷阻止了在重写规则中保留查询字符串。 此缺陷已修复，以与 IIS UrlRewrite 模块的行为保持一致。

有关讨论，请参阅问题 [dotnet/aspnetcore#22972](https://github.com/dotnet/aspnetcore/issues/22972)。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 5.0 预览版 7

## <a name="old-behavior"></a>旧行为

考虑以下重写规则：

```xml
<rule name="MyRule" stopProcessing="true">
  <match url="^about" />
  <action type="Redirect" url="/contact" redirectType="Temporary" appendQueryString="true" />
</rule>
```

前面的规则不追加查询字符串。 URI（如 `/about?id=1`）会重定向到 `/contact` 而不是 `/contact?id=1`。 `appendQueryString` 属性也默认为 `true`。

## <a name="new-behavior"></a>新行为

保留查询字符串。 [旧行为](#old-behavior)的示例中的 URI 将为 `/contact?id=1`。

## <a name="reason-for-change"></a>更改原因

旧行为与 IIS UrlRewrite 模块的行为不匹配。 若要支持在中间件与模块之间进行移植，应以保持一致的行为为目标。

## <a name="recommended-action"></a>建议操作

如果首选删除查询字符串的行为，请将 `action` 元素设置为 `appendQueryString="false"`。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`Overload:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite`

-->
