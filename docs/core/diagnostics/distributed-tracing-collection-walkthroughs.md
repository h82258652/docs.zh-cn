---
title: 收集分布式跟踪 - .NET
description: 有关使用 OpenTelemetry、Application Insights 或 ActivityListener 在 .NET 应用程序中收集分布式跟踪的教程
ms.topic: tutorial
ms.date: 03/14/2021
ms.openlocfilehash: de10145dcf731e2fe6293461f127463c9dee3cf5
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231175"
---
# <a name="collect-a-distributed-trace"></a>收集分布式跟踪

**本文适用范围：✔️** .NET Core 2.1 及更高版本和 .NET Framework 4.5 及更高版本

检测代码可以创建 <xref:System.Diagnostics.Activity> 对象作为分布式跟踪的一部分，但需要将这些对象中的信息收集到集中存储中，以便稍后可以查看整个跟踪。 本教程将以不同的方式收集分布式跟踪遥测，以便在需要时可用于诊断应用程序问题。 如果需要添加新的检测，请参阅[检测教程](distributed-tracing-instrumentation-walkthroughs.md)。

## <a name="collect-traces-using-opentelemetry"></a>使用 OpenTelemetry 收集跟踪

### <a name="prerequisites"></a>先决条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本

### <a name="create-an-example-application"></a>创建一个示例应用程序

需要先创建应用程序，然后才能收集任何分布式跟踪遥测。 通常，此检测可能位于库中，但为简单起见，将使用 <xref:System.Diagnostics.ActivitySource.StartActivity%2A> 创建一个小型应用并在其中包含一些示例检测。 此时，尚未进行任何收集，StartActivity() 没有副作用，并且返回 null。 有关详细信息，请参阅[检测教程](distributed-tracing-instrumentation-walkthroughs.md)。

```dotnetcli
dotnet new console
```

面向 .NET 5 及更高版本的应用程序已包含必要的分布式跟踪 API。 对于面向早期 .NET 版本的应用，请添加 [System.Diagnostics.DiagnosticSource NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/)版本 5 或更高版本。

```dotnetcli
dotnet add package System.Diagnostics.DiagnosticSource
```

将生成的 Program.cs 内容替换为此示例源：

```C#
using System;
using System.Diagnostics;
using System.Threading.Tasks;

namespace Sample.DistributedTracing
{
    class Program
    {
        static ActivitySource s_source = new ActivitySource("Sample.DistributedTracing");

        static async Task Main(string[] args)
        {
            await DoSomeWork();
            Console.WriteLine("Example work done");
        }

        static async Task DoSomeWork()
        {
            using (Activity a = s_source.StartActivity("SomeWork"))
            {
                await StepOne();
                await StepTwo();
            }
        }

        static async Task StepOne()
        {
            using (Activity a = s_source.StartActivity("StepOne"))
            {
                await Task.Delay(500);
            }
        }

        static async Task StepTwo()
        {
            using (Activity a = s_source.StartActivity("StepTwo"))
            {
                await Task.Delay(1000);
            }
        }
    }
}
```

运行应用时暂时不会收集任何跟踪数据：

```dotnetcli
> dotnet run
Example work done
```

### <a name="collect-using-opentelemetry"></a>使用 OpenTelemetry 收集

