---
title: 中断性变更：Blazor：JSObjectReference 和 JSInProcessObjectReference 类型已更改为 internal
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：JSObjectReference 和 JSInProcessObjectReference 类型已更改为 internal
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 44e4c902ff22e18fa9ab8b484952082e5b81be94
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497848"
---
# <a name="blazor-jsobjectreference-and-jsinprocessobjectreference-types-changed-to-internal"></a>Blazor：JSObjectReference 和 JSInProcessObjectReference 类型已更改为 internal

ASP.NET Core 5.0 RC1 中引入的新的 `Microsoft.JSInterop.JSObjectReference` 和 `Microsoft.JSInterop.JSInProcessObjectReference` 类型已被标记为 `internal`。

## <a name="version-introduced"></a>引入的版本

5.0 RC2

## <a name="old-behavior"></a>旧行为

可以通过 `IJSRuntime` 从 JavaScript 互操作调用中获取 `JSObjectReference`。 例如：

```csharp
var jsObjectReference = await JSRuntime.InvokeAsync<JSObjectReference>(...);
```

## <a name="new-behavior"></a>新行为

`JSObjectReference` 使用 [internal](../../../../csharp/language-reference/keywords/internal.md) 访问修饰符。 必须改为使用 `public` `IJSObjectReference` 接口。 例如：

```csharp
var jsObjectReference = await JSRuntime.InvokeAsync<IJSObjectReference>(...);
```

`JSInProcessObjectReference` 也被标记为 `internal` 并由 `IJSInProcessObjectReference` 替换。

## <a name="reason-for-change"></a>更改原因

此更改使 JavaScript 互操作功能与 Blazor 中的其他模式更加一致。 `IJSObjectReference` 类似于 `IJSRuntime`，因为它有类似的目的，并且有类似的方法和扩展。

## <a name="recommended-action"></a>建议操作

分别用 `IJSObjectReference` 和 `IJSInProcessObjectReference` 替换出现的 `JSObjectReference` 和 `JSInProcessObjectReference`。

## <a name="affected-apis"></a>受影响的 API

- `Microsoft.JSInterop.JSObjectReference`
- `Microsoft.JSInterop.JSInProcessObjectReference`

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.JSInterop.JSObjectReference`
- `T:Microsoft.JSInterop.JSInProcessObjectReference`

-->
