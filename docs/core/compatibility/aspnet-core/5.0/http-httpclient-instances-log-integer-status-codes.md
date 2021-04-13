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
# <a name="http-httpclient-instances-created-by-ihttpclientfactory-log-integer-status-codes"></a><span data-ttu-id="cb510-103">HTTP：IHttpClientFactory 创建的 HttpClient 实例记录整数状态代码</span><span class="sxs-lookup"><span data-stu-id="cb510-103">HTTP: HttpClient instances created by IHttpClientFactory log integer status codes</span></span>

<span data-ttu-id="cb510-104"><xref:System.Net.Http.IHttpClientFactory> 创建的 <xref:System.Net.Http.HttpClient> 实例将 HTTP 状态代码记录为整数而不是状态代码名称。</span><span class="sxs-lookup"><span data-stu-id="cb510-104"><xref:System.Net.Http.HttpClient> instances created by <xref:System.Net.Http.IHttpClientFactory> log HTTP status codes as integers instead of with status code names.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="cb510-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="cb510-105">Version introduced</span></span>

<span data-ttu-id="cb510-106">5.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="cb510-106">5.0 Preview 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="cb510-107">旧行为</span><span class="sxs-lookup"><span data-stu-id="cb510-107">Old behavior</span></span>

<span data-ttu-id="cb510-108">日志记录使用 HTTP 状态代码的文本说明。</span><span class="sxs-lookup"><span data-stu-id="cb510-108">Logging uses the textual descriptions of HTTP status codes.</span></span> <span data-ttu-id="cb510-109">考虑以下日志消息：</span><span class="sxs-lookup"><span data-stu-id="cb510-109">Consider the following log messages:</span></span>

```output
Received HTTP response after 56.0044ms - OK
End processing HTTP request after 70.0862ms - OK
```

## <a name="new-behavior"></a><span data-ttu-id="cb510-110">新行为</span><span class="sxs-lookup"><span data-stu-id="cb510-110">New behavior</span></span>

<span data-ttu-id="cb510-111">日志记录使用 HTTP 状态代码的整数值。</span><span class="sxs-lookup"><span data-stu-id="cb510-111">Logging uses the integer values of HTTP status codes.</span></span> <span data-ttu-id="cb510-112">考虑以下日志消息：</span><span class="sxs-lookup"><span data-stu-id="cb510-112">Consider the following log messages:</span></span>

```output
Received HTTP response after 56.0044ms - 200
End processing HTTP request after 70.0862ms - 200
```

## <a name="reason-for-change"></a><span data-ttu-id="cb510-113">更改原因</span><span class="sxs-lookup"><span data-stu-id="cb510-113">Reason for change</span></span>

