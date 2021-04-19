---
title: 中断性变更：更改了 signalr-protocol-msgpack 包中的 MessagePack 库
description: 了解 ASP.NET Core 6.0 中的中断性变更：更改了 MessagePack 库，并在 @microsoft/signalr-protocol-msgpack 包中删除了两个选项。
ms.date: 04/07/2021
ms.openlocfilehash: 62f853cbed0904e278e25f231528845baac9f71d
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107260026"
---
# <a name="changed-messagepack-library-in-microsoftsignalr-protocol-msgpack"></a>更改了 @microsoft/signalr-protocol-msgpack 中的 MessagePack 库

npm 包 [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) 现在引用 `@msgpack/msgpack`，而不是引用 `msgpack5`。 此外，更改了可选择性地传递到 `MessagePackHubProtocol` 的可用选项。 删除了 `MessagePackOptions.disableTimestampEncoding` 和 `MessagePackOptions.forceFloat64` 属性，并添加了一些新选项。

有关讨论内容，请参阅 <https://github.com/dotnet/aspnetcore/issues/30471>。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 6.0

## <a name="old-behavior"></a>旧行为

在以前的版本中，必须包含三个脚本引用才能在浏览器中使用 [MessagePack Hub 协议](/aspnet/core/signalr/messagepackhubprotocol)：

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

## <a name="new-behavior"></a>新行为

从 ASP.NET Core 6 开始，只需要两个脚本引用即可在浏览器中使用 [MessagePack Hub 协议](/aspnet/core/signalr/messagepackhubprotocol)：

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

如果想要直接在应用中使用，则会将 `@msgpack/msgpack` 包（而不是 `msgpack5` 包）下载到 node_modules 目录中。

最后，`MessagePackOptions` 具有新的附加属性，并且删除了 `disableTimestampEncoding` 和 `forceFloat64` 属性。

## <a name="reason-for-change"></a>更改原因

进行此更改是为了减小资产大小、更简单地使用包并添加更多可定制性。

## <a name="recommended-action"></a>建议操作

如果以前在应用中使用 `msgpack5`，则需要在 package.json 文件中添加对库的直接引用。

## <a name="affected-apis"></a>受影响的 API

删除了以下 API：

- `MessagePackOptions.disableTimestampEncoding`
- `MessagePackOptions.forceFloat64`

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis.

-->
