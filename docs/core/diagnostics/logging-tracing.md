---
title: 日志记录和跟踪 - .NET Core
description: .NET Core 日志记录和跟踪简介。
ms.date: 10/12/2020
ms.openlocfilehash: dfecdad4518c79b605eb4fbe991af07fcba7db1c
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111083"
---
# <a name="net-core-logging-and-tracing"></a>.NET Core 日志记录和跟踪

日志记录和跟踪实际上是同一技术的两个名称。 这种简单的技术从计算机早期就开始使用了。 它只涉及检测应用程序以写入稍后要使用的输出。

## <a name="reasons-to-use-logging-and-tracing"></a>使用日志记录和跟踪的原因

这一简单技术功能非常强大。 可以在调试器失败的情况下使用它：

- 很难使用传统调试器来调试在很长一段时间内发生的问题。 借助日志，可以跨在较长时间进行详细的事后检查。 与此相反，调试器限制为只能进行实时分析。
- 多线程应用程序和分布式应用程序通常难以调试。  附加调试器往往会修改行为。 可以根据需要分析详细日志，以了解复杂的系统。
- 分布式应用程序中的问题可能源于许多组件之间的复杂交互，将调试器连接到系统的每个部分可能是不合理的。
- 许多服务不应停止。 附加调试器往往会导致超时失败。
- 问题并非总是可预见的。 日志记录和跟踪旨在降低开销，以便在出现问题的情况下可以始终记录程序。

## <a name="net-core-apis"></a>.NET Core API

### <a name="print-style-apis"></a>打印样式 API

每个 <xref:System.Console?displayProperty=nameWithType>、<xref:System.Diagnostics.Trace?displayProperty=nameWithType> 和 <xref:System.Diagnostics.Debug?displayProperty=nameWithType> 类都提供类似的打印样式 API，以便于日志记录。

选择使用哪种打印样式 API 由用户自己决定。 主要区别包括：

- <xref:System.Console?displayProperty=nameWithType>
  - 始终启用，并始终写入控制台。
  - 这对于客户可能需要在版本中查看的信息非常有用。
  - 由于这是最简单的方法，所以常常用于临时调试。 此调试代码通常不会签入到源代码管理中。
- <xref:System.Diagnostics.Trace?displayProperty=nameWithType>
  - 仅在通过向源中添加 `#define TRACE` 或在编译时指定选项 `/d:TRACE` 来定义 `TRACE` 时启用。
  - 写入到附加 <xref:System.Diagnostics.Trace.Listeners>，默认情况下为 <xref:System.Diagnostics.DefaultTraceListener>。
  - 创建将在大多数生成中启用的日志时使用此 API。
- <xref:System.Diagnostics.Debug?displayProperty=nameWithType>
  - 仅在通过向源中添加 `#define DEBUG` 或在编译时指定选项 `/d:DEBUG` 来定义 `DEBUG` 时启用。
  - 写入附加调试器。
  - 在 `*nix` 写入到 stderr 时（如果设置了 `COMPlus_DebugWriteToStdErr`）。
  - 创建将仅在调试生成中启用的日志时使用此 API。

### <a name="logging-events"></a>记录事件

以下 API 更面向于事件。 它们记录事件对象，而不是记录简单字符串。

- <xref:System.Diagnostics.Tracing.EventSource?displayProperty=nameWithType>
  - EventSource 是主根 .NET Core 跟踪 API。
  - 在所有 .NET Standard 版本中提供。
  - 仅允许跟踪可序列化的对象。
  - 可以通过配置为使用 EventSource 的任何 [EventListener](xref:System.Diagnostics.Tracing.EventListener) 实例在进程中使用。
  - 可通过以下方式在进程外使用：
    - 所有平台上的 [.NET Core EventPipe](./eventpipe.md)
    - [Windows 事件跟踪 (ETW)](/windows/win32/etw/event-tracing-portal)
    - [适用于 Linux 的 LTTng 跟踪框架](https://lttng.org/)
      - 演练：[使用 PerfCollect 收集 LTTng 跟踪](trace-perfcollect-lttng.md)。

- <xref:System.Diagnostics.DiagnosticSource?displayProperty=nameWithType>
  - 包含在 .NET Core 中，用作 .NET Framework 的 [NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource)。
  - 允许对非序列化对象进行进程内跟踪。
  - 包括一个桥，以允许将已记录对象的所选字段写入 <xref:System.Diagnostics.Tracing.EventSource>。

- <xref:System.Diagnostics.EventLog?displayProperty=nameWithType>
  - 仅限 Windows。
  - 将消息写入 Windows 事件日志。
  - 系统管理员希望严重的应用程序错误消息会在 Windows 事件日志中出现。

## <a name="distributed-tracing"></a>分布式跟踪

[分布式跟踪](./distributed-tracing.md)是一种诊断技术，可帮助工程师找出应用程序中的故障和性能问题，尤其是那些可能跨多个计算机或进程分布的问题。 此技术通过应用程序跟踪请求，将不同应用程序组件完成的工作关联在一起，并将其与应用程序可能为并发请求所做的其他工作分开。

## <a name="ilogger-and-logging-frameworks"></a>ILogger 和日志记录框架

低级别 API 可能不符合你的日志记录需求。 可能需要考虑使用日志记录框架。

<xref:Microsoft.Extensions.Logging.ILogger> 接口已用于创建一个公共日志记录接口，可在其中通过依赖项注入插入记录器。

例如，为了使你能够为应用程序做出最佳选择，.NET 提供了对选择的内置和第三方框架的支持：

- [.NET 内置日志记录提供程序](../extensions/logging-providers.md#built-in-logging-providers)
- [.NET 第三方日志记录提供程序](../extensions/logging-providers.md#third-party-logging-providers)

## <a name="logging-related-references"></a>与日志记录相关的引用

- [如何：使用跟踪和调试进行条件编译](../../framework/debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md)

- [如何：向应用程序代码添加跟踪语句](../../framework/debug-trace-profile/how-to-add-trace-statements-to-application-code.md)

- [.NET 中的日志记录](../extensions/logging.md)提供它所支持的日志记录技术的概述。

- [C# 字符串内插](../../csharp/language-reference/tokens/interpolated.md)可以简化日志记录代码的编写。

- [运行时提供程序事件列表](../../fundamentals/diagnostics/runtime-events.md)

- [.NET 中的已知事件提供程序](well-known-event-providers.md)

- <xref:System.Exception.Message?displayProperty=nameWithType> 属性对日志记录异常很有用。

- <xref:System.Diagnostics.StackTrace?displayProperty=nameWithType> 类可用于在日志中提供堆栈信息。

## <a name="performance-considerations"></a>性能注意事项

字符串格式设置可能会占用大量的 CPU 处理时间。

在性能关键应用程序中，建议执行以下操作：

- 如果未侦听，则避免进行大量日志记录。 通过检查是否首先启用了日志记录，避免构造开销较大的日志记录消息。
- 仅记录有用的内容。
- 将复杂的格式设置推迟到分析阶段。
