---
title: 中断性变更：SignalR：MessagePack 集线器协议已移至 MessagePack 2.x 包
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：SignalR：MessagePack 集线器协议已移至 MessagePack 2.x 包
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 6851c4fdbaab61e7655dd71ba3c21b8835b2b855
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497107"
---
# <a name="signalr-messagepack-hub-protocol-moved-to-messagepack-2x-package"></a>SignalR：MessagePack 集线器协议已移至 MessagePack 2.x 包

ASP.NET Core SignalR [MessagePack 集线器协议](/aspnet/core/signalr/messagepackhubprotocol)使用 [MessagePack NuGet 包](https://www.nuget.org/packages/MessagePack)实现 MessagePack 序列化。 ASP.NET Core 5.0 将包从 1.x 升级到最新的 2.x 包版本。

有关此问题的讨论，请参阅 [dotnet/aspnetcore#18692](https://github.com/dotnet/aspnetcore/issues/18692)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 1

## <a name="old-behavior"></a>旧行为

ASP.NET Core SignalR 以前使用 MessagePack 1.x 包对 MessagePack 消息进行序列化和反序列化。

## <a name="new-behavior"></a>新行为

ASP.NET Core SignalR 使用 MessagePack 2.x 包对 MessagePack 消息进行序列化和反序列化。

## <a name="reason-for-change"></a>更改原因

MessagePack 2.x 包中的最新改进添加了有用的功能。

## <a name="recommended-action"></a>建议操作

此重大更改适用于以下情况：

* 在 <xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions> 上设置或配置值。
* 直接使用 MessagePack API，以及在同一项目中使用 ASP.NET Core SignalR MessagePack 集线器协议。 将加载较新的版本，而不是以前的版本。

若要获得包创建者的迁移指导，请参阅[从 MessagePack v1.x 迁移到 MessagePack v2.x](https://github.com/neuecc/MessagePack-CSharp/blob/master/doc/migration.md)。 消息序列化和反序列化的某些方面会受到影响。 具体而言，[对 DateTime 值的序列化方式进行了行为上的更改](https://github.com/neuecc/MessagePack-CSharp/blob/master/doc/migration.md#behavioral-changes)。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions`

-->
