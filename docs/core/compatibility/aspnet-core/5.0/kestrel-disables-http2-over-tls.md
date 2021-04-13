---
title: 中断性变更：Kestrel：在不兼容的 Windows 版本上通过 TLS 禁用 HTTP/2
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Kestrel：在不兼容的 Windows 版本上通过 TLS 禁用 HTTP/2
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: b8bbe113b3357508b6af29fa9a4d1431f6a6acea
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498108"
---
# <a name="kestrel-http2-disabled-over-tls-on-incompatible-windows-versions"></a>Kestrel：在不兼容的 Windows 版本上通过 TLS 禁用 HTTP/2

要在 Windows 上启用基于传输层安全性 (TLS) 的 HTTP/2，需要满足以下两个要求：

- 应用层协议协商 (ALPN) 支持，从 Windows 8.1 和 Windows Server 2012 R2 开始提供。
- 与 HTTP/2 兼容的一组密码，从 Windows 10 和 Windows Server 2016 开始提供。

因此，配置基于 TLS 的 HTTP/2 时，Kestrel 的行为已更改为：

- 当 [ListenOptions.HttpProtocols](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.httpprotocols) 设置为 `Http1AndHttp2` 时，降级到 `Http1` 并记录 `Information` 级别的消息。 `Http1AndHttp2` 是 `ListenOptions.HttpProtocols` 的默认值。
- 当 `ListenOptions.HttpProtocols` 设置为 `Http2` 时，引发 `NotSupportedException`。

有关讨论，请参阅问题 [dotnet/aspnetcore#23068](https://github.com/dotnet/aspnetcore/issues/23068)。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 5.0 预览版 7

## <a name="old-behavior"></a>旧行为

下表概述了配置基于 TLS 的 HTTP/2 时的行为。

| 协议 | Windows 7、<br />Windows Server 2008 R2<br />或更早版本 | Windows 8、<br />Windows Server 2012 | Windows 8.1、<br />Windows Server 2012 R2 | Windows 10、<br />Windows Server 2016<br />或更高版本 |
|---------------|-----------------------------------------------|--------------------------------|-------------------------------------|------------------------------------------|
| `Http2`         | 引发 `NotSupportedException`                   | TLS 握手期间出错     | TLS 握手期间出错 &ast;     | 无错误 |
| `Http1AndHttp2` | 降级到 `Http1`                    | 降级到 `Http1`     | TLS 握手期间出错 &ast;     | 无错误 |

&ast; 配置兼容密码套件以支持这些方案。

## <a name="new-behavior"></a>新行为

下表概述了配置基于 TLS 的 HTTP/2 时的行为。

| 协议 | Windows 7、<br />Windows Server 2008 R2<br />或更早版本 | Windows 8、<br />Windows Server 2012 | Windows 8.1、<br />Windows Server 2012 R2 | Windows 10、<br />Windows Server 2016<br />或更高版本 |
|---------------|-----------------------------------------------|--------------------------------|-------------------------------------|------------------------------------------|
| `Http2`         | 引发 `NotSupportedException`                   | 引发 `NotSupportedException`     | 引发 `NotSupportedException` &ast;&ast;     | 无错误 |
| `Http1AndHttp2` | 降级到 `Http1`                    | 降级到 `Http1`     | 降级到 `Http1` &ast;&ast;     | 无错误 |

&ast;&ast; 配置兼容的密码套件并将应用上下文切换 `Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2` 设置为 `true` 以启用这些方案。

## <a name="reason-for-change"></a>更改原因

此更改可确保在较早的 Windows 版本上尽早、尽可能清晰地呈现基于 TLS 的 HTTP/2 的兼容性错误。

## <a name="recommended-action"></a>建议操作

确保在不兼容的 Windows 版本上禁用基于 TLS 的 HTTP/2。 Windows 8.1 和 Windows Server 2012 R2 是不兼容的，因为默认情况下它们缺少必要的密码。 但是，可以将“计算机配置”设置更新为使用 HTTP/2 兼容密码。 有关详细信息，请参阅 [Windows 8.1 中的 TLS 密码套件](/windows/win32/secauthn/tls-cipher-suites-in-windows-8-1)。 配置完成后，必须通过设置应用上下文切换 `Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2`，在 Kestrel 上启用基于 TLS 的 HTTP/2。 例如：

```csharp
AppContext.SetSwitch("Microsoft.AspNetCore.Server.Kestrel.EnableWindows81Http2", true);
```

未更改基础支持。 例如，基于 TLS 的 HTTP/2 在 Windows 8 或 Windows Server 2012 上始终无效。 此更改将修改这些不受支持的方案中错误的呈现方式。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
