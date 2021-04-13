---
title: 中断性变更：Kestrel：Libuv 传输标记为已过时
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Kestrel：Libuv 传输标记为已过时
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 7edec666df729edbffe60b6017a2b8ee5f46ae67
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497627"
---
# <a name="kestrel-libuv-transport-marked-as-obsolete"></a>Kestrel：Libuv 传输标记为已过时

早期版本的 ASP.NET Core 使用 Libuv 作为执行异步输入和输出的方法的实现细节。 在 ASP.NET Core 2.0 中，我们开发了一种替代方案：基于 <xref:System.Net.Sockets.Socket> 的传输。 在 ASP.NET Core 2.1 中，默认情况下，Kestrel 切换为使用基于 `Socket` 的传输。 出于兼容性原因，保留了 Libuv 支持。

目前人们普遍使用基于 `Socket` 的传输，而不使用 Libuv 传输。 因此，Libuv 支持在 .NET 5.0 中标记为已过时，并将完全从 .NET 6.0 中删除。

作为此更改的一部分，.NET 5.0 时间范围内不会添加对新操作系统平台（例如 Windows ARM64）的 Libuv 支持。

有关阻止需要使用 Libuv 传输的问题的讨论，请参阅 GitHub 问题，网址为：[dotnet/aspnetcore#23409](https://github.com/dotnet/aspnetcore/issues/23409)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 8

## <a name="old-behavior"></a>旧行为

Libuv API 未标记为已过时。

## <a name="new-behavior"></a>新行为

Libuv API 标记为已过时。

## <a name="reason-for-change"></a>更改原因

基于 `Socket` 的传输是默认设置。 不必继续使用 Libuv 传输。

## <a name="recommended-action"></a>建议操作

停止使用 [Libuv 包](https://www.nuget.org/packages/Libuv) 和扩展方法。

## <a name="affected-apis"></a>受影响的 API

- [WebHostBuilderLibuvExtensions](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions?view=aspnetcore-3.0)
- [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv?view=aspnetcore-3.0)
- [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.transport.libuv.libuvtransportoptions?view=aspnetcore-3.0)
- [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.ThreadCount](/dotnet/api/microsoft.aspnetcore.server.kestrel.transport.libuv.libuvtransportoptions.threadcount?view=aspnetcore-3.0)
- [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.NoDelay](/dotnet/api/microsoft.aspnetcore.server.kestrel.transport.libuv.libuvtransportoptions.nodelay?view=aspnetcore-3.0)
- [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.MaxWriteBufferSize](/dotnet/api/microsoft.aspnetcore.server.kestrel.transport.libuv.libuvtransportoptions.maxwritebuffersize?view=aspnetcore-3.0)
- [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.MaxReadBufferSize](/dotnet/api/microsoft.aspnetcore.server.kestrel.transport.libuv.libuvtransportoptions.maxreadbuffersize?view=aspnetcore-3.0)
- `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.Backlog`

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions`
- `Overload:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv`
- `T:Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions`
- `P:Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.ThreadCount`
- `P:Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.NoDelay`
- `P:Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.MaxWriteBufferSize`
- `P:Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.MaxReadBufferSize`
- `P:Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv.LibuvTransportOptions.Backlog`

-->
