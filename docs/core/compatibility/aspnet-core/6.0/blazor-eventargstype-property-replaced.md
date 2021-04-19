---
title: 中断性变更：Blazor：已替换 WebEventDescriptor.EventArgsType 属性
description: 了解 ASP.NET Core 6.0 中的中断性变更，其中 WebEventDescriptor.EventArgsType 属性被替换为 EventName 属性。
ms.author: scaddie
ms.date: 02/24/2021
no-loc:
- Blazor
ms.openlocfilehash: ed4bb3d19e6b78d751d24ae640f35c72bd15ed1f
ms.sourcegitcommit: fdfa01f6cd3aa4c36b6e8a1830693ff22d35aeea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107292296"
---
# <a name="blazor-no-loc-textwebeventdescriptoreventargstype-property-replaced"></a>Blazor：已替换 :::no-loc text="WebEventDescriptor.EventArgsType"::: 属性

<xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor> 类是 Blazor 内部协议的一部分，用于将事件从 JavaScript 传递到 .NET 中。 这个类通常不由应用代码使用，而是由平台作者使用。

从 ASP.NET Core 6.0 开始，`WebEventDescriptor` 上的 <xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType%2A> 属性被新的 `EventName` 属性所替代。 这一变更不太可能影响任何应用代码，因为这是一个低级别的平台实现细节。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 6.0

## <a name="old-behavior"></a>旧行为

在 ASP.NET Core 5.0 及更早版本中，属性 `EventArgsType` 为 DOM 事件类型组描述了一个非标准的、特定于 Blazor 的类别名称。 例如，`click` 和 `mousedown` 事件都映射到 `mouse` 的 `EventArgsType` 值。 同样，`cut`、`copy` 和 `paste` 事件被映射到 `clipboard` 的 `EventArgsType` 值。 这些类别名称用于确定将传入事件参数数据反序列化的 .NET 类型。

## <a name="new-behavior"></a>新行为

从 ASP.NET Core 6.0 开始，新属性 `EventName` 仅指定原始事件的名称。 例如，`click`、`mousedown`、`cut`、`copy` 或 `paste`。 不再需要提供特定于 Blazor 的类别名称。 因此，旧属性 `EventArgsType` 被删除。

## <a name="reason-for-change"></a>更改原因

在拉取请求 [dotnet/aspnetcore#29993](https://github.com/dotnet/aspnetcore/pull/29993) 中，引入了对自定义事件参数类的支持。 作为此支持的一部分，框架不再依赖于所有事件都符合预定义的类别集。 现在，框架只需要知道原始事件名称。

## <a name="recommended-action"></a>建议的操作

应用代码应该不受影响，不需要更改。

如果是构建自定义 Blazor 呈现平台，可能需要更新将事件调度到 `Renderer` 中的机制。 用提供原始事件名称的更简单逻辑替换任何关于事件类别的硬编码规则。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType%2A?displayProperty=nameWithType>

<!--

## Category

ASP.NET Core

## Affected APIs

`P:Microsoft.AspNetCore.Components.RenderTree.WebEventDescriptor.EventArgsType`

-->