[OpenTelemetry](https://opentelemetry.io/) 是由[云本机计算基础](https://www.cncf.io/)支持的与供应商无关的开放源代码项目，旨在标准化为云原生软件生成和收集遥测数据的过程。 此示例将收集和显示控制台上的分布式跟踪信息，尽管可以重新配置 OpenTelemetry 以将其发送到其他位置。 有关详细信息，请参阅 [OpenTelemetry 入门指南](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md)。

添加 [OpenTelemetry.Exporter.Console](https://www.nuget.org/packages/OpenTelemetry.Exporter.Console/) NuGet 包。

```dotnetcli
dotnet add package OpenTelemetry.Exporter.Console
```

使用以下语句通过其他 OpenTelemetry 更新 Program.cs：

```C#
using OpenTelemetry;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;
using System;
using System.Diagnostics;
using System.Threading.Tasks;
```

更新 Main() 以创建 OpenTelemetry TracerProvider：

```C#
        public static async Task Main()
        {
            using var tracerProvider = Sdk.CreateTracerProviderBuilder()
                .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MySample"))
                .AddSource("Sample.DistributedTracing")
                .AddConsoleExporter()
                .Build();

            await DoSomeWork();
            Console.WriteLine("Example work done");
        }
```

现在，应用将收集分布式跟踪信息，并将其显示到控制台：

```dotnetcli
> dotnet run
Activity.Id:          00-7759221f2c5599489d455b84fa0f90f4-6081a9b8041cd840-01
Activity.ParentId:    00-7759221f2c5599489d455b84fa0f90f4-9a52f72c08a9d447-01
Activity.DisplayName: StepOne
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:46:46.8649754Z
Activity.Duration:    00:00:00.5069226
Resource associated with Activity:
    service.name: MySample
    service.instance.id: 909a4624-3b2e-40e4-a86b-4a2c8003219e

Activity.Id:          00-7759221f2c5599489d455b84fa0f90f4-d2b283db91cf774c-01
Activity.ParentId:    00-7759221f2c5599489d455b84fa0f90f4-9a52f72c08a9d447-01
Activity.DisplayName: StepTwo
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:46:47.3838737Z
Activity.Duration:    00:00:01.0142278
Resource associated with Activity:
    service.name: MySample
    service.instance.id: 909a4624-3b2e-40e4-a86b-4a2c8003219e

Activity.Id:          00-7759221f2c5599489d455b84fa0f90f4-9a52f72c08a9d447-01
Activity.DisplayName: SomeWork
Activity.Kind:        Internal
Activity.StartTime:   2021-03-18T10:46:46.8634510Z
Activity.Duration:    00:00:01.5402045
Resource associated with Activity:
    service.name: MySample
    service.instance.id: 909a4624-3b2e-40e4-a86b-4a2c8003219e

Example work done
```

#### <a name="sources"></a>源

此示例代码调用了 `AddSource("Sample.DistributedTracing")`，这样 OpenTelemetry 就可以捕获代码中已经存在的 ActivitySource 所产生的活动：

```csharp
        static ActivitySource s_source = new ActivitySource("Sample.DistributedTracing");
```

可以通过使用源名称调用 AddSource() 来捕获任何 ActivitySource 中的遥测数据。

#### <a name="exporters"></a>导出工具

控制台导出工具对简单的示例或本地开发非常有用，但在生产部署中，可能需要将跟踪发送到集中存储。 OpenTelemetry 支持使用不同[导出工具](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/glossary.md#exporter-library)的各种目标。
有关如何配置 OpenTelemetry 的详细信息，请参阅 [OpenTelemetry 入门指南](https://github.com/open-telemetry/opentelemetry-dotnet#getting-started)。

## <a name="collect-traces-using-application-insights"></a>使用 Application Insights 收集跟踪

配置 Application Insights SDK（[ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)、[ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)）或启用[无代码检测](https://docs.microsoft.com/azure/azure-monitor/app/codeless-overview)后，会自动捕获分布式跟踪遥测。

有关详细信息，请参阅 [Application Insights 分布式跟踪文档](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)。

> [!NOTE]
> 目前 Application Insights 仅支持收集特定的已知活动检测，并将忽略新用户添加的活动。 Application Insights 提供 [TrackDependency](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics#trackdependency) 作为供应商特定的 API，用于添加自定义分布式跟踪信息。

## <a name="collect-traces-using-custom-logic"></a>使用自定义逻辑收集跟踪

开发人员可随意为活动跟踪数据创建自定义收集逻辑。 此示例使用 .NET 提供的 <xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> API 收集遥测数据，并将其输出到控制台。

### <a name="prerequisites"></a>先决条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本

### <a name="create-an-example-application"></a>创建一个示例应用程序

首先将创建一个示例应用程序，并在其中包含一些分布式跟踪检测，但未收集任何跟踪数据。

```dotnetcli
dotnet new console
```

面向 .NET 5 及更高版本的应用程序已包含必要的分布式跟踪 API。 对于面向早期 .NET 版本的应用，请添加 [System.Diagnostics.DiagnosticSource NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/)版本 5 或更高版本。

```dotnetcli
dotnet add package System.Diagnostics.DiagnosticSource
```

将生成的 Program.cs 内容替换为此示例源：

```C#
using System;
using System.Diagnostics;
using System.Threading.Tasks;

namespace Sample.DistributedTracing
{
    class Program
    {
        static ActivitySource s_source = new ActivitySource("Sample.DistributedTracing");

        static async Task Main(string[] args)
        {
            await DoSomeWork();
            Console.WriteLine("Example work done");
        }

        static async Task DoSomeWork()
        {
            using (Activity a = s_source.StartActivity("SomeWork"))
            {
                await StepOne();
                await StepTwo();
            }
        }

        static async Task StepOne()
        {
            using (Activity a = s_source.StartActivity("StepOne"))
            {
                await Task.Delay(500);
            }
        }

        static async Task StepTwo()
        {
            using (Activity a = s_source.StartActivity("StepTwo"))
            {
                await Task.Delay(1000);
            }
        }
    }
}
```

运行应用时暂时不会收集任何跟踪数据：

```dotnetcli
> dotnet run
Example work done
```

### <a name="add-code-to-collect-the-traces"></a>添加代码以收集跟踪数据

使用下面的代码更新 Main()：

```C#
        static async Task Main(string[] args)
        {
            Activity.DefaultIdFormat = ActivityIdFormat.W3C;
            Activity.ForceDefaultIdFormat = true;

            Console.WriteLine("         {0,-15} {1,-60} {2,-15}", "OperationName", "Id", "Duration");
            ActivitySource.AddActivityListener(new ActivityListener()
            {
                ShouldListenTo = (source) => true,
                Sample = (ref ActivityCreationOptions<ActivityContext> options) => ActivitySamplingResult.AllDataAndRecorded,
                ActivityStarted = activity => Console.WriteLine("Started: {0,-15} {1,-60}", activity.OperationName, activity.Id),
                ActivityStopped = activity => Console.WriteLine("Stopped: {0,-15} {1,-60} {2,-15}", activity.OperationName, activity.Id, activity.Duration)
            });

            await DoSomeWork();
            Console.WriteLine("Example work done");
        }
```

现在，输出包括日志记录：

```dotnetcli
> dotnet run
         OperationName   Id                                                           Duration
Started: SomeWork        00-bdb5faffc2fc1548b6ba49a31c4a0ae0-c447fb302059784f-01
Started: StepOne         00-bdb5faffc2fc1548b6ba49a31c4a0ae0-a7c77a4e9a02dc4a-01
Stopped: StepOne         00-bdb5faffc2fc1548b6ba49a31c4a0ae0-a7c77a4e9a02dc4a-01      00:00:00.5093849
Started: StepTwo         00-bdb5faffc2fc1548b6ba49a31c4a0ae0-9210ad536cae9e4e-01
Stopped: StepTwo         00-bdb5faffc2fc1548b6ba49a31c4a0ae0-9210ad536cae9e4e-01      00:00:01.0111847
Stopped: SomeWork        00-bdb5faffc2fc1548b6ba49a31c4a0ae0-c447fb302059784f-01      00:00:01.5236391
Example work done
```

虽然设置 <xref:System.Diagnostics.Activity.DefaultIdFormat> 和 <xref:System.Diagnostics.Activity.ForceDefaultIdFormat> 是可选操作，但这样做有助于确保示例在不同的 .NET 运行时版本上生成类似的输出。 .NET 5 默认使用 W3C TraceContext ID 格式，但早期的 .NET 版本默认使用 <xref:System.Diagnostics.ActivityIdFormat.Hierarchical> ID 格式。 有关更多详细信息，请参阅[活动 ID](distributed-tracing-concepts.md#activity-ids)。

<xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> 用于在活动的生存期内接收回调。

- <xref:System.Diagnostics.ActivityListener.ShouldListenTo> - 每个活动都与 ActivitySource 关联，后者可充当活动的命名空间和生成方。
对于进程中的每个 ActivitySource，都会调用一次此回调。 如果你有兴趣执行采样或收到有关此源产生的活动的启动/停止事件的通知，则返回 true。
- <xref:System.Diagnostics.ActivityListener.Sample> - 默认情况下，<xref:System.Diagnostics.ActivitySource.StartActivity%2A> 不会创建 Activity 对象，除非某个 ActivityListener 指示应对其进行采用。 返回 <xref:System.Diagnostics.ActivitySamplingResult.AllDataAndRecorded>
表示应创建活动，<xref:System.Diagnostics.Activity.IsAllDataRequested> 应设置为 true，并且 <xref:System.Diagnostics.Activity.ActivityTraceFlags> 将设置 <xref:System.Diagnostics.ActivityTraceFlags.Recorded> 标志。 IsAllDataRequested 可被检测代码观测到，这提示侦听器需要确保填充辅助活动信息（如标记和事件）。
记录的标志在 W3C TraceContext ID 中进行编码，暗示分布式跟踪中涉及的其他进程应对此跟踪进行采样。
- <xref:System.Diagnostics.ActivityListener.ActivityStarted> 和 <xref:System.Diagnostics.ActivityListener.ActivityStopped> 分别在启动和停止活动时调用。 可以通过这些回调记录有关活动的相关信息或对其进行修改。
当活动刚开始时，许多数据可能仍然不完整，并将在活动停止之前进行填充。

创建 ActivityListener 并填充回调之后，即可通过调用 <xref:System.Diagnostics.ActivitySource.AddActivityListener(System.Diagnostics.ActivityListener)?displayProperty=nameWithType>
启动调用回调操作。 调用 <xref:System.Diagnostics.ActivityListener.Dispose?displayProperty=nameWithType> 可停止回调流。 请注意，在多线程代码中，当 Dispose() 运行时，甚至在它返回后不久，都可能会收到正在进行的回调通知。
