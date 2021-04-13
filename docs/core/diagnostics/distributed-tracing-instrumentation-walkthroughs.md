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
# <a name="adding-distributed-tracing-instrumentation"></a><span data-ttu-id="94cdc-103">添加分布式跟踪检测</span><span class="sxs-lookup"><span data-stu-id="94cdc-103">Adding distributed tracing instrumentation</span></span>

<span data-ttu-id="94cdc-104">本文适用范围：✔️ .NET Core 2.1 及更高版本和 .NET Framework 4.5 及更高版本</span><span class="sxs-lookup"><span data-stu-id="94cdc-104">**This article applies to: ✔️** .NET Core 2.1 and later versions **and** .NET Framework 4.5 and later versions</span></span>

<span data-ttu-id="94cdc-105">可以使用 <xref:System.Diagnostics.Activity?displayProperty=nameWithType> API 检测 .NET 应用程序，以生成分布式跟踪遥测。</span><span class="sxs-lookup"><span data-stu-id="94cdc-105">.NET applications can be instrumented using the <xref:System.Diagnostics.Activity?displayProperty=nameWithType> API to produce distributed tracing telemetry.</span></span> <span data-ttu-id="94cdc-106">一些检测内置于标准 .NET 库中，但你可能会想要添加更多检测来让代码更易于诊断。</span><span class="sxs-lookup"><span data-stu-id="94cdc-106">Some instrumentation is built-in to standard .NET libraries but you may want to add more to make your code more easily diagnosable.</span></span> <span data-ttu-id="94cdc-107">在本教程中，你将添加新的自定义分布式跟踪检测。</span><span class="sxs-lookup"><span data-stu-id="94cdc-107">In this tutorial you will add new custom distributed tracing instrumentation.</span></span> <span data-ttu-id="94cdc-108">请参阅[集合教程](distributed-tracing-instrumentation-walkthroughs.md)，详细了解如何记录此检测生成的遥测。</span><span class="sxs-lookup"><span data-stu-id="94cdc-108">See [the collection tutorial](distributed-tracing-instrumentation-walkthroughs.md) to learn more about recording the telemetry produced by this instrumentation.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94cdc-109">先决条件</span><span class="sxs-lookup"><span data-stu-id="94cdc-109">Prerequisites</span></span>

- <span data-ttu-id="94cdc-110">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="94cdc-110">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet) or a later version</span></span>

## <a name="an-initial-app"></a><span data-ttu-id="94cdc-111">初始应用</span><span class="sxs-lookup"><span data-stu-id="94cdc-111">An initial app</span></span>

<span data-ttu-id="94cdc-112">首先，将创建使用 OpenTelemetry 收集遥测，但尚未安装任何检测的示例应用。</span><span class="sxs-lookup"><span data-stu-id="94cdc-112">First you will create a sample app that collects telemetry using OpenTelemetry, but doesn't yet have any instrumentation.</span></span>

```dotnetcli
dotnet new console
```

