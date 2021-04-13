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
# <a name="collect-a-distributed-trace"></a><span data-ttu-id="459ff-103">收集分布式跟踪</span><span class="sxs-lookup"><span data-stu-id="459ff-103">Collect a distributed trace</span></span>

<span data-ttu-id="459ff-104">**本文适用范围：✔️** .NET Core 2.1 及更高版本和 .NET Framework 4.5 及更高版本</span><span class="sxs-lookup"><span data-stu-id="459ff-104">**This article applies to: ✔️** .NET Core 2.1 and later versions **and** .NET Framework 4.5 and later versions</span></span>

<span data-ttu-id="459ff-105">检测代码可以创建 <xref:System.Diagnostics.Activity> 对象作为分布式跟踪的一部分，但需要将这些对象中的信息收集到集中存储中，以便稍后可以查看整个跟踪。</span><span class="sxs-lookup"><span data-stu-id="459ff-105">Instrumented code can create <xref:System.Diagnostics.Activity> objects as part of a distributed trace, but the information in these objects needs to be collected into centralized storage so that the entire trace can be reviewed later.</span></span> <span data-ttu-id="459ff-106">本教程将以不同的方式收集分布式跟踪遥测，以便在需要时可用于诊断应用程序问题。</span><span class="sxs-lookup"><span data-stu-id="459ff-106">In this tutorial you will collect the distributed trace telemetry in different ways so that it is available to diagnose application issues when needed.</span></span> <span data-ttu-id="459ff-107">如果需要添加新的检测，请参阅[检测教程](distributed-tracing-instrumentation-walkthroughs.md)。</span><span class="sxs-lookup"><span data-stu-id="459ff-107">See [the instrumentation tutorial](distributed-tracing-instrumentation-walkthroughs.md) if you need to add new instrumentation.</span></span>

## <a name="collect-traces-using-opentelemetry"></a><span data-ttu-id="459ff-108">使用 OpenTelemetry 收集跟踪</span><span class="sxs-lookup"><span data-stu-id="459ff-108">Collect traces using OpenTelemetry</span></span>

### <a name="prerequisites"></a><span data-ttu-id="459ff-109">先决条件</span><span class="sxs-lookup"><span data-stu-id="459ff-109">Prerequisites</span></span>

- <span data-ttu-id="459ff-110">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="459ff-110">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) or a later version</span></span>

### <a name="create-an-example-application"></a><span data-ttu-id="459ff-111">创建一个示例应用程序</span><span class="sxs-lookup"><span data-stu-id="459ff-111">Create an example application</span></span>

<span data-ttu-id="459ff-112">需要先创建应用程序，然后才能收集任何分布式跟踪遥测。</span><span class="sxs-lookup"><span data-stu-id="459ff-112">Before any distributed trace telemetry can be collected we need to produce it.</span></span> <span data-ttu-id="459ff-113">通常，此检测可能位于库中，但为简单起见，将使用 <xref:System.Diagnostics.ActivitySource.StartActivity%2A> 创建一个小型应用并在其中包含一些示例检测。</span><span class="sxs-lookup"><span data-stu-id="459ff-113">Often this instrumentation might be in libraries but for simplicity you will create a small app that has some example instrumentation using <xref:System.Diagnostics.ActivitySource.StartActivity%2A>.</span></span> <span data-ttu-id="459ff-114">此时，尚未进行任何收集，StartActivity() 没有副作用，并且返回 null。</span><span class="sxs-lookup"><span data-stu-id="459ff-114">At this point no collection is happening yet, StartActivity() has no side-effect and returns null.</span></span> <span data-ttu-id="459ff-115">有关详细信息，请参阅[检测教程](distributed-tracing-instrumentation-walkthroughs.md)。</span><span class="sxs-lookup"><span data-stu-id="459ff-115">See [the instrumentation tutorial](distributed-tracing-instrumentation-walkthroughs.md) for more details.</span></span>

```dotnetcli
dotnet new console
```

<span data-ttu-id="459ff-116">面向 .NET 5 及更高版本的应用程序已包含必要的分布式跟踪 API。</span><span class="sxs-lookup"><span data-stu-id="459ff-116">Applications that target .NET 5 and later already have the necessary distributed tracing APIs included.</span></span> <span data-ttu-id="459ff-117">对于面向早期 .NET 版本的应用，请添加 [System.Diagnostics.DiagnosticSource NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/)版本 5 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="459ff-117">For apps targeting older .NET versions add the [System.Diagnostics.DiagnosticSource NuGet package](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/) version 5 or greater.</span></span>

