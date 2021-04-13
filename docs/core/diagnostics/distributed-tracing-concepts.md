---
title: 分布式跟踪概念 - .NET
description: .NET 分布式跟踪概念
ms.date: 03/14/2021
ms.openlocfilehash: 368cb545b9928534766e3005992a7a55571b8dcc
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111500"
---
# <a name="net-distributed-tracing-concepts"></a>.NET 分布式跟踪概念

分布式跟踪是一种诊断技术，可帮助工程师找出应用程序中的故障和性能问题，尤其是那些可能跨多个计算机或进程分布的问题。 如需深入了解分布式跟踪的使用场景以及入门示例代码，请参阅[分布式跟踪概述](distributed-tracing.md)。

### <a name="traces-and-activities"></a>跟踪和活动

每次应用程序收到新请求时，它都可以与跟踪相关联。 在用 .NET 编写的应用程序组件中，跟踪中的工作单元由 <xref:System.Diagnostics.Activity?displayProperty=nameWithType> 实例表示，并且跟踪整体上构成了这些活动的树，可能跨越了许多不同的进程。 为新请求创建的第一个活动形成跟踪树的根，并跟踪处理请求的总体持续时间和成功/失败。 可以选择创建子活动，以将工作细分为可单独跟踪的不同步骤。
例如，假设某个活动跟踪了 Web 服务器中的特定入站 HTTP 请求，则可以创建子活动来跟踪完成请求所需的每个数据库查询。 这样做可以单独记录每个查询的持续时间和成功情况。
活动可以记录每个工作单元的其他信息，例如 <xref:System.Diagnostics.Activity.OperationName>、称为 <xref:System.Diagnostics.Activity.Tags> 的名称-值对和 <xref:System.Diagnostics.Activity.Events>。 名称标识所执行的工作类型；标记可以记录工作的描述性参数；事件是一种简单的日志记录机制，用于记录带时间戳的诊断消息。

> [!NOTE]
> 分布式跟踪中工作单元的另一个常见的行业名称为“Span”。
> .NET 在很多年前就采用了“Activity(活动)”这个术语，当时“Span”这个名称的概念还不为人们所了解。

### <a name="activity-ids"></a>活动 ID