<span data-ttu-id="94cdc-113">面向 .NET 5 及更高版本的应用程序已包含必要的分布式跟踪 API。</span><span class="sxs-lookup"><span data-stu-id="94cdc-113">Applications that target .NET 5 and later already have the necessary distributed tracing APIs included.</span></span> <span data-ttu-id="94cdc-114">对于面向早期 .NET 版本的应用，请添加 [System.Diagnostics.DiagnosticSource NuGet 包](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/)版本 5 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="94cdc-114">For apps targeting older .NET versions add the [System.Diagnostics.DiagnosticSource NuGet package](https://www.nuget.org/packages/System.Diagnostics.DiagnosticSource/) version 5 or greater.</span></span>

```dotnetcli
dotnet add package System.Diagnostics.DiagnosticSource
```

<span data-ttu-id="94cdc-115">添加将用于收集遥测的 [OpenTelemetry](https://www.nuget.org/packages/OpenTelemetry/) 和 [OpenTelemetry.Exporter.Console](https://www.nuget.org/packages/OpenTelemetry.Exporter.Console/) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="94cdc-115">Add the [OpenTelemetry](https://www.nuget.org/packages/OpenTelemetry/) and [OpenTelemetry.Exporter.Console](https://www.nuget.org/packages/OpenTelemetry.Exporter.Console/) NuGet packages which will be used to collect the telemetry.</span></span>

```dotnetcli
dotnet add package OpenTelemetry
dotnet add package OpenTelemetry.Exporter.Console
```

<span data-ttu-id="94cdc-116">将生成的 Program.cs 内容替换为此示例源：</span><span class="sxs-lookup"><span data-stu-id="94cdc-116">Replace the contents of the generated Program.cs with this example source:</span></span>

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

<span data-ttu-id="94cdc-117">该应用尚无检测，因此没有要显示的跟踪信息：</span><span class="sxs-lookup"><span data-stu-id="94cdc-117">The app has no instrumentation yet so there is no trace information to display:</span></span>

```dotnetcli
> dotnet run
Example work done
```

#### <a name="best-practices"></a><span data-ttu-id="94cdc-118">最佳实践</span><span class="sxs-lookup"><span data-stu-id="94cdc-118">Best Practices</span></span>

<span data-ttu-id="94cdc-119">仅应用开发人员需要引用可选的第三方库用于收集分布式跟踪遥测，例如本示例中的 OpenTelemetry。</span><span class="sxs-lookup"><span data-stu-id="94cdc-119">Only app developers need to reference an optional 3rd party library for collecting the distributed trace telemetry, such as OpenTelemetry in this example.</span></span> <span data-ttu-id="94cdc-120">.NET 库创建者可以仅依赖于 System.Diagnostics.DiagnosticSource（.NET 运行时的一部分）中的 API。</span><span class="sxs-lookup"><span data-stu-id="94cdc-120">.NET library authors can exclusively rely on APIs in System.Diagnostics.DiagnosticSource which is part of .NET runtime.</span></span> <span data-ttu-id="94cdc-121">这样可确保库在各种 .NET 应用中运行，而无论应用开发人员在用于收集遥测的库或供应商方面有何偏好。</span><span class="sxs-lookup"><span data-stu-id="94cdc-121">This ensures that libraries will run in a wide range of .NET apps, regardless of the app developer's preferences about which library or vendor to use for collecting telemetry.</span></span>

## <a name="adding-basic-instrumentation"></a><span data-ttu-id="94cdc-122">添加基本检测</span><span class="sxs-lookup"><span data-stu-id="94cdc-122">Adding Basic Instrumentation</span></span>

<span data-ttu-id="94cdc-123">应用程序和库使用 <xref:System.Diagnostics.ActivitySource?displayProperty=nameWithType> 和 <xref:System.Diagnostics.Activity?displayProperty=nameWithType> 类添加分布式跟踪检测。</span><span class="sxs-lookup"><span data-stu-id="94cdc-123">Applications and libraries add distributed tracing instrumentation using the <xref:System.Diagnostics.ActivitySource?displayProperty=nameWithType> and <xref:System.Diagnostics.Activity?displayProperty=nameWithType> classes.</span></span>

### <a name="activitysource"></a><span data-ttu-id="94cdc-124">ActivitySource</span><span class="sxs-lookup"><span data-stu-id="94cdc-124">ActivitySource</span></span>

<span data-ttu-id="94cdc-125">首先创建 ActivitySource 实例。</span><span class="sxs-lookup"><span data-stu-id="94cdc-125">First create an instance of ActivitySource.</span></span> <span data-ttu-id="94cdc-126">ActivitySource 提供用于创建和启动 Activity 对象的 API。</span><span class="sxs-lookup"><span data-stu-id="94cdc-126">ActivitySource provides APIs to create and start Activity objects.</span></span> <span data-ttu-id="94cdc-127">将 Main() 和 `using System.Diagnostics;` 上方的静态 ActivitySource 变量添加到 using 语句。</span><span class="sxs-lookup"><span data-stu-id="94cdc-127">Add the static ActivitySource variable above Main() and `using System.Diagnostics;` to the using statements.</span></span>

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

#### <a name="best-practices"></a><span data-ttu-id="94cdc-128">最佳实践</span><span class="sxs-lookup"><span data-stu-id="94cdc-128">Best Practices</span></span>

- <span data-ttu-id="94cdc-129">创建一次 ActivitySource，将它存储在静态变量中，并根据需要使用相应实例。</span><span class="sxs-lookup"><span data-stu-id="94cdc-129">Create the ActivitySource once, store it in a static variable and use that instance as long as needed.</span></span>
<span data-ttu-id="94cdc-130">每个库或库子组件都可以（并且通常应该）创建自己的源。</span><span class="sxs-lookup"><span data-stu-id="94cdc-130">Each library or library sub-component can (and often should) create its own source.</span></span> <span data-ttu-id="94cdc-131">如果预期应用开发人员想要能够独立启用和禁用源中的 Activity 遥测，请考虑创建新源，而不是重复使用现有源。</span><span class="sxs-lookup"><span data-stu-id="94cdc-131">Consider creating a new source rather than re-using an existing one if you anticipate app developers would appreciate being able to enable and disable the Activity telemetry in the sources independently.</span></span>

- <span data-ttu-id="94cdc-132">传递给构造函数的源名称必须是唯一的，以免与其他任何源发生冲突。</span><span class="sxs-lookup"><span data-stu-id="94cdc-132">The source name passed to the constructor has to be unique to avoid the conflicts with any other sources.</span></span>
<span data-ttu-id="94cdc-133">建议使用包含程序集名称的层次结构名称，如果同一程序集内有多个源，则还可以使用包含组件名称的层次结构名称。</span><span class="sxs-lookup"><span data-stu-id="94cdc-133">It is recommended to use a hierarchical name that contains the assembly name and optionally a component name if there are multiple sources within the same assembly.</span></span> <span data-ttu-id="94cdc-134">例如，`Microsoft.AspNetCore.Hosting`。</span><span class="sxs-lookup"><span data-stu-id="94cdc-134">For example, `Microsoft.AspNetCore.Hosting`.</span></span> <span data-ttu-id="94cdc-135">如果程序集在第二个独立程序集中添加代码检测，则名称应基于定义 ActivitySource 的程序集，而不是要检测其代码的程序集。</span><span class="sxs-lookup"><span data-stu-id="94cdc-135">If an assembly is adding instrumentation for code in a 2nd independent assembly, the name should be based on the assembly that defines the ActivitySource, not the assembly whose code is being instrumented.</span></span>

- <span data-ttu-id="94cdc-136">version 是可选参数。</span><span class="sxs-lookup"><span data-stu-id="94cdc-136">The version parameter is optional.</span></span> <span data-ttu-id="94cdc-137">建议在发布库的多个版本时提供 version 并更改已检测的遥测。</span><span class="sxs-lookup"><span data-stu-id="94cdc-137">It is recommended to provide the version in case you release multiple versions of the library and make changes to the instrumented telemetry.</span></span>

> [!NOTE]
> <span data-ttu-id="94cdc-138">OpenTelemetry 使用替代术语“Tracer”和“Span”。</span><span class="sxs-lookup"><span data-stu-id="94cdc-138">OpenTelemetry uses alternate terms 'Tracer' and 'Span'.</span></span> <span data-ttu-id="94cdc-139">在 .NET 中，“ActivitySource”是 Tracer 的实现，而 Activity 则是“Span”的实现。</span><span class="sxs-lookup"><span data-stu-id="94cdc-139">In .NET 'ActivitySource' is the implementation of Tracer and Activity is the implementation of 'Span'.</span></span> <span data-ttu-id="94cdc-140">.NET 的 Activity 类型远早于 OpenTelemetry 规范，并且已保留原始 .NET 命名，以确保 .NET 生态系统内的一致性和 .NET 应用程序兼容性。</span><span class="sxs-lookup"><span data-stu-id="94cdc-140">.NET's Activity type long pre-dates the OpenTelemetry specification and the original .NET naming has been preserved for consistency within the .NET ecosystem and .NET application compatibility.</span></span>

### <a name="activity-creation"></a><span data-ttu-id="94cdc-141">创建 Activity 对象</span><span class="sxs-lookup"><span data-stu-id="94cdc-141">Activity Creation</span></span>

<span data-ttu-id="94cdc-142">使用 ActivitySource 对象，根据有意义的工作单元启动和停止 Activity 对象。</span><span class="sxs-lookup"><span data-stu-id="94cdc-142">Use the ActivitySource object to Start and Stop Activity objects around meaningful units of work.</span></span> <span data-ttu-id="94cdc-143">使用下面显示的代码更新 DoSomeWork()：</span><span class="sxs-lookup"><span data-stu-id="94cdc-143">Update DoSomeWork() with the code shown here:</span></span>

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

<span data-ttu-id="94cdc-144">现在运行应用会显示正在记录的新 Activity：</span><span class="sxs-lookup"><span data-stu-id="94cdc-144">Running the app now shows the new Activity being logged:</span></span>

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

#### <a name="notes"></a><span data-ttu-id="94cdc-145">说明</span><span class="sxs-lookup"><span data-stu-id="94cdc-145">Notes</span></span>

- <span data-ttu-id="94cdc-146"><xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 同时创建和启动 Activity。</span><span class="sxs-lookup"><span data-stu-id="94cdc-146"><xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> creates and starts the activity at the same time.</span></span> <span data-ttu-id="94cdc-147">列出的代码模式使用的是 `using` 块，它会在执行此块后自动释放创建的 Activity 对象。</span><span class="sxs-lookup"><span data-stu-id="94cdc-147">The listed code pattern is using the `using` block which automatically disposes the created Activity object after executing the block.</span></span> <span data-ttu-id="94cdc-148">释放 Activity 对象将停止它，因此代码无需显式调用 <xref:System.Diagnostics.Activity.Stop?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="94cdc-148">Disposing the Activity object will stop it so the code doesn't need to explicitly call <xref:System.Diagnostics.Activity.Stop?displayProperty=nameWithType>.</span></span>
<span data-ttu-id="94cdc-149">这简化了编码模式。</span><span class="sxs-lookup"><span data-stu-id="94cdc-149">That simplifies the coding pattern.</span></span>

- <span data-ttu-id="94cdc-150"><xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 在内部确定是否有任何侦听器记录 Activity。</span><span class="sxs-lookup"><span data-stu-id="94cdc-150"><xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> internally determines if there are any listeners recording the Activity.</span></span> <span data-ttu-id="94cdc-151">如果没有已注册的侦听器，或有不关注此类事件的侦听器，那么 StartActivity() 会返回 `null`，并避免创建 Activity 对象。</span><span class="sxs-lookup"><span data-stu-id="94cdc-151">If there are no registered listeners or there are listeners which are not interested, StartActivity() will return `null` and avoid creating the Activity object.</span></span> <span data-ttu-id="94cdc-152">这是一项性能优化，以便代码模式仍可用于频繁调用的函数。</span><span class="sxs-lookup"><span data-stu-id="94cdc-152">This is a performance optimization so that the code pattern can still be used in functions that are called very frequently.</span></span>

## <a name="optional-populate-tags"></a><span data-ttu-id="94cdc-153">可选：填充标记</span><span class="sxs-lookup"><span data-stu-id="94cdc-153">Optional: Populate tags</span></span>

<span data-ttu-id="94cdc-154">Activity 支持名为标记的键值数据，后者通常用于存储可能对诊断有用的工作的所有参数。</span><span class="sxs-lookup"><span data-stu-id="94cdc-154">Activities support key-value data called Tags, commonly used to store any parameters of the work that may be useful for diagnostics.</span></span> <span data-ttu-id="94cdc-155">更新 DoSomeWork() 以包括它们：</span><span class="sxs-lookup"><span data-stu-id="94cdc-155">Update DoSomeWork() to include them:</span></span>

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

#### <a name="best-practices"></a><span data-ttu-id="94cdc-156">最佳实践</span><span class="sxs-lookup"><span data-stu-id="94cdc-156">Best Practices</span></span>

- <span data-ttu-id="94cdc-157">如上所述，<xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 返回的 `activity` 可能为 null。</span><span class="sxs-lookup"><span data-stu-id="94cdc-157">As mentioned above, `activity` returned by <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> may be null.</span></span> <span data-ttu-id="94cdc-158">C# 中的 null 合并运算符 ?.</span><span class="sxs-lookup"><span data-stu-id="94cdc-158">The null-coallescing operator ?.</span></span> <span data-ttu-id="94cdc-159">是一个方便的快捷方式，可在 Activity 不为 null 时仅调用 <xref:System.Diagnostics.Activity.SetTag%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="94cdc-159">in C# is a convenient short-hand to only invoke <xref:System.Diagnostics.Activity.SetTag%2A?displayProperty=nameWithType> if activity is not null.</span></span> <span data-ttu-id="94cdc-160">该行为等同于写入：</span><span class="sxs-lookup"><span data-stu-id="94cdc-160">The behavior is identical to writing:</span></span>

```csharp
if(activity != null)
{
    activity.SetTag("foo", foo);
}
```

- <span data-ttu-id="94cdc-161">OpenTelemetry 提供一组建议的[约定](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/trace/semantic_conventions)，用于在 Activity 上设置代表常见应用程序工作类型的标记。</span><span class="sxs-lookup"><span data-stu-id="94cdc-161">OpenTelemetry provides a set of recommended [conventions](https://github.com/open-telemetry/opentelemetry-specification/tree/main/specification/trace/semantic_conventions) for setting Tags on Activities that represent common types of application work.</span></span>

- <span data-ttu-id="94cdc-162">如果要检测具有高性能要求的函数，则 <xref:System.Diagnostics.Activity.IsAllDataRequested?displayProperty=nameWithType> 是一个提示，可指示侦听 Activity 的任何代码是否打算读取辅助信息，例如标记。</span><span class="sxs-lookup"><span data-stu-id="94cdc-162">If you are instrumenting functions with high performance requirements, <xref:System.Diagnostics.Activity.IsAllDataRequested?displayProperty=nameWithType> is a hint that indicates whether any of the code listening to Activities intends to read auxilliary information such as Tags.</span></span> <span data-ttu-id="94cdc-163">如果没有侦听器要进行读取，则检测的代码无需耗费 CPU 周期来填充它。</span><span class="sxs-lookup"><span data-stu-id="94cdc-163">If no listener will read it then there is no need for the instrumented code to spend CPU cycles populating it.</span></span> <span data-ttu-id="94cdc-164">为简单起见，此示例未应用该优化。</span><span class="sxs-lookup"><span data-stu-id="94cdc-164">For simplicity this sample doesn't apply that optimization.</span></span>

## <a name="optional-adding-events"></a><span data-ttu-id="94cdc-165">可选：添加事件</span><span class="sxs-lookup"><span data-stu-id="94cdc-165">Optional: Adding Events</span></span>

<span data-ttu-id="94cdc-166">事件是带有时间戳的消息，可以将任意附加诊断数据流附加到 Activity。</span><span class="sxs-lookup"><span data-stu-id="94cdc-166">Events are timestamped messages that can attach an arbitrary stream of additional diagnostic data to Activities.</span></span> <span data-ttu-id="94cdc-167">向 Activity 添加一些事件：</span><span class="sxs-lookup"><span data-stu-id="94cdc-167">Add some events to the Activity:</span></span>

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

#### <a name="best-practices"></a><span data-ttu-id="94cdc-168">最佳实践</span><span class="sxs-lookup"><span data-stu-id="94cdc-168">Best Practices</span></span>

- <span data-ttu-id="94cdc-169">事件存储在内存中列表中，直到可以传输，这使得此机制仅适合记录适量的事件。</span><span class="sxs-lookup"><span data-stu-id="94cdc-169">Events are stored in an in-memory list until they can be transmitted which makes this mechanism only suitable for recording a modest number of events.</span></span> <span data-ttu-id="94cdc-170">对于大量或不限量的事件，使用专注于此任务的日志记录 API（例如 [ILogger](https://docs.microsoft.com/aspnet/core/fundamentals/logging/)）是更好的选择。</span><span class="sxs-lookup"><span data-stu-id="94cdc-170">For a large or unbounded volume of events using a logging API focused on this task such as [ILogger](https://docs.microsoft.com/aspnet/core/fundamentals/logging/) is a better choice.</span></span> <span data-ttu-id="94cdc-171">ILogger 还可确保无论应用开发人员是否选择使用分布式跟踪，日志记录信息都将可用。</span><span class="sxs-lookup"><span data-stu-id="94cdc-171">ILogger also ensures that the logging information will be available regardless whether the app developer opts to use distributed tracing.</span></span>
<span data-ttu-id="94cdc-172">ILogger 支持自动捕获活动 Activity ID，因此仍可以将通过该 API 记录的消息与分布式跟踪关联。</span><span class="sxs-lookup"><span data-stu-id="94cdc-172">ILogger supports automatically capturing the active Activity IDs so messages logged via that API can still be correlated with the distributed trace.</span></span>

## <a name="optional-adding-status"></a><span data-ttu-id="94cdc-173">可选：添加状态</span><span class="sxs-lookup"><span data-stu-id="94cdc-173">Optional: Adding Status</span></span>

<span data-ttu-id="94cdc-174">OpenTelemetry 允许每个 Activity 报告代表工作的通过/失败结果的[状态](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#set-status)。</span><span class="sxs-lookup"><span data-stu-id="94cdc-174">OpenTelemetry allows each Activity to report a [Status](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/api.md#set-status) that represents the pass/fail result of the work.</span></span> <span data-ttu-id="94cdc-175">.NET 目前没有用于此目的的强类型 API，但存在有关使用标记的既有约定：</span><span class="sxs-lookup"><span data-stu-id="94cdc-175">.NET does not currently have a strongly typed API for this purpose but there is an established convention using Tags:</span></span>

- <span data-ttu-id="94cdc-176">`otel.status_code` 是用于存储 `StatusCode` 的标记名称。</span><span class="sxs-lookup"><span data-stu-id="94cdc-176">`otel.status_code` is the Tag name used to store `StatusCode`.</span></span> <span data-ttu-id="94cdc-177">StatusCode 标记的值必须是字符串“UNSET”、“OK”或“ERROR”之一，其分别对应于 StatusCode 枚举 `Unset`、`Ok` 和 `Error`。</span><span class="sxs-lookup"><span data-stu-id="94cdc-177">Values for the StatusCode tag must be one of the strings "UNSET", "OK", or "ERROR", which correspond respectively to the enums `Unset`, `Ok`, and `Error` from StatusCode.</span></span>
- <span data-ttu-id="94cdc-178">`otel.status_description` 是用于存储可选 `Description` 的标记名称</span><span class="sxs-lookup"><span data-stu-id="94cdc-178">`otel.status_description` is the Tag name used to store the optional `Description`</span></span>

<span data-ttu-id="94cdc-179">更新 DoSomeWork() 以设置状态：</span><span class="sxs-lookup"><span data-stu-id="94cdc-179">Update DoSomeWork() to set status:</span></span>

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

## <a name="optional-add-additional-activities"></a><span data-ttu-id="94cdc-180">可选：添加其他 Activity</span><span class="sxs-lookup"><span data-stu-id="94cdc-180">Optional: Add Additional Activities</span></span>

<span data-ttu-id="94cdc-181">可以嵌套 Activity 用于描述较大工作单元的各个部分。</span><span class="sxs-lookup"><span data-stu-id="94cdc-181">Activities can be nested to describe portions of a larger unit of work.</span></span> <span data-ttu-id="94cdc-182">这对于可能不会快速执行的代码部分或更好地找到来自特定外部依赖项的故障而言尤其有价值。</span><span class="sxs-lookup"><span data-stu-id="94cdc-182">This can be particularly valuable around portions of code that might not execute quickly or to better localize failures that come from specific external dependencies.</span></span> <span data-ttu-id="94cdc-183">尽管此示例在每种方法中都使用 Activity，但这仅仅是因为已最大限度地减少了额外的代码。</span><span class="sxs-lookup"><span data-stu-id="94cdc-183">Although this sample uses an Activity in every method, that is solely because extra code has been minimized.</span></span> <span data-ttu-id="94cdc-184">在更大、更真实的项目中，在每种方法中都使用 Activity 会产生极其详细的跟踪，因此不建议使用。</span><span class="sxs-lookup"><span data-stu-id="94cdc-184">In a larger and more realistic project using an Activity in every method would produce extremely verbose traces so it is not recommended.</span></span>

<span data-ttu-id="94cdc-185">更新 StepOne 和 StepTwo，以围绕这些单独的步骤添加更多跟踪：</span><span class="sxs-lookup"><span data-stu-id="94cdc-185">Update StepOne and StepTwo to add more tracing around these separate steps:</span></span>

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

<span data-ttu-id="94cdc-186">请注意，StepOne 和 StepTwo 都包含引用 SomeWork 的 ParentId。</span><span class="sxs-lookup"><span data-stu-id="94cdc-186">Notice that both StepOne and StepTwo include a ParentId that refers to SomeWork.</span></span> <span data-ttu-id="94cdc-187">控制台不能很好地呈现嵌套工作树，但许多 GUI 查看器（例如 [Zipkin](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/src/OpenTelemetry.Exporter.Zipkin/README.md)）都可以将其显示为甘特图：</span><span class="sxs-lookup"><span data-stu-id="94cdc-187">The console is not a great visualization of nested trees of work, but many GUI viewers such as [Zipkin](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/src/OpenTelemetry.Exporter.Zipkin/README.md) can show this as a Gantt chart:</span></span>

<span data-ttu-id="94cdc-188">[![Zipkin 甘特图](media/zipkin-nested-activities.jpg)](media/zipkin-nested-activities.jpg)</span><span class="sxs-lookup"><span data-stu-id="94cdc-188">[![Zipkin Gantt chart](media/zipkin-nested-activities.jpg)](media/zipkin-nested-activities.jpg)</span></span>

### <a name="optional-activitykind"></a><span data-ttu-id="94cdc-189">可选：ActivityKind</span><span class="sxs-lookup"><span data-stu-id="94cdc-189">Optional: ActivityKind</span></span>

<span data-ttu-id="94cdc-190">Activity 包含描述 Activity、其父项和子项之间关系的 <xref:System.Diagnostics.Activity.Kind%2A?displayProperty=nameWithType> 属性。</span><span class="sxs-lookup"><span data-stu-id="94cdc-190">Activities have an <xref:System.Diagnostics.Activity.Kind%2A?displayProperty=nameWithType> property which describes the relationship between the Activity, its parent and its children.</span></span> <span data-ttu-id="94cdc-191">默认情况下，所有新 Activity 都设置为 <xref:System.Diagnostics.ActivityKind.Internal>，这适用于属于应用程序中的内部操作且没有远程父项或子项的 Activity。</span><span class="sxs-lookup"><span data-stu-id="94cdc-191">By default all new Activities are set to <xref:System.Diagnostics.ActivityKind.Internal> which is appropriate for Activities that are an internal operation within an application with no remote parent or children.</span></span> <span data-ttu-id="94cdc-192">可以使用 <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType> 上的 kind 参数设置其他类型。</span><span class="sxs-lookup"><span data-stu-id="94cdc-192">Other kinds can be set using the kind parameter on <xref:System.Diagnostics.ActivitySource.StartActivity%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="94cdc-193">有关其他选项，请参阅 <xref:System.Diagnostics.ActivityKind?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="94cdc-193">See <xref:System.Diagnostics.ActivityKind?displayProperty=nameWithType> for other options.</span></span>

### <a name="optional-links"></a><span data-ttu-id="94cdc-194">可选：链接</span><span class="sxs-lookup"><span data-stu-id="94cdc-194">Optional: Links</span></span>

<span data-ttu-id="94cdc-195">当工作在批处理系统中发生时，单个 Activity 可能表示同时代表许多不同请求的工作，且其中每个请求都有自己的跟踪 ID。尽管 Activity 被限制为具有单个父项，但它可以使用 <xref:System.Diagnostics.ActivityLink?displayProperty=nameWithType> 链接到其他跟踪 ID。</span><span class="sxs-lookup"><span data-stu-id="94cdc-195">When work occurs in batch processing systems a single Activity might represent work on behalf of many different requests simultaneously, each of which has its own trace-id. Although Activity is restricted to have a single parent, it can link to additional trace-ids using <xref:System.Diagnostics.ActivityLink?displayProperty=nameWithType>.</span></span> <span data-ttu-id="94cdc-196">每个 ActivityLink 都填充有 <xref:System.Diagnostics.ActivityContext>，后者存储有关要链接到的 Activity 的 ID 信息。</span><span class="sxs-lookup"><span data-stu-id="94cdc-196">Each ActivityLink is populated with an <xref:System.Diagnostics.ActivityContext> that stores ID information about the Activity being linked to.</span></span> <span data-ttu-id="94cdc-197">可以使用 <xref:System.Diagnostics.Activity.Context%2A?displayProperty=nameWithType> 从进程内 Activity 对象中检索 ActivityContext，也可以使用 <xref:System.Diagnostics.ActivityContext.Parse(System.String,System.String)?displayProperty=nameWithType> 从序列化 ID 信息中分析它。</span><span class="sxs-lookup"><span data-stu-id="94cdc-197">ActivityContext can be retrieved from in-process Activity objects using <xref:System.Diagnostics.Activity.Context%2A?displayProperty=nameWithType> or it can be parsed from serialized id information using <xref:System.Diagnostics.ActivityContext.Parse(System.String,System.String)?displayProperty=nameWithType>.</span></span>

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

<span data-ttu-id="94cdc-198">与可以按需添加的事件和标记不同，链接必须在 StartActivity() 期间添加且此后不可变。</span><span class="sxs-lookup"><span data-stu-id="94cdc-198">Unlike events and Tags that can be added on-demand, links must be added during StartActivity() and are immutable afterwards.</span></span>
