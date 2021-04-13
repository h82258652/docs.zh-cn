---
title: 中断性变更：Blazor：Blazor 应用中的路由逻辑更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：Blazor 应用中的路由逻辑更改
ms.author: scaddie
ms.date: 12/14/2020
ms.openlocfilehash: cec05b26319fc8302261e41cc868dd73b46c8af0
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497822"
---
# <a name="blazor-route-precedence-logic-changed-in-blazor-apps"></a>Blazor：Blazor 应用中已更改路由优先逻辑

Blazor 路由实现中的 bug 影响了路由优先级的确定方式。 此 bug 会影响 Blazor 应用内的 catch-all 路由或带可选参数的路由。

## <a name="version-introduced"></a>引入的版本

5.0.1

## <a name="old-behavior"></a>旧行为

发生错误行为时，系统将优先考虑并匹配优先级较低的路由，而不是优先级较高的路由。 例如，它会先匹配 `/customer/{id}` 路由，后匹配 `{*slug}` 路由。

## <a name="new-behavior"></a>新行为

当前的行为会更精确地匹配 ASP.NET Core 应用中定义的路由。 框架首先确定每个网段的路由优先级。 路由的长度仅用作与中断相关的第二个条件。

## <a name="reason-for-change"></a>更改原因

最初的行为被视为实现中的一个 bug。 作为目标，Blazor 应用中路由系统的行为方式应与其他 ASP.NET Core 中路由系统的行为方式相同。

## <a name="recommended-action"></a>建议的操作

如果从 Blazor 的早期版本升级到 5.x，请使用 `Router` 组件上的 `PreferExactMatches` 属性。 此属性可用于选择正确的行为。 例如：

```razor
<Router AppAssembly="@typeof(Program).Assembly" PreferExactMatches="true">
```

如果将 `PreferExactMatches` 设置为 `true`，则路由匹配将优先选取精确匹配而非通配符匹配。

## <a name="affected-apis"></a>受影响的 API

无

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis

-->