```dotnetcli
dotnet add package System.Diagnostics.DiagnosticSource
```

<span data-ttu-id="459ff-118">将生成的 Program.cs 内容替换为此示例源：</span><span class="sxs-lookup"><span data-stu-id="459ff-118">Replace the contents of the generated Program.cs with this example source:</span></span>

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

<span data-ttu-id="459ff-119">运行应用时暂时不会收集任何跟踪数据：</span><span class="sxs-lookup"><span data-stu-id="459ff-119">Running the app does not collect any trace data yet:</span></span>

```dotnetcli
> dotnet run
Example work done
```

### <a name="collect-using-opentelemetry"></a><span data-ttu-id="459ff-120">使用 OpenTelemetry 收集</span><span class="sxs-lookup"><span data-stu-id="459ff-120">Collect using OpenTelemetry</span></span>

<span data-ttu-id="459ff-121">[OpenTelemetry](https://opentelemetry.io/) 是由[云本机计算基础](https://www.cncf.io/)支持的与供应商无关的开放源代码项目，旨在标准化为云原生软件生成和收集遥测数据的过程。</span><span class="sxs-lookup"><span data-stu-id="459ff-121">[OpenTelemetry](https://opentelemetry.io/) is a vendor neutral open source project supported by the [Cloud Native Computing Foundation](https://www.cncf.io/) that aims to standardize generating and collecting telemetry for cloud-native software.</span></span> <span data-ttu-id="459ff-122">此示例将收集和显示控制台上的分布式跟踪信息，尽管可以重新配置 OpenTelemetry 以将其发送到其他位置。</span><span class="sxs-lookup"><span data-stu-id="459ff-122">In this example you will collect and display distributed trace information on the console though OpenTelemetry can be reconfigured to send it elsewhere.</span></span> <span data-ttu-id="459ff-123">有关详细信息，请参阅 [OpenTelemetry 入门指南](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md)。</span><span class="sxs-lookup"><span data-stu-id="459ff-123">See the [OpenTelemetry getting started guide](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md) for more information.</span></span>

<span data-ttu-id="459ff-124">添加 [OpenTelemetry.Exporter.Console](https://www.nuget.org/packages/OpenTelemetry.Exporter.Console/) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="459ff-124">Add the [OpenTelemetry.Exporter.Console](https://www.nuget.org/packages/OpenTelemetry.Exporter.Console/) NuGet package.</span></span>

```dotnetcli
dotnet add package OpenTelemetry.Exporter.Console
```

<span data-ttu-id="459ff-125">使用以下语句通过其他 OpenTelemetry 更新 Program.cs：</span><span class="sxs-lookup"><span data-stu-id="459ff-125">Update Program.cs with additional OpenTelemetry using statments:</span></span>

```C#
using OpenTelemetry;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;
using System;
using System.Diagnostics;
using System.Threading.Tasks;
```

<span data-ttu-id="459ff-126">更新 Main() 以创建 OpenTelemetry TracerProvider：</span><span class="sxs-lookup"><span data-stu-id="459ff-126">Update Main() to create the OpenTelemetry TracerProvider:</span></span>

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

<span data-ttu-id="459ff-127">现在，应用将收集分布式跟踪信息，并将其显示到控制台：</span><span class="sxs-lookup"><span data-stu-id="459ff-127">Now the app collects distributed trace information and displays it to the console:</span></span>

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

#### <a name="sources"></a><span data-ttu-id="459ff-128">源</span><span class="sxs-lookup"><span data-stu-id="459ff-128">Sources</span></span>

<span data-ttu-id="459ff-129">此示例代码调用了 `AddSource("Sample.DistributedTracing")`，这样 OpenTelemetry 就可以捕获代码中已经存在的 ActivitySource 所产生的活动：</span><span class="sxs-lookup"><span data-stu-id="459ff-129">In the example code you invoked `AddSource("Sample.DistributedTracing")` so that OpenTelemetry would capture the Activities produced by the ActivitySource that was already present in the code:</span></span>

```csharp
        static ActivitySource s_source = new ActivitySource("Sample.DistributedTracing");
```

<span data-ttu-id="459ff-130">可以通过使用源名称调用 AddSource() 来捕获任何 ActivitySource 中的遥测数据。</span><span class="sxs-lookup"><span data-stu-id="459ff-130">Telemetry from any ActivitySource can captured by calling AddSource() with the source's name.</span></span>

#### <a name="exporters"></a><span data-ttu-id="459ff-131">导出工具</span><span class="sxs-lookup"><span data-stu-id="459ff-131">Exporters</span></span>

<span data-ttu-id="459ff-132">控制台导出工具对简单的示例或本地开发非常有用，但在生产部署中，可能需要将跟踪发送到集中存储。</span><span class="sxs-lookup"><span data-stu-id="459ff-132">The console exporter is helpful for quick examples or local development but in a production deployment you will probably want to send traces to a centralized store.</span></span> <span data-ttu-id="459ff-133">OpenTelemetry 支持使用不同[导出工具](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/glossary.md#exporter-library)的各种目标。</span><span class="sxs-lookup"><span data-stu-id="459ff-133">OpenTelemetry supports a variety of destinations using different [exporters](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/glossary.md#exporter-library).</span></span>
<span data-ttu-id="459ff-134">有关如何配置 OpenTelemetry 的详细信息，请参阅 [OpenTelemetry 入门指南](https://github.com/open-telemetry/opentelemetry-dotnet#getting-started)。</span><span class="sxs-lookup"><span data-stu-id="459ff-134">See the [OpenTelemetry getting started guide](https://github.com/open-telemetry/opentelemetry-dotnet#getting-started) for more information on configuring OpenTelemetry.</span></span>

## <a name="collect-traces-using-application-insights"></a><span data-ttu-id="459ff-135">使用 Application Insights 收集跟踪</span><span class="sxs-lookup"><span data-stu-id="459ff-135">Collect traces using Application Insights</span></span>

<span data-ttu-id="459ff-136">配置 Application Insights SDK（[ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)、[ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)）或启用[无代码检测](https://docs.microsoft.com/azure/azure-monitor/app/codeless-overview)后，会自动捕获分布式跟踪遥测。</span><span class="sxs-lookup"><span data-stu-id="459ff-136">Distributed tracing telemetry is automatically captured after configuring the Application Insights SDK ([ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net), [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)) or by enabling [code-less instrumentation](https://docs.microsoft.com/azure/azure-monitor/app/codeless-overview).</span></span>

<span data-ttu-id="459ff-137">有关详细信息，请参阅 [Application Insights 分布式跟踪文档](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)。</span><span class="sxs-lookup"><span data-stu-id="459ff-137">See the [Application Insights distributed tracing documentation](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="459ff-138">目前 Application Insights 仅支持收集特定的已知活动检测，并将忽略新用户添加的活动。</span><span class="sxs-lookup"><span data-stu-id="459ff-138">Currently Application Insights only supports collecting specific well-known Activity instrumentation and will ignore new user added Activities.</span></span> <span data-ttu-id="459ff-139">Application Insights 提供 [TrackDependency](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics#trackdependency) 作为供应商特定的 API，用于添加自定义分布式跟踪信息。</span><span class="sxs-lookup"><span data-stu-id="459ff-139">Application Insights offers [TrackDependency](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics#trackdependency) as a vendor specific API for adding custom distributed tracing information.</span></span>

## <a name="collect-traces-using-custom-logic"></a><span data-ttu-id="459ff-140">使用自定义逻辑收集跟踪</span><span class="sxs-lookup"><span data-stu-id="459ff-140">Collect traces using custom logic</span></span>

<span data-ttu-id="459ff-141">开发人员可随意为活动跟踪数据创建自定义收集逻辑。</span><span class="sxs-lookup"><span data-stu-id="459ff-141">Developers are free to create their own customized collection logic for Activity trace data.</span></span> <span data-ttu-id="459ff-142">此示例使用 .NET 提供的 <xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> API 收集遥测数据，并将其输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="459ff-142">This example collects the telemetry using the <xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> API provided by .NET and prints it to the console.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="459ff-143">先决条件</span><span class="sxs-lookup"><span data-stu-id="459ff-143">Prerequisites</span></span>

- <span data-ttu-id="459ff-144">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="459ff-144">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) or a later version</span></span>

### <a name="create-an-example-application"></a><span data-ttu-id="459ff-145">创建一个示例应用程序</span><span class="sxs-lookup"><span data-stu-id="459ff-145">Create an example application</span></span>

<span data-ttu-id="459ff-146">首先将创建一个示例应用程序，并在其中包含一些分布式跟踪检测，但未收集任何跟踪数据。</span><span class="sxs-lookup"><span data-stu-id="459ff-146">First you will create an example application that has some distributed trace instrumentation but no trace data is being collected.</span></span>

```dotnetcli
dotnet new console
```

<span data-ttu-id="459ff-147">面向 .NET 5 及更高版本的应用程序已包含必要的分布式跟踪 API。</span><span class="sxs-lookup"><span data-stu-id="459ff-147">Applications that target .NET 5 and later already have the necessary distributed tracing APIs included.</span></span> <span data-ttu-id="459ff-148">对于面向早期 .NET 版本的应用，请添加 [System.Diagnostics.DiagnosticSource NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/)版本 5 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="459ff-148">For apps targeting older .NET versions add the [System.Diagnostics.DiagnosticSource NuGet package](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/) version 5 or greater.</span></span>

```dotnetcli
dotnet add package System.Diagnostics.DiagnosticSource
```

<span data-ttu-id="459ff-149">将生成的 Program.cs 内容替换为此示例源：</span><span class="sxs-lookup"><span data-stu-id="459ff-149">Replace the contents of the generated Program.cs with this example source:</span></span>

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

<span data-ttu-id="459ff-150">运行应用时暂时不会收集任何跟踪数据：</span><span class="sxs-lookup"><span data-stu-id="459ff-150">Running the app does not collect any trace data yet:</span></span>

```dotnetcli
> dotnet run
Example work done
```

### <a name="add-code-to-collect-the-traces"></a><span data-ttu-id="459ff-151">添加代码以收集跟踪数据</span><span class="sxs-lookup"><span data-stu-id="459ff-151">Add code to collect the traces</span></span>

<span data-ttu-id="459ff-152">使用下面的代码更新 Main()：</span><span class="sxs-lookup"><span data-stu-id="459ff-152">Update Main() with this code:</span></span>

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

<span data-ttu-id="459ff-153">现在，输出包括日志记录：</span><span class="sxs-lookup"><span data-stu-id="459ff-153">The output now includes logging:</span></span>

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

<span data-ttu-id="459ff-154">虽然设置 <xref:System.Diagnostics.Activity.DefaultIdFormat> 和 <xref:System.Diagnostics.Activity.ForceDefaultIdFormat> 是可选操作，但这样做有助于确保示例在不同的 .NET 运行时版本上生成类似的输出。</span><span class="sxs-lookup"><span data-stu-id="459ff-154">Setting <xref:System.Diagnostics.Activity.DefaultIdFormat> and <xref:System.Diagnostics.Activity.ForceDefaultIdFormat> is optional but helps ensure the sample produces similar output on different .NET runtime versions.</span></span> <span data-ttu-id="459ff-155">.NET 5 默认使用 W3C TraceContext ID 格式，但早期的 .NET 版本默认使用 <xref:System.Diagnostics.ActivityIdFormat.Hierarchical> ID 格式。</span><span class="sxs-lookup"><span data-stu-id="459ff-155">.NET 5 uses the W3C TraceContext ID format by default but earlier .NET versions default to using <xref:System.Diagnostics.ActivityIdFormat.Hierarchical> ID format.</span></span> <span data-ttu-id="459ff-156">有关更多详细信息，请参阅[活动 ID](distributed-tracing-concepts.md#activity-ids)。</span><span class="sxs-lookup"><span data-stu-id="459ff-156">See [Activity IDs](distributed-tracing-concepts.md#activity-ids) for more details.</span></span>

<span data-ttu-id="459ff-157"><xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> 用于在活动的生存期内接收回调。</span><span class="sxs-lookup"><span data-stu-id="459ff-157"><xref:System.Diagnostics.ActivityListener?displayProperty=nameWithType> is used to receive callbacks during the lifetime of an Activity.</span></span>

- <span data-ttu-id="459ff-158"><xref:System.Diagnostics.ActivityListener.ShouldListenTo> - 每个活动都与 ActivitySource 关联，后者可充当活动的命名空间和生成方。</span><span class="sxs-lookup"><span data-stu-id="459ff-158"><xref:System.Diagnostics.ActivityListener.ShouldListenTo> - Each Activity is associated with an ActivitySource which acts as its namespace and producer.</span></span>
<span data-ttu-id="459ff-159">对于进程中的每个 ActivitySource，都会调用一次此回调。</span><span class="sxs-lookup"><span data-stu-id="459ff-159">This callback is invoked once for each ActivitySource in the process.</span></span> <span data-ttu-id="459ff-160">如果你有兴趣执行采样或收到有关此源产生的活动的启动/停止事件的通知，则返回 true。</span><span class="sxs-lookup"><span data-stu-id="459ff-160">Return true if you are interested in performing sampling or being notified about start/stop events for Activities produced by this source.</span></span>
- <span data-ttu-id="459ff-161"><xref:System.Diagnostics.ActivityListener.Sample> - 默认情况下，<xref:System.Diagnostics.ActivitySource.StartActivity%2A> 不会创建 Activity 对象，除非某个 ActivityListener 指示应对其进行采用。</span><span class="sxs-lookup"><span data-stu-id="459ff-161"><xref:System.Diagnostics.ActivityListener.Sample> - By default <xref:System.Diagnostics.ActivitySource.StartActivity%2A> does not create an Activity object unless some ActivityListener indicates it should be sampled.</span></span> <span data-ttu-id="459ff-162">返回 <xref:System.Diagnostics.ActivitySamplingResult.AllDataAndRecorded></span><span class="sxs-lookup"><span data-stu-id="459ff-162">Returning <xref:System.Diagnostics.ActivitySamplingResult.AllDataAndRecorded></span></span>
<span data-ttu-id="459ff-163">表示应创建活动，<xref:System.Diagnostics.Activity.IsAllDataRequested> 应设置为 true，并且 <xref:System.Diagnostics.Activity.ActivityTraceFlags> 将设置 <xref:System.Diagnostics.ActivityTraceFlags.Recorded> 标志。</span><span class="sxs-lookup"><span data-stu-id="459ff-163">indicates that the Activity should be created, <xref:System.Diagnostics.Activity.IsAllDataRequested> should be set to true, and <xref:System.Diagnostics.Activity.ActivityTraceFlags> will have the <xref:System.Diagnostics.ActivityTraceFlags.Recorded> flag set.</span></span> <span data-ttu-id="459ff-164">IsAllDataRequested 可被检测代码观测到，这提示侦听器需要确保填充辅助活动信息（如标记和事件）。</span><span class="sxs-lookup"><span data-stu-id="459ff-164">IsAllDataRequested can be observed by the instrumented code as a hint that a listener wants to ensure that auxilliary Activity information such as Tags and Events are populated.</span></span>
<span data-ttu-id="459ff-165">记录的标志在 W3C TraceContext ID 中进行编码，暗示分布式跟踪中涉及的其他进程应对此跟踪进行采样。</span><span class="sxs-lookup"><span data-stu-id="459ff-165">The Recorded flag is encoded in the W3C TraceContext ID and is a hint to other processes involved in the distributed trace that this trace should be sampled.</span></span>
- <span data-ttu-id="459ff-166"><xref:System.Diagnostics.ActivityListener.ActivityStarted> 和 <xref:System.Diagnostics.ActivityListener.ActivityStopped> 分别在启动和停止活动时调用。</span><span class="sxs-lookup"><span data-stu-id="459ff-166"><xref:System.Diagnostics.ActivityListener.ActivityStarted> and <xref:System.Diagnostics.ActivityListener.ActivityStopped> are called when an Activity is started and stopped respectively.</span></span> <span data-ttu-id="459ff-167">可以通过这些回调记录有关活动的相关信息或对其进行修改。</span><span class="sxs-lookup"><span data-stu-id="459ff-167">These callbacks provide an oportunity to record relevant information about the Activity or potentially to modify it.</span></span>
<span data-ttu-id="459ff-168">当活动刚开始时，许多数据可能仍然不完整，并将在活动停止之前进行填充。</span><span class="sxs-lookup"><span data-stu-id="459ff-168">When an Activity has just started much of the data may still be incomplete and it will be populated before the Activity stops.</span></span>

<span data-ttu-id="459ff-169">创建 ActivityListener 并填充回调之后，即可通过调用 <xref:System.Diagnostics.ActivitySource.AddActivityListener(System.Diagnostics.ActivityListener)?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="459ff-169">Once an ActivityListener has been created and the callbacks are populated, calling <xref:System.Diagnostics.ActivitySource.AddActivityListener(System.Diagnostics.ActivityListener)?displayProperty=nameWithType></span></span>
<span data-ttu-id="459ff-170">启动调用回调操作。</span><span class="sxs-lookup"><span data-stu-id="459ff-170">initiates invoking the callbacks.</span></span> <span data-ttu-id="459ff-171">调用 <xref:System.Diagnostics.ActivityListener.Dispose?displayProperty=nameWithType> 可停止回调流。</span><span class="sxs-lookup"><span data-stu-id="459ff-171">Call <xref:System.Diagnostics.ActivityListener.Dispose?displayProperty=nameWithType> to stop the flow of callbacks.</span></span> <span data-ttu-id="459ff-172">请注意，在多线程代码中，当 Dispose() 运行时，甚至在它返回后不久，都可能会收到正在进行的回调通知。</span><span class="sxs-lookup"><span data-stu-id="459ff-172">Beware that in multi-threaded code callback notifications in progress could be received while Dispose() is running or even very shortly after it has returned.</span></span>
