---
title: 添加分布式跟踪检测 - .NET
description: 在 .NET 应用程序中检测分布式跟踪的教程
ms.topic: tutorial
ms.date: 03/14/2021
ms.openlocfilehash: eeb065d1ac39dd34d9b27e2ad63195818d0d6493
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111501"
---
# <a name="adding-distributed-tracing-instrumentation"></a>添加分布式跟踪检测

本文适用范围：✔️ .NET Core 2.1 及更高版本和 .NET Framework 4.5 及更高版本

可以使用 <xref:System.Diagnostics.Activity?displayProperty=nameWithType> API 检测 .NET 应用程序，以生成分布式跟踪遥测。 一些检测内置于标准 .NET 库中，但你可能会想要添加更多检测来让代码更易于诊断。 在本教程中，你将添加新的自定义分布式跟踪检测。 请参阅[集合教程](distributed-tracing-instrumentation-walkthroughs.md)，详细了解如何记录此检测生成的遥测。

## <a name="prerequisites"></a>先决条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本

## <a name="an-initial-app"></a>初始应用

首先，将创建使用 OpenTelemetry 收集遥测，但尚未安装任何检测的示例应用。

```dotnetcli
dotnet new console
```

面向 .NET 5 及更高版本的应用程序已包含必要的分布式跟踪 API。 对于面向早期 .NET 版本的应用，请添加 [System.Diagnostics.DiagnosticSource NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/)版本 5 或更高版本。

```dotnetcli
dotnet add package System.Diagnostics.DiagnosticSource
```

