---
title: 在 .NET 中实现自定义日志记录提供程序
description: 了解如何在 .NET 应用程序中实现自定义日志记录提供程序。
author: IEvangelist
ms.author: dapine
ms.date: 04/07/2021
ms.topic: how-to
ms.openlocfilehash: 56dd3aa9962d2cdaf13df85960a99aab7b050477
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255124"
---
# <a name="implement-a-custom-logging-provider-in-net"></a>在 .NET 中实现自定义日志记录提供程序

有很多[日志记录提供程序](logging-providers.md)可用于常见日志记录需求。 如果某个可用的提供程序不满足你的应用程序需要，则你可能需要实现自定义的 <xref:Microsoft.Extensions.Logging.ILoggerProvider>。 在本文中，你将学习如何实现可用于在控制台中为日志着色的自定义日志记录提供程序。

### <a name="sample-custom-logger-configuration"></a>示例自定义记录器配置

此示例会使用以下配置类型为每个日志级别和事件 ID 创建不同的颜色控制台条目：

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLoggerConfiguration.cs":::

前面的代码将默认级别设置为 `Information`，将颜色设置为 `Green`，而且 `EventId` 隐式设置为 `0`。

### <a name="create-the-custom-logger"></a>创建自定义记录器

`ILogger` 实现类别名称通常是日志记录源。 例如，创建记录器的类型：

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLogger.cs":::

前面的代码：

- 为每个类别名称创建一个记录器实例。
- 在 `IsEnabled` 中检查 `_config.LogLevels.ContainsKey(logLevel)`，因此每个 `logLevel` 都有一个唯一的记录器。 还应为所有更高的日志级别启用记录器：

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLogger.cs" range="16-17":::

## <a name="custom-logger-provider"></a>自定义记录器提供程序

`ILoggerProvider` 对象负责创建记录器实例。 也许不需要为每个类别创建一个记录器实例，但这对于某些记录器（例如 NLog 或 log4net）来说是需要的。 这样，你还可以按需为每个类别选择不同的日志记录输出目标：

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLoggerProvider.cs":::

在前面的代码中，<xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger%2A> 会为每个类别名称创建一个 `ColorConsoleLogger` 实例并将其存储在 [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2) 中。 此外，还需要 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 接口才能更新对基础 `ColorConsoleLoggerConfiguration` 对象的更改。

## <a name="usage-and-registration-of-the-custom-logger"></a>自定义记录器的使用和注册

若要添加自定义日志记录提供程序和相应的记录器，请从 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging(Microsoft.Extensions.Hosting.IHostBuilder,System.Action{Microsoft.Extensions.Logging.ILoggingBuilder})?displayProperty=nameWithType> 使用 <xref:Microsoft.Extensions.Logging.ILoggingBuilder> 添加 <xref:Microsoft.Extensions.Logging.ILoggerProvider>：

:::code language="csharp" source="snippets/configuration/console-custom-logging/Program.cs" range="23-33":::

`ILoggingBuilder` 创建一个或多个 `ILogger` 实例。 框架使用 `ILogger` 实例记录信息。

按照约定，`ILoggingBuilder` 上的扩展方法用于注册自定义提供程序：

:::code language="csharp" source="snippets/configuration/console-custom-logging/Extensions/ColorConsoleLoggerExtensions.cs":::

运行此简单应用程序将把颜色输出呈现到控制台窗口，如下图所示：

:::image type="content" source="media/color-console-logger.png" alt-text="为控制台日志器示例输出着色":::

## <a name="see-also"></a>另请参阅

- [.NET 中的日志记录](logging.md)
- [.NET 中的日志记录提供程序](logging-providers.md)
- [.NET 中的依赖关系注入](dependency-injection.md)
- [.NET 中的高性能日志记录](high-performance-logging.md)