使用唯一 ID 建立分布式跟踪树中活动之间的父-子关系。 .NET 分布式跟踪的实现支持两种 ID 方案，即 W3C 标准 [TraceContext](https://www.w3.org/TR/trace-context/)（这是 .NET 5 中的默认设置）和一个较早的名为“分层”的 .NET 约定（可用于后向兼容性）。
<xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType> 控制使用的 ID 方案。 在 W3C TraceContext 标准中，每个跟踪都分配有一个全局唯一的 16 字节跟踪 ID (<xref:System.Diagnostics.Activity.TraceId?displayProperty=nameWithType>)，且该跟踪内的每个活动都分配有唯一的 8 字节 Span ID (<xref:System.Diagnostics.Activity.SpanId?displayProperty=nameWithType>)。 每个活动都记录跟踪 ID、其自身的 Span ID 以及其父 (<xref:System.Diagnostics.Activity.ParentSpanId?displayProperty=nameWithType>) 的 Span ID。 由于分布式跟踪可以跟踪跨进程边界的工作，因此父活动和子活动可能不在同一进程中。 跟踪 ID 和父 Span ID 的组合可以在全局范围内仅标识父活动，无论该活动驻留在哪个进程中。

<xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType> 控制使用哪种 ID 格式来启动新的跟踪，但默认情况下，如果将新活动添加到现有跟踪，则会使用任何父活动所使用的格式。
将 <xref:System.Diagnostics.Activity.ForceDefaultIdFormat?displayProperty=nameWithType> 设置为 true 会覆盖此行为，并使用 DefaultIdFormat 创建所有新的活动（即使父级使用其他 ID 格式也是如此）。

### <a name="starting-and-stopping-activities"></a>启动和停止活动

进程中的每个线程都有一个对应的 Activity 对象，该对象跟踪该线程上发生的工作，可通过 <xref:System.Diagnostics.Activity.Current?displayProperty=nameWithType> 进行访问。 当前活动会自动流过线程上的所有同步调用，以及在不同线程上处理的异步调用。 如果活动 A 是线程上的当前活动，并且代码启动新活动 B，则 B 将成为该线程上的新的当前活动。 默认情况下，活动 B 还会将活动 A 视为其父项。 之后，当活动 B 停止时，活动 A 将还原为该线程上的当前活动。 活动启动时，它会将当前时间捕获为 <xref:System.Diagnostics.Activity.StartTimeUtc?displayProperty=nameWithType>。 停止时，会将 <xref:System.Diagnostics.Activity.Duration?displayProperty=nameWithType> 计算为当前时间与开始时间之间的差值。

### <a name="coordinating-across-process-boundaries"></a>跨进程边界进行协调

为了跟踪跨进程边界的工作，需要在网络中传输活动的父 ID，以便接收进程可以创建引用它们的活动。 使用 W3C TraceContext ID 格式时，.NET 还将使用[标准](https://www.w3.org/TR/trace-context/)推荐的 HTTP 标头来传输此信息。 使用 <xref:System.Diagnostics.ActivityIdFormat.Hierarchical> ID 格式时，.NET 会使用自定义 request-id HTTP 标头来传输该 ID。 与许多其他语言运行时不同，.NET 内置库（如 ASP.NET Web 服务器和 System.Net.Http）本机理解如何对 HTTP 消息上的活动 ID 进行解码和编码。 运行时还理解如何通过同步和异步调用流式传输 ID。 这意味着，接收和发出 HTTP 消息的 .NET 应用程序会自动参与到流动的分布式跟踪 ID 中，应用开发人员无需进行特殊编码，也无需依赖第三方库。 第三方库可以添加对通过非 HTTP 消息协议传输 ID 的支持，或者支持 HTTP 的自定义编码约定。

### <a name="collecting-traces"></a>收集跟踪

检测代码可以创建 <xref:System.Diagnostics.Activity> 对象作为分布式跟踪的一部分，但需将这些对象中的信息传输到集中持久性存储中并在其中进行序列化，以便以后可以对整个跟踪进行有用的查看。 有几个遥测收集库可以执行此任务，例如 [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)、[OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md) 或第三方遥测或 APM 供应商提供的库。 或者，开发人员可以使用 <xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> 或 <xref:System.Diagnostics.DiagnosticListener?displayProperty=nameWithType> 创作自己的自定义活动遥测收集逻辑。 ActivityListener 支持观察任何活动，无论开发人员是否对此有先验知识。
因此，ActivityListener 是一种简单而灵活的常规用途解决方案。 与此相反，使用 DiagnosticListener 是一个更复杂的方案，该方案需要检测代码通过调用 <xref:System.Diagnostics.DiagnosticSource.StartActivity%2A?displayProperty=nameWithType> 来选择加入，而且集合库需要知道检测代码在启动时所使用的确切命名信息。 使用 DiagnosticSource 和 DiagnosticListener，创建者和侦听器可以交换任意 .NET 对象并建立自定义的信息传递约定。

### <a name="sampling"></a>采样

为了改进高吞吐量应用程序的性能，.NET 上的分布式跟踪支持仅采样跟踪的一部分，而不是记录所有跟踪。 对于使用推荐的 <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> API 创建的活动，遥测收集库可以使用 <xref:System.Diagnostics.ActivityListener.Sample%2A?displayProperty=nameWithType> 回调来控制采样。
日志记录库可以选择完全不创建活动，使用传播分布式跟踪 ID 所需的最小信息创建活动，或者用完整的诊断信息填充活动。 这些选择进行了权衡，增加了性能开销以提高诊断效用。 使用较旧的调用方式 <xref:System.Diagnostics.Activity.%23ctor%2A?displayProperty=nameWithType> 和 <xref:System.Diagnostics.DiagnosticSource.StartActivity%2A?displayProperty=nameWithType> 启动的活动，可以通过首先调用 <xref:System.Diagnostics.DiagnosticSource.IsEnabled%2A?displayProperty=nameWithType> 来支持 DiagnosticListener 采样。
即使在捕获完整的诊断信息时，.NET 实现也可以快速与高效的收集器耦合，同时，活动可以在新式硬件上以大约一微秒的时间创建、填充和传输。 采样可以将每个未记录的活动的检测成本降低到小于 100 毫微秒。

## <a name="next-steps"></a>后续步骤

有关在 .NET 应用程序开始使用分布式跟踪的示例代码，请参见[分布式跟踪概述](distributed-tracing.md)。