添加将用于收集遥测的 [OpenTelemetry](https://www.nuget.org/packages/OpenTelemetry/) 和 [OpenTelemetry.Exporter.Console](https://www.nuget.org/packages/OpenTelemetry.Exporter.Console/) NuGet 包。

```dotnetcli
dotnet add package OpenTelemetry
dotnet add package OpenTelemetry.Exporter.Console
```

将生成的 Program.cs 内容替换为此示例源：

```C#
using OpenTelemetry;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;
using System;
using System.Threading.Tasks;

namespace Sample.DistributedTracing
{
    class Program
    {
        static async Task Main(string[] args)
        {
            using var tracerProvider = Sdk.CreateTracerProviderBuilder()
                .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MySample"))
                .AddSource("Sample.DistributedTracing")
                .AddConsoleExporter()
                .Build();

            await DoSomeWork("banana", 8);
            Console.WriteLine("Example work done");
        }

        // All the functions below simulate doing some arbitrary work
        static async Task DoSomeWork(string foo, int bar)
        {
            await StepOne();
            await StepTwo();
        }

        static async Task StepOne()
        {
            await Task.Delay(500);
        }

        static async Task StepTwo()
        {
            await Task.Delay(1000);
        }
    }
}
```

该应用尚无检测，因此没有要显示的跟踪信息：

```dotnetcli
> dotnet run
Example work done
```

#### <a name="best-practices"></a>最佳实践

仅应用开发人员需要引用可选的第三方库用于收集分布式跟踪遥测，例如本示例中的 OpenTelemetry。 .NET 库创建者可以仅依赖于 System.Diagnostics.DiagnosticSource（.NET 运行时的一部分）中的 API。 这样可确保库在各种 .NET 应用中运行，而无论应用开发人员在用于收集遥测的库或供应商方面有何偏好。

## <a name="adding-basic-instrumentation"></a>添加基本检测

应用程序和库使用 <xref:System.Diagnostics.ActivitySource?displayProperty=nameWithType> 和 <xref:System.Diagnostics.Activity?displayProperty=nameWithType> 类添加分布式跟踪检测。

### <a name="activitysource"></a>ActivitySource

首先创建 ActivitySource 实例。 ActivitySource 提供用于创建和启动 Activity 对象的 API。 将 Main() 和 `using System.Diagnostics;` 上方的静态 ActivitySource 变量添加到 using 语句。

```csharp
using OpenTelemetry;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;
using System;
using System.Diagnostics;
using System.Threading.Tasks;

namespace Sample.DistributedTracing
{
    class Program
    {
        private static ActivitySource source = new ActivitySource("Sample.DistributedTracing", "1.0.0");

        static async Task Main(string[] args)
        {
            ...    
```

#### <a name="best-practices"></a>最佳实践

- 创建一次 ActivitySource，将它存储在静态变量中，并根据需要使用相应实例。
每个库或库子组件都可以（并且通常应该）创建自己的源。 如果预期应用开发人员想要能够独立启用和禁用源中的 Activity 遥测，请考虑创建新源，而不是重复使用现有源。

- 传递给构造函数的源名称必须是唯一的，以免与其他任何源发生冲突。
建议使用包含程序集名称的层次结构名称，如果同一程序集内有多个源，则还可以使用包含组件名称的层次结构名称。 例如，`Microsoft.AspNetCore.Hosting`。 如果程序集在第二个独立程序集中添加代码检测，则名称应基于定义 ActivitySource 的程序集，而不是要检测其代码的程序集。

- version 是可选参数。 建议在发布库的多个版本时提供 version 并更改已检测的遥测。

> [!NOTE]
> OpenTelemetry 使用替代术语“Tracer”和“Span”。 在 .NET 中，“ActivitySource”是 Tracer 的实现，而 Activity 则是“Span”的实现。 .NET 的 Activity 类型远早于 OpenTelemetry 规范，并且已保留原始 .NET 命名，以确保 .NET 生态系统内的一致性和 .NET 应用程序兼容性。

### <a name="activity-creation"></a>创建 Activity 对象

使用 ActivitySource 对象，根据有意义的工作单元启动和停止 Activity 对象。 使用下面显示的代码更新 DoSomeWork()：

```csharp
        static async Task DoSomeWork(string foo, int bar)
        {
            using (Activity activity = source.StartActivity("SomeWork"))
            {
                await StepOne();
                await StepTwo();
            }
        }
```

现在运行应用会显示正在记录的新 Activity：

```dotnetcli
> dotnet run
Activity.Id:          00-f443e487a4998c41a6fd6fe88bae644e-5b7253de08ed474f-01
Activity.DisplayName: SomeWork
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:36:51.4720202Z
Activity.Duration:    00:00:01.5025842
Resource associated with Activity:
    service.name: MySample
    service.instance.id: 067f4bb5-a5a8-4898-a288-dec569d6dbef
```

#### <a name="notes"></a>说明

- <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 同时创建和启动 Activity。 列出的代码模式使用的是 `using` 块，它会在执行此块后自动释放创建的 Activity 对象。 释放 Activity 对象将停止它，因此代码无需显式调用 <xref:System.Diagnostics.Activity.Stop?displayProperty=nameWithType>。
这简化了编码模式。

- <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 在内部确定是否有任何侦听器记录 Activity。 如果没有已注册的侦听器，或有不关注此类事件的侦听器，那么 StartActivity() 会返回 `null`，并避免创建 Activity 对象。 这是一项性能优化，以便代码模式仍可用于频繁调用的函数。

## <a name="optional-populate-tags"></a>可选：填充标记

Activity 支持名为标记的键值数据，后者通常用于存储可能对诊断有用的工作的所有参数。 更新 DoSomeWork() 以包括它们：

```csharp
        static async Task DoSomeWork(string foo, int bar)
        {
            using (Activity activity = source.StartActivity("SomeWork"))
            {
                activity?.SetTag("foo", foo);
                activity?.SetTag("bar", bar);
                await StepOne();
                await StepTwo();
            }
        }
```

```dotnetcli
> dotnet run
Activity.Id:          00-2b56072db8cb5a4496a4bfb69f46aa06-7bc4acda3b9cce4d-01
Activity.DisplayName: SomeWork
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:37:31.4949570Z
Activity.Duration:    00:00:01.5417719
Activity.TagObjects:
    foo: banana
    bar: 8
Resource associated with Activity:
    service.name: MySample
    service.instance.id: 25bbc1c3-2de5-48d9-9333-062377fea49c

Example work done
```

#### <a name="best-practices"></a>最佳实践

- 如上所述，<xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 返回的 `activity` 可能为 null。 C# 中的 null 合并运算符 ?. 是一个方便的快捷方式，可在 Activity 不为 null 时仅调用 <xref:System.Diagnostics.Activity.SetTag%2A?displayProperty=nameWithType>。 该行为等同于写入：

```csharp
if(activity != null)
{
    activity.SetTag("foo", foo);
}
```

- OpenTelemetry 提供一组建议的[约定](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/trace/semantic_conventions)，用于在 Activity 上设置代表常见应用程序工作类型的标记。

- 如果要检测具有高性能要求的函数，则 <xref:System.Diagnostics.Activity.IsAllDataRequested?displayProperty=nameWithType> 是一个提示，可指示侦听 Activity 的任何代码是否打算读取辅助信息，例如标记。 如果没有侦听器要进行读取，则检测的代码无需耗费 CPU 周期来填充它。 为简单起见，此示例未应用该优化。

## <a name="optional-adding-events"></a>可选：添加事件

事件是带有时间戳的消息，可以将任意附加诊断数据流附加到 Activity。 向 Activity 添加一些事件：

```csharp
        static async Task DoSomeWork(string foo, int bar)
        {
            using (Activity activity = source.StartActivity("SomeWork"))
            {
                activity?.SetTag("foo", foo);
                activity?.SetTag("bar", bar);
                await StepOne();
                activity?.AddEvent(new ActivityEvent("Part way there"));
                await StepTwo();
                activity?.AddEvent(new ActivityEvent("Done now"));
            }
        }
```

```dotnetcli
> dotnet run
Activity.Id:          00-82cf6ea92661b84d9fd881731741d04e-33fff2835a03c041-01
Activity.DisplayName: SomeWork
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:39:10.6902609Z
Activity.Duration:    00:00:01.5147582
Activity.TagObjects:
    foo: banana
    bar: 8
Activity.Events:
    Part way there [3/18/2021 10:39:11 AM +00:00]
    Done now [3/18/2021 10:39:12 AM +00:00]
Resource associated with Activity:
    service.name: MySample
    service.instance.id: ea7f0fcb-3673-48e0-b6ce-e4af5a86ce4f

Example work done
```

#### <a name="best-practices"></a>最佳实践

- 事件存储在内存中列表中，直到可以传输，这使得此机制仅适合记录适量的事件。 对于大量或不限量的事件，使用专注于此任务的日志记录 API（例如 [ILogger](https://docs.microsoft.com/aspnet/core/fundamentals/logging/)）是更好的选择。 ILogger 还可确保无论应用开发人员是否选择使用分布式跟踪，日志记录信息都将可用。
ILogger 支持自动捕获活动 Activity ID，因此仍可以将通过该 API 记录的消息与分布式跟踪关联。

## <a name="optional-adding-status"></a>可选：添加状态

OpenTelemetry 允许每个 Activity 报告代表工作的通过/失败结果的[状态](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#set-status)。 .NET 目前没有用于此目的的强类型 API，但存在有关使用标记的既有约定：

- `otel.status_code` 是用于存储 `StatusCode` 的标记名称。 StatusCode 标记的值必须是字符串“UNSET”、“OK”或“ERROR”之一，其分别对应于 StatusCode 枚举 `Unset`、`Ok` 和 `Error`。
- `otel.status_description` 是用于存储可选 `Description` 的标记名称

更新 DoSomeWork() 以设置状态：

```csharp
        static async Task DoSomeWork(string foo, int bar)
        {
            using (Activity activity = source.StartActivity("SomeWork"))
            {
                activity?.SetTag("foo", foo);
                activity?.SetTag("bar", bar);
                await StepOne();
                activity?.AddEvent(new ActivityEvent("Part way there"));
                await StepTwo();
                activity?.AddEvent(new ActivityEvent("Done now"));
                
                // Pretend something went wrong
                activity?.SetTag("otel.status_code", "ERROR");
                activity?.SetTag("otel.status_description", "Use this text give more information about the error");
            }
        }
```

## <a name="optional-add-additional-activities"></a>可选：添加其他 Activity

可以嵌套 Activity 用于描述较大工作单元的各个部分。 这对于可能不会快速执行的代码部分或更好地找到来自特定外部依赖项的故障而言尤其有价值。 尽管此示例在每种方法中都使用 Activity，但这仅仅是因为已最大限度地减少了额外的代码。 在更大、更真实的项目中，在每种方法中都使用 Activity 会产生极其详细的跟踪，因此不建议使用。

更新 StepOne 和 StepTwo，以围绕这些单独的步骤添加更多跟踪：

```csharp
        static async Task StepOne()
        {
            using (Activity activity = source.StartActivity("StepOne"))
            {
                await Task.Delay(500);
            }
        }

        static async Task StepTwo()
        {
            using (Activity activity = source.StartActivity("StepTwo"))
            {
                await Task.Delay(1000);
            }
        }
```

```dotnetcli
> dotnet run
Activity.Id:          00-9d5aa439e0df7e49b4abff8d2d5329a9-39cac574e8fda44b-01
Activity.ParentId:    00-9d5aa439e0df7e49b4abff8d2d5329a9-f16529d0b7c49e44-01
Activity.DisplayName: StepOne
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:40:51.4278822Z
Activity.Duration:    00:00:00.5051364
Resource associated with Activity:
    service.name: MySample
    service.instance.id: e0a8c12c-249d-4bdd-8180-8931b9b6e8d0

Activity.Id:          00-9d5aa439e0df7e49b4abff8d2d5329a9-4ccccb6efdc59546-01
Activity.ParentId:    00-9d5aa439e0df7e49b4abff8d2d5329a9-f16529d0b7c49e44-01
Activity.DisplayName: StepTwo
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:40:51.9441095Z
Activity.Duration:    00:00:01.0052729
Resource associated with Activity:
    service.name: MySample
    service.instance.id: e0a8c12c-249d-4bdd-8180-8931b9b6e8d0

Activity.Id:          00-9d5aa439e0df7e49b4abff8d2d5329a9-f16529d0b7c49e44-01
Activity.DisplayName: SomeWork
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:40:51.4256627Z
Activity.Duration:    00:00:01.5286408
Activity.TagObjects:
    foo: banana
    bar: 8
    otel.status_code: ERROR
    otel.status_description: Use this text give more information about the error
Activity.Events:
    Part way there [3/18/2021 10:40:51 AM +00:00]
    Done now [3/18/2021 10:40:52 AM +00:00]
Resource associated with Activity:
    service.name: MySample
    service.instance.id: e0a8c12c-249d-4bdd-8180-8931b9b6e8d0

Example work done
```

请注意，StepOne 和 StepTwo 都包含引用 SomeWork 的 ParentId。 控制台不能很好地呈现嵌套工作树，但许多 GUI 查看器（例如 [Zipkin](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/src/OpenTelemetry.Exporter.Zipkin/README.md)）都可以将其显示为甘特图：

[![Zipkin 甘特图](media/zipkin-nested-activities.jpg)](media/zipkin-nested-activities.jpg)

### <a name="optional-activitykind"></a>可选：ActivityKind

Activity 包含描述 Activity、其父项和子项之间关系的 <xref:System.Diagnostics.Activity.Kind%2A?displayProperty=nameWithType> 属性。 默认情况下，所有新 Activity 都设置为 <xref:System.Diagnostics.ActivityKind.Internal>，这适用于属于应用程序中的内部操作且没有远程父项或子项的 Activity。 可以使用 <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 上的 kind 参数设置其他类型。 有关其他选项，请参阅 <xref:System.Diagnostics.ActivityKind?displayProperty=nameWithType>。

### <a name="optional-links"></a>可选：链接

当工作在批处理系统中发生时，单个 Activity 可能表示同时代表许多不同请求的工作，且其中每个请求都有自己的跟踪 ID。尽管 Activity 被限制为具有单个父项，但它可以使用 <xref:System.Diagnostics.ActivityLink?displayProperty=nameWithType> 链接到其他跟踪 ID。 每个 ActivityLink 都填充有 <xref:System.Diagnostics.ActivityContext>，后者存储有关要链接到的 Activity 的 ID 信息。 可以使用 <xref:System.Diagnostics.Activity.Context%2A?displayProperty=nameWithType> 从进程内 Activity 对象中检索 ActivityContext，也可以使用 <xref:System.Diagnostics.ActivityContext.Parse(System.String,System.String)?displayProperty=nameWithType> 从序列化 ID 信息中分析它。

```csharp
void DoBatchWork(ActivityContext[] requestContexts)
{
    // Assume each context in requestContexts encodes the trace-id that was sent with a request
    using(Activity activity = s_source.StartActivity(name: "BigBatchOfWork",
                                                     kind: ActivityKind.Internal,
                                                     parentContext: null,
                                                     links: requestContexts.Select(ctx => new ActivityLink(ctx))
    {
        // do the batch of work here
    }
}
```

与可以按需添加的事件和标记不同，链接必须在 StartActivity() 期间添加且此后不可变。
