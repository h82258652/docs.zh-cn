---
title: 通过 Azure SDK for .NET 启用日志记录
description: 了解如何使用 Azure SDK for .NET 客户端库启用日志记录
ms.date: 03/20/2020
ms.custom: devx-track-dotnet
ms.author: casoper
author: camsoper
ms.openlocfilehash: b0f7bdf7eaf7db242545d43856fae8d1548bc004
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494332"
---
# <a name="logging-with-the-azure-sdk-for-net"></a><span data-ttu-id="ba36b-103">通过 Azure SDK for .NET 启用日志记录</span><span class="sxs-lookup"><span data-stu-id="ba36b-103">Logging with the Azure SDK for .NET</span></span>

<span data-ttu-id="ba36b-104">[Azure SDK](https://azure.microsoft.com/downloads/) for .NET 客户端库包括记录客户端库操作的功能。</span><span class="sxs-lookup"><span data-stu-id="ba36b-104">The [Azure SDK](https://azure.microsoft.com/downloads/) for .NET client libraries includes the ability to log client library operations.</span></span> <span data-ttu-id="ba36b-105">这使你可以监视客户端库对 Azure 服务进行的 I/O 请求和响应。</span><span class="sxs-lookup"><span data-stu-id="ba36b-105">This allows you to monitor I/O requests and responses that client libraries are making to Azure services.</span></span> <span data-ttu-id="ba36b-106">通常，日志用于调试或诊断通信问题。</span><span class="sxs-lookup"><span data-stu-id="ba36b-106">Typically, the logs are used to debug or diagnose communication issues.</span></span> <span data-ttu-id="ba36b-107">本文介绍了三种通过 Azure SDK for .NET 启用日志记录的方法：</span><span class="sxs-lookup"><span data-stu-id="ba36b-107">This article describes three approaches to enable logging with the Azure SDK for .NET:</span></span>

- <span data-ttu-id="ba36b-108">记录到控制台窗口</span><span class="sxs-lookup"><span data-stu-id="ba36b-108">Log to the console window</span></span>
- <span data-ttu-id="ba36b-109">记录到 .NET 诊断跟踪</span><span class="sxs-lookup"><span data-stu-id="ba36b-109">Log to .NET diagnostics traces</span></span>
- <span data-ttu-id="ba36b-110">配置自定义日志记录</span><span class="sxs-lookup"><span data-stu-id="ba36b-110">Configure custom logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba36b-111">本文适用于使用最新版 Azure SDK for .NET 的客户端库。</span><span class="sxs-lookup"><span data-stu-id="ba36b-111">This article applies to client libraries that use the most recent versions of the Azure SDK for .NET.</span></span> <span data-ttu-id="ba36b-112">若要查看库是否受支持，请参阅 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/index.html)列表。</span><span class="sxs-lookup"><span data-stu-id="ba36b-112">To see if a library is supported, refer to the list of [Azure SDK latest releases](https://azure.github.io/azure-sdk/releases/latest/index.html).</span></span> <span data-ttu-id="ba36b-113">如果应用程序使用旧版 Azure SDK 客户端库，请参阅适用服务文档中的具体说明。</span><span class="sxs-lookup"><span data-stu-id="ba36b-113">If your application is using an older version of the Azure SDK client libraries, refer to specific instructions in the applicable service documentation.</span></span>

## <a name="log-information"></a><span data-ttu-id="ba36b-114">记录信息</span><span class="sxs-lookup"><span data-stu-id="ba36b-114">Log information</span></span>

<span data-ttu-id="ba36b-115">SDK 记录以下信息，清理参数查询和标头值以删除个人数据。</span><span class="sxs-lookup"><span data-stu-id="ba36b-115">The SDK logs the following information, sanitizing parameter query and header values to remove personal data.</span></span>

<span data-ttu-id="ba36b-116">HTTP 请求日志条目：</span><span class="sxs-lookup"><span data-stu-id="ba36b-116">HTTP request log entry:</span></span>

- <span data-ttu-id="ba36b-117">唯一 ID</span><span class="sxs-lookup"><span data-stu-id="ba36b-117">Unique ID</span></span>
- <span data-ttu-id="ba36b-118">HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="ba36b-118">HTTP method</span></span>
- <span data-ttu-id="ba36b-119">URI</span><span class="sxs-lookup"><span data-stu-id="ba36b-119">URI</span></span>
- <span data-ttu-id="ba36b-120">传出请求标头</span><span class="sxs-lookup"><span data-stu-id="ba36b-120">Outgoing request headers</span></span>

<span data-ttu-id="ba36b-121">HTTP 响应日志条目：</span><span class="sxs-lookup"><span data-stu-id="ba36b-121">HTTP response log entry:</span></span>

- <span data-ttu-id="ba36b-122">I/O 操作的持续时间（已用时间）</span><span class="sxs-lookup"><span data-stu-id="ba36b-122">Duration of I/O operation (time elapsed)</span></span>
- <span data-ttu-id="ba36b-123">请求 ID</span><span class="sxs-lookup"><span data-stu-id="ba36b-123">Request ID</span></span>
- <span data-ttu-id="ba36b-124">HTTP 状态代码</span><span class="sxs-lookup"><span data-stu-id="ba36b-124">HTTP status code</span></span>
- <span data-ttu-id="ba36b-125">HTTP 原因短语</span><span class="sxs-lookup"><span data-stu-id="ba36b-125">HTTP reason phrase</span></span>
- <span data-ttu-id="ba36b-126">响应头</span><span class="sxs-lookup"><span data-stu-id="ba36b-126">Response headers</span></span>
- <span data-ttu-id="ba36b-127">错误信息（如果适用）</span><span class="sxs-lookup"><span data-stu-id="ba36b-127">Error information, when applicable</span></span>

<span data-ttu-id="ba36b-128">对于请求和响应内容：</span><span class="sxs-lookup"><span data-stu-id="ba36b-128">For request and response content:</span></span>

- <span data-ttu-id="ba36b-129">内容流是文本形式还是字节形式，取决于 Content-Type 标头。</span><span class="sxs-lookup"><span data-stu-id="ba36b-129">Content stream as text or bytes depending on the Content-Type header.</span></span>
     > [!NOTE]
     > <span data-ttu-id="ba36b-130">内容日志记录默认处于禁用状态。</span><span class="sxs-lookup"><span data-stu-id="ba36b-130">Content logging is disabled by default.</span></span> <span data-ttu-id="ba36b-131">要启用它，请将 `ClientOptions` 中的 `Diagnostics.IsLoggingContentEnabled` 设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="ba36b-131">To enable it, set `Diagnostics.IsLoggingContentEnabled` to `true` in `ClientOptions`.</span></span>

<span data-ttu-id="ba36b-132">通常以以下三个级别之一输出事件日志：</span><span class="sxs-lookup"><span data-stu-id="ba36b-132">Event logs are output usually at one of these three levels:</span></span>

- <span data-ttu-id="ba36b-133">有关请求和响应事件的信息</span><span class="sxs-lookup"><span data-stu-id="ba36b-133">Informational for request and response events</span></span>
- <span data-ttu-id="ba36b-134">错误警告</span><span class="sxs-lookup"><span data-stu-id="ba36b-134">Warning for errors</span></span>
- <span data-ttu-id="ba36b-135">详细消息和内容记录的详细信息</span><span class="sxs-lookup"><span data-stu-id="ba36b-135">Verbose for detailed messages and content logging</span></span>

## <a name="enable-logging-with-built-in-methods"></a><span data-ttu-id="ba36b-136">使用内置方法启用日志记录</span><span class="sxs-lookup"><span data-stu-id="ba36b-136">Enable logging with built-in methods</span></span>

<span data-ttu-id="ba36b-137">Azure SDK for .NET 客户端库通过 [`EventSource` 类](/dotnet/api/system.diagnostics.tracing.eventsource)将事件记录到 Windows 事件跟踪 (ETW)，这是 .NET 的典型功能。</span><span class="sxs-lookup"><span data-stu-id="ba36b-137">The Azure SDK for .NET client libraries log events to Event Tracing for Windows (ETW) via the [`EventSource` class](/dotnet/api/system.diagnostics.tracing.eventsource), which is typical for .NET.</span></span> <span data-ttu-id="ba36b-138">事件源使你能够以最小的性能开销在应用程序代码中使用结构化日志记录。</span><span class="sxs-lookup"><span data-stu-id="ba36b-138">Event sources allow you to use structured logging in your application code with a minimal performance overhead.</span></span> <span data-ttu-id="ba36b-139">要访问这些事件日志，需要注册事件侦听器。</span><span class="sxs-lookup"><span data-stu-id="ba36b-139">To gain access to these event logs, you need to register event listeners.</span></span>

<span data-ttu-id="ba36b-140">SDK 包含 `Azure.Core.Diagnostics.AzureEventSourceListener` 类（在 Azure.Core NuGet 包中定义），该类包含两个静态方法，可用于简化 .NET 应用程序的全面日志记录：分别是 `CreateConsoleLogger` 方法和 `CreateTraceLogger` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba36b-140">The SDK includes the `Azure.Core.Diagnostics.AzureEventSourceListener` class (defined in the Azure.Core NuGet package), which contains two static methods that simplify comprehensive logging for your .NET application: `CreateConsoleLogger` and `CreateTraceLogger`.</span></span> <span data-ttu-id="ba36b-141">这些方法采用一个可选参数来指定日志级别。</span><span class="sxs-lookup"><span data-stu-id="ba36b-141">These methods take an optional parameter that specifies a log level.</span></span>

### <a name="log-to-the-console-window"></a><span data-ttu-id="ba36b-142">记录到控制台窗口</span><span class="sxs-lookup"><span data-stu-id="ba36b-142">Log to the console window</span></span>

<span data-ttu-id="ba36b-143">Azure SDK for .NET 客户端库的核心原则是简化实时查看综合日志的功能。</span><span class="sxs-lookup"><span data-stu-id="ba36b-143">A core tenet of the Azure SDK for .NET client libraries is to simplify the ability to view comprehensive logs in real time.</span></span> <span data-ttu-id="ba36b-144">利用 `CreateConsoleLogger` 方法，可以使用一行代码将日志发送到控制台窗口：</span><span class="sxs-lookup"><span data-stu-id="ba36b-144">The `CreateConsoleLogger` method allows you to send logs to the console window with a single line of code:</span></span>

```csharp
using AzureEventSourceListener listener = AzureEventSourceListener.CreateConsoleLogger();
```

### <a name="log-to-diagnostic-traces"></a><span data-ttu-id="ba36b-145">记录到诊断跟踪</span><span class="sxs-lookup"><span data-stu-id="ba36b-145">Log to diagnostic traces</span></span>

<span data-ttu-id="ba36b-146">如果实现跟踪侦听器，则可以使用 `CreateTraceLogger` 方法记录到标准 .NET 事件跟踪机制 ([`System.Diagnostics.Tracing`](/dotnet/api/system.diagnostics.tracing))。</span><span class="sxs-lookup"><span data-stu-id="ba36b-146">If you implement trace listeners, you can use the `CreateTraceLogger` method to log to the standard .NET event tracing mechanism ([`System.Diagnostics.Tracing`](/dotnet/api/system.diagnostics.tracing)).</span></span> <span data-ttu-id="ba36b-147">有关 .NET 中的事件跟踪的详细信息，请参阅[跟踪侦听器](../framework/debug-trace-profile/trace-listeners.md)。</span><span class="sxs-lookup"><span data-stu-id="ba36b-147">For more information on event tracing in .NET, see [Trace Listeners](../framework/debug-trace-profile/trace-listeners.md).</span></span> <span data-ttu-id="ba36b-148">下面的示例指定一个详细的日志级别：</span><span class="sxs-lookup"><span data-stu-id="ba36b-148">This example specifies a log level of verbose:</span></span>

```csharp
using AzureEventSourceListener listener = AzureEventSourceListener.CreateTraceLogger(EventLevel.Verbose);
```

## <a name="configure-custom-logging"></a><span data-ttu-id="ba36b-149">配置自定义日志记录</span><span class="sxs-lookup"><span data-stu-id="ba36b-149">Configure custom logging</span></span>

<span data-ttu-id="ba36b-150">如上所述，需要注册事件侦听器才能从 Azure SDK for .NET 接收日志消息。</span><span class="sxs-lookup"><span data-stu-id="ba36b-150">As mentioned above, you need to register event listeners to receive log messages from the Azure SDK for .NET.</span></span> <span data-ttu-id="ba36b-151">如果你不想使用上面的一种简化方法来实现全面的日志记录，可以构造 `AzureEventSourceListener` 类的实例，并向其传递你编写的回调函数。</span><span class="sxs-lookup"><span data-stu-id="ba36b-151">If you don’t want to implement comprehensive logging using one the simplified methods above, you can construct an instance of the `AzureEventSourceListener` class and pass it a callback function that you write.</span></span> <span data-ttu-id="ba36b-152">此方法将接收可以处理并且必须处理的日志消息。</span><span class="sxs-lookup"><span data-stu-id="ba36b-152">This method will receive log messages that you can process however you need to.</span></span> <span data-ttu-id="ba36b-153">另外，在构造实例时，可以指定要包括的日志级别。</span><span class="sxs-lookup"><span data-stu-id="ba36b-153">In addition, when you construct the instance, you can specify the log levels to include.</span></span>

<span data-ttu-id="ba36b-154">下面的示例创建一个事件侦听器，它使用自定义消息记录到控制台，并被筛选为详细级别的 Azure 核心事件。</span><span class="sxs-lookup"><span data-stu-id="ba36b-154">The following example creates an event listener that logs to the console with a custom message, and is filtered to Azure core events of the level verbose.</span></span>

```csharp
using AzureEventSourceListener listener = new AzureEventSourceListener((e, message) =>
    {
        // Only log messages from Azure-Core event source
        if (e.EventSource.Name == "Azure-Core")
        {
            Console.WriteLine($"{DateTime.Now} {message}");
        }
    },
    level: EventLevel.Verbose);
```

## <a name="next-steps"></a><span data-ttu-id="ba36b-155">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ba36b-155">Next steps</span></span>

- [<span data-ttu-id="ba36b-156">在 Azure 应用服务中为应用启用诊断日志记录</span><span class="sxs-lookup"><span data-stu-id="ba36b-156">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/troubleshoot-diagnostic-logs)
- <span data-ttu-id="ba36b-157">了解 [Azure 安全日志记录和审核](/azure/security/fundamentals/log-audit)选项</span><span class="sxs-lookup"><span data-stu-id="ba36b-157">Review [Azure security logging and auditing](/azure/security/fundamentals/log-audit) options</span></span>
- <span data-ttu-id="ba36b-158">了解如何使用 [Azure 平台日志](/azure/azure-monitor/platform/platform-logs-overview)</span><span class="sxs-lookup"><span data-stu-id="ba36b-158">Learn how to work with [Azure platform logs](/azure/azure-monitor/platform/platform-logs-overview)</span></span>
- <span data-ttu-id="ba36b-159">详细了解 [.NET Core 日志记录和跟踪](../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="ba36b-159">Read more about [.NET Core logging and tracing](../core/diagnostics/logging-tracing.md)</span></span>
