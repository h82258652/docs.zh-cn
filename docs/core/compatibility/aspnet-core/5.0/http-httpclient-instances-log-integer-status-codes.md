---
title: 中断性变更：HTTP：IHttpClientFactory 创建的 HttpClient 实例记录整数状态代码
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：HTTP：IHttpClientFactory 创建的 HttpClient 实例记录整数状态代码
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 59e64c7127a97b1c11587d13803e4a085dbc090b
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497679"
---
# <a name="http-httpclient-instances-created-by-ihttpclientfactory-log-integer-status-codes"></a>HTTP：IHttpClientFactory 创建的 HttpClient 实例记录整数状态代码

<xref:System.Net.Http.IHttpClientFactory> 创建的 <xref:System.Net.Http.HttpClient> 实例将 HTTP 状态代码记录为整数而不是状态代码名称。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 1

## <a name="old-behavior"></a>旧行为

日志记录使用 HTTP 状态代码的文本说明。 考虑以下日志消息：

```output
Received HTTP response after 56.0044ms - OK
End processing HTTP request after 70.0862ms - OK
```

## <a name="new-behavior"></a>新行为

日志记录使用 HTTP 状态代码的整数值。 考虑以下日志消息：

```output
Received HTTP response after 56.0044ms - 200
End processing HTTP request after 70.0862ms - 200
```

## <a name="reason-for-change"></a>更改原因

此日志记录的原始行为与始终使用整数值的 ASP.NET Core 的其他部分不一致。 不一致使通过结构化日志记录系统（如 [Elasticsearch](https://www.elastic.co/elasticsearch/)）查询日志变得困难。 有关详细信息，请参阅 [dotnet/extensions#1549](https://github.com/dotnet/extensions/issues/1549)。

使用整数值比文本更灵活，因为它允许对值范围进行查询。

考虑添加另一个日志值来捕获整数状态代码。 遗憾的是，这样做会导致与 ASP.NET Core 的其余部分不一致。 HttpClient 日志记录和 HTTP 服务器/托管日志记录使用相同的 `StatusCode` 密钥名称。

## <a name="recommended-action"></a>建议操作

最佳选择是更新日志记录查询，以使用状态代码的整数值。 这种方式可能会导致跨多个 ASP.NET Core 版本编写查询时遇到一些困难。 但是出于此目的使用整数查询日志会更加灵活。

如果需要强制与旧行为兼容并使用文本状态代码，请将 `IHttpClientFactory` 日志记录替换为你自己的日志记录：

1. 将以下类的 .NET Core 3.1 版本复制到项目中：

    * [LoggingHttpMessageHandlerBuilderFilter](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingHttpMessageHandlerBuilderFilter.cs)
    * [LoggingHttpMessageHandler](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingHttpMessageHandler.cs)
    * [LoggingScopeHttpMessageHandler](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingScopeHttpMessageHandler.cs)
    * [HttpHeadersLogValue](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/HttpHeadersLogValue.cs)

1. 重命名类，以避免与 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http) NuGet 包中的公共类型发生冲突。

1. 在项目的 `Startup.ConfigureServices` 方法中，将 `LoggingHttpMessageHandlerBuilderFilter` 的内置实现替换为自己的实现。 例如：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // Other service registrations go first. Code omitted for brevity.

        // Place the following after all AddHttpClient registrations.
        services.RemoveAll<IHttpMessageHandlerBuilderFilter>();

        services.AddSingleton<IHttpMessageHandlerBuilderFilter,
                              MyLoggingHttpMessageHandlerBuilderFilter>();
    }
    ```

## <a name="affected-apis"></a>受影响的 API

<xref:System.Net.Http.HttpClient?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:System.Net.Http.HttpClient`

-->
