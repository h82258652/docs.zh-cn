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
# <a name="logging-with-the-azure-sdk-for-net"></a>通过 Azure SDK for .NET 启用日志记录

[Azure SDK](https://azure.microsoft.com/downloads/) for .NET 客户端库包括记录客户端库操作的功能。 这使你可以监视客户端库对 Azure 服务进行的 I/O 请求和响应。 通常，日志用于调试或诊断通信问题。 本文介绍了三种通过 Azure SDK for .NET 启用日志记录的方法：

- 记录到控制台窗口
- 记录到 .NET 诊断跟踪
- 配置自定义日志记录

> [!IMPORTANT]
> 本文适用于使用最新版 Azure SDK for .NET 的客户端库。 若要查看库是否受支持，请参阅 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/index.html)列表。 如果应用程序使用旧版 Azure SDK 客户端库，请参阅适用服务文档中的具体说明。

## <a name="log-information"></a>记录信息

SDK 记录以下信息，清理参数查询和标头值以删除个人数据。

HTTP 请求日志条目：

- 唯一 ID
- HTTP 方法
- URI
- 传出请求标头

HTTP 响应日志条目：

- I/O 操作的持续时间（已用时间）
- 请求 ID
- HTTP 状态代码
- HTTP 原因短语
- 响应头
- 错误信息（如果适用）

对于请求和响应内容：

- 内容流是文本形式还是字节形式，取决于 Content-Type 标头。
     > [!NOTE]
     > 内容日志记录默认处于禁用状态。 要启用它，请将 `ClientOptions` 中的 `Diagnostics.IsLoggingContentEnabled` 设置为 `true`。

通常以以下三个级别之一输出事件日志：

- 有关请求和响应事件的信息
- 错误警告
- 详细消息和内容记录的详细信息

## <a name="enable-logging-with-built-in-methods"></a>使用内置方法启用日志记录

Azure SDK for .NET 客户端库通过 [`EventSource` 类](/dotnet/api/system.diagnostics.tracing.eventsource)将事件记录到 Windows 事件跟踪 (ETW)，这是 .NET 的典型功能。 事件源使你能够以最小的性能开销在应用程序代码中使用结构化日志记录。 要访问这些事件日志，需要注册事件侦听器。

SDK 包含 `Azure.Core.Diagnostics.AzureEventSourceListener` 类（在 Azure.Core NuGet 包中定义），该类包含两个静态方法，可用于简化 .NET 应用程序的全面日志记录：分别是 `CreateConsoleLogger` 方法和 `CreateTraceLogger` 方法。 这些方法采用一个可选参数来指定日志级别。

### <a name="log-to-the-console-window"></a>记录到控制台窗口

Azure SDK for .NET 客户端库的核心原则是简化实时查看综合日志的功能。 利用 `CreateConsoleLogger` 方法，可以使用一行代码将日志发送到控制台窗口：

```csharp
using AzureEventSourceListener listener = AzureEventSourceListener.CreateConsoleLogger();
```

### <a name="log-to-diagnostic-traces"></a>记录到诊断跟踪

如果实现跟踪侦听器，则可以使用 `CreateTraceLogger` 方法记录到标准 .NET 事件跟踪机制 ([`System.Diagnostics.Tracing`](/dotnet/api/system.diagnostics.tracing))。 有关 .NET 中的事件跟踪的详细信息，请参阅[跟踪侦听器](../framework/debug-trace-profile/trace-listeners.md)。 下面的示例指定一个详细的日志级别：

```csharp
using AzureEventSourceListener listener = AzureEventSourceListener.CreateTraceLogger(EventLevel.Verbose);
```

## <a name="configure-custom-logging"></a>配置自定义日志记录

如上所述，需要注册事件侦听器才能从 Azure SDK for .NET 接收日志消息。 如果你不想使用上面的一种简化方法来实现全面的日志记录，可以构造 `AzureEventSourceListener` 类的实例，并向其传递你编写的回调函数。 此方法将接收可以处理并且必须处理的日志消息。 另外，在构造实例时，可以指定要包括的日志级别。

下面的示例创建一个事件侦听器，它使用自定义消息记录到控制台，并被筛选为详细级别的 Azure 核心事件。

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

## <a name="next-steps"></a>后续步骤

- [在 Azure 应用服务中为应用启用诊断日志记录](/azure/app-service/troubleshoot-diagnostic-logs)
- 了解 [Azure 安全日志记录和审核](/azure/security/fundamentals/log-audit)选项
- 了解如何使用 [Azure 平台日志](/azure/azure-monitor/platform/platform-logs-overview)
- 详细了解 [.NET Core 日志记录和跟踪](../core/diagnostics/logging-tracing.md)
