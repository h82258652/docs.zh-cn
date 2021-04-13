---
title: 中断性变更：Kestrel：默认支持的 TLS 协议版本已更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Kestrel：默认支持的 TLS 协议版本已更改
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 1e3ebd700e5bb603f95a8b20ebdbd61379f1e508
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497640"
---
# <a name="kestrel-default-supported-tls-protocol-versions-changed"></a>Kestrel：默认支持的 TLS 协议版本已更改

Kestrel 现在使用系统默认的 TLS 协议版本，而不是像以前一样将连接限制于 TLS 1.1 和 TLS 1.2 协议。

此更改允许：

* 在支持 TLS 1.3 的环境中默认使用它。
* 在某些环境（例如默认的 Windows Server 2016）中使用 TLS 1.0，但这通常[不理想](/security/engineering/solving-tls1-problem)。

有关讨论，请参阅 [dotnet/aspnetcore#22563](https://github.com/dotnet/aspnetcore/issues/22563)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 6

## <a name="old-behavior"></a>旧行为

Kestrel 过去默认要求连接使用 TLS 1.1 或 TLS 1.2。

## <a name="new-behavior"></a>新行为

Kestrel 允许操作系统选择要使用的最佳协议，并阻止不安全的协议。 <xref:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.SslProtocols%2A?displayProperty=nameWithType> 现在默认为 `SslProtocols.None`，而不是 `SslProtocols.Tls12 | SslProtocols.Tls11`。

## <a name="reason-for-change"></a>更改原因

进行此项更改是为了在 TLS 1.3 和未来的 TLS 版本可用时默认支持它们。

## <a name="recommended-action"></a>建议操作

应使用新的默认协议，除非应用有特定原因而不能使用它们。 验证你的系统是否配置为仅允许安全协议。

若要禁用旧协议，请执行以下操作之一：

* 参照 [Windows 说明](../../../../framework/network-programming/tls.md#configuring-schannel-protocols-in-the-windows-registry)，在系统范围禁用旧的协议（如 TLS 1.0）。 目前所有 Windows 版本上都默认启用该协议。
* 在代码中手动选择要支持的协议，如下所示：

    ```csharp
    using System.Security.Authentication;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Extensions.Hosting;

    public class Program
    {
        public static void Main(string[] args) =>
            CreateHostBuilder(args).Build().Run();

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseKestrel(kestrelOptions =>
                    {
                        kestrelOptions.ConfigureHttpsDefaults(httpsOptions =>
                        {
                            httpsOptions.SslProtocols = SslProtocols.Tls12 | SslProtocols.Tls13;
                        });
                    });

                    webBuilder.UseStartup<Startup>();
                });
    }
    ```

遗憾的是，没有用于排除特定协议的 API。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.SslProtocols%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`P:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.SslProtocols`

-->