<span data-ttu-id="cb510-114">此日志记录的原始行为与始终使用整数值的 ASP.NET Core 的其他部分不一致。</span><span class="sxs-lookup"><span data-stu-id="cb510-114">The original behavior of this logging is inconsistent with other parts of ASP.NET Core that have always used integer values.</span></span> <span data-ttu-id="cb510-115">不一致使通过结构化日志记录系统（如 [Elasticsearch](https://www.elastic.co/elasticsearch/)）查询日志变得困难。</span><span class="sxs-lookup"><span data-stu-id="cb510-115">The inconsistency makes logs difficult to query via structured logging systems such as [Elasticsearch](https://www.elastic.co/elasticsearch/).</span></span> <span data-ttu-id="cb510-116">有关详细信息，请参阅 [dotnet/extensions#1549](https://github.com/dotnet/extensions/issues/1549)。</span><span class="sxs-lookup"><span data-stu-id="cb510-116">For more context, see [dotnet/extensions#1549](https://github.com/dotnet/extensions/issues/1549).</span></span>

<span data-ttu-id="cb510-117">使用整数值比文本更灵活，因为它允许对值范围进行查询。</span><span class="sxs-lookup"><span data-stu-id="cb510-117">Using integer values is more flexible than text because it allows queries on ranges of values.</span></span>

<span data-ttu-id="cb510-118">考虑添加另一个日志值来捕获整数状态代码。</span><span class="sxs-lookup"><span data-stu-id="cb510-118">Adding another log value to capture the integer status code was considered.</span></span> <span data-ttu-id="cb510-119">遗憾的是，这样做会导致与 ASP.NET Core 的其余部分不一致。</span><span class="sxs-lookup"><span data-stu-id="cb510-119">Unfortunately, doing so would introduce another inconsistency with the rest of ASP.NET Core.</span></span> <span data-ttu-id="cb510-120">HttpClient 日志记录和 HTTP 服务器/托管日志记录使用相同的 `StatusCode` 密钥名称。</span><span class="sxs-lookup"><span data-stu-id="cb510-120">HttpClient logging and HTTP server/hosting logging use the same `StatusCode` key name already.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="cb510-121">建议操作</span><span class="sxs-lookup"><span data-stu-id="cb510-121">Recommended action</span></span>

<span data-ttu-id="cb510-122">最佳选择是更新日志记录查询，以使用状态代码的整数值。</span><span class="sxs-lookup"><span data-stu-id="cb510-122">The best option is to update logging queries to use the integer values of status codes.</span></span> <span data-ttu-id="cb510-123">这种方式可能会导致跨多个 ASP.NET Core 版本编写查询时遇到一些困难。</span><span class="sxs-lookup"><span data-stu-id="cb510-123">This option may cause some difficulty writing queries across multiple ASP.NET Core versions.</span></span> <span data-ttu-id="cb510-124">但是出于此目的使用整数查询日志会更加灵活。</span><span class="sxs-lookup"><span data-stu-id="cb510-124">However, using integers for this purpose is much more flexible for querying logs.</span></span>

<span data-ttu-id="cb510-125">如果需要强制与旧行为兼容并使用文本状态代码，请将 `IHttpClientFactory` 日志记录替换为你自己的日志记录：</span><span class="sxs-lookup"><span data-stu-id="cb510-125">If you need to force compatibility with the old behavior and use textual status codes, replace the `IHttpClientFactory` logging with your own:</span></span>

1. <span data-ttu-id="cb510-126">将以下类的 .NET Core 3.1 版本复制到项目中：</span><span class="sxs-lookup"><span data-stu-id="cb510-126">Copy the .NET Core 3.1 versions of the following classes into your project:</span></span>

    * [<span data-ttu-id="cb510-127">LoggingHttpMessageHandlerBuilderFilter</span><span class="sxs-lookup"><span data-stu-id="cb510-127">LoggingHttpMessageHandlerBuilderFilter</span></span>](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingHttpMessageHandlerBuilderFilter.cs)
    * [<span data-ttu-id="cb510-128">LoggingHttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="cb510-128">LoggingHttpMessageHandler</span></span>](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingHttpMessageHandler.cs)
    * [<span data-ttu-id="cb510-129">LoggingScopeHttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="cb510-129">LoggingScopeHttpMessageHandler</span></span>](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/LoggingScopeHttpMessageHandler.cs)
    * [<span data-ttu-id="cb510-130">HttpHeadersLogValue</span><span class="sxs-lookup"><span data-stu-id="cb510-130">HttpHeadersLogValue</span></span>](https://github.com/dotnet/extensions/blob/release/3.1/src/HttpClientFactory/Http/src/Logging/HttpHeadersLogValue.cs)

1. <span data-ttu-id="cb510-131">重命名类，以避免与 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http) NuGet 包中的公共类型发生冲突。</span><span class="sxs-lookup"><span data-stu-id="cb510-131">Rename the classes to avoid conflicts with public types in the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http) NuGet package.</span></span>

1. <span data-ttu-id="cb510-132">在项目的 `Startup.ConfigureServices` 方法中，将 `LoggingHttpMessageHandlerBuilderFilter` 的内置实现替换为自己的实现。</span><span class="sxs-lookup"><span data-stu-id="cb510-132">Replace the built-in implementation of `LoggingHttpMessageHandlerBuilderFilter` with your own in the project's `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="cb510-133">例如：</span><span class="sxs-lookup"><span data-stu-id="cb510-133">For example:</span></span>

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

## <a name="affected-apis"></a><span data-ttu-id="cb510-134">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="cb510-134">Affected APIs</span></span>

<xref:System.Net.Http.HttpClient?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:System.Net.Http.HttpClient`

-->
