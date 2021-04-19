---
title: 在 .NET 中使用依赖注入
description: 了解如何在 .NET 应用程序中使用依赖注入。
author: IEvangelist
ms.author: dapine
ms.date: 04/12/2021
ms.topic: tutorial
no-loc:
- Transient
- Scoped
- Singleton
- Example
ms.openlocfilehash: 03828496dfa3487ad70fac8cf32ff81844eca6fd
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494267"
---
# <a name="tutorial-use-dependency-injection-in-net"></a><span data-ttu-id="8a6d3-103">教程：在 .NET 中使用依赖注入</span><span class="sxs-lookup"><span data-stu-id="8a6d3-103">Tutorial: Use dependency injection in .NET</span></span>

<span data-ttu-id="8a6d3-104">本教程介绍如何[在 .NET 中使用依赖注入 (DI)](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-104">This tutorial shows how to use [dependency injection (DI) in .NET](dependency-injection.md).</span></span> <span data-ttu-id="8a6d3-105">使用 Microsoft 扩展时，DI 是“一等公民”，其中服务是在 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 中添加和配置的。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-105">With *Microsoft Extensions*, DI is a first-class citizen where services are added and configured in an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="8a6d3-106"><xref:Microsoft.Extensions.Hosting.IHost> 接口会公开 <xref:System.IServiceProvider> 实例，它充当所有已注册的服务的容器。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-106">The <xref:Microsoft.Extensions.Hosting.IHost> interface exposes the <xref:System.IServiceProvider> instance, which acts as a container of all the registered services.</span></span>

<span data-ttu-id="8a6d3-107">本教程介绍如何执行下列操作：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="8a6d3-108">创建一个使用依赖注入的 .NET 控制台应用</span><span class="sxs-lookup"><span data-stu-id="8a6d3-108">Create a .NET console app that uses dependency injection</span></span>
> - <span data-ttu-id="8a6d3-109">生成和配置[通用主机](generic-host.md)</span><span class="sxs-lookup"><span data-stu-id="8a6d3-109">Build and configure a [Generic Host](generic-host.md)</span></span>
> - <span data-ttu-id="8a6d3-110">编写多个接口及相应的实现</span><span class="sxs-lookup"><span data-stu-id="8a6d3-110">Write several interfaces and corresponding implementations</span></span>
> - <span data-ttu-id="8a6d3-111">为 DI 使用服务生存期和范围设定</span><span class="sxs-lookup"><span data-stu-id="8a6d3-111">Use service lifetime and scoping for DI</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a6d3-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="8a6d3-112">Prerequisites</span></span>

- <span data-ttu-id="8a6d3-113">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-113">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) or later.</span></span>
- <span data-ttu-id="8a6d3-114">熟悉如何创建新的 .NET 应用程序以及如何安装 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-114">Familiarity with creating new .NET applications and installing NuGet packages.</span></span>

## <a name="create-a-new-console-application"></a><span data-ttu-id="8a6d3-115">创建新的控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="8a6d3-115">Create a new console application</span></span>

<span data-ttu-id="8a6d3-116">通过 [dotnet new](../tools/dotnet-new.md) 命令或 IDE 的“新建项目”向导，新建一个名为 ConsoleDI 的 .NET 控制台应用程序 **Example** 。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-116">Using either the [dotnet new](../tools/dotnet-new.md) command or an IDE new project wizard, create a new .NET console application named **ConsoleDI.Example**.</span></span> <span data-ttu-id="8a6d3-117">将 NuGet 包 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 添加到项目。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-117">Add the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) NuGet package to the project.</span></span>

## <a name="add-interfaces"></a><span data-ttu-id="8a6d3-118">添加接口</span><span class="sxs-lookup"><span data-stu-id="8a6d3-118">Add interfaces</span></span>

<span data-ttu-id="8a6d3-119">将以下接口添加到项目根目录：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-119">Add the following interfaces to the project root directory:</span></span>

<span data-ttu-id="8a6d3-120">IOperation.cs</span><span class="sxs-lookup"><span data-stu-id="8a6d3-120">*IOperation.cs*</span></span>

:::code language="csharp" source="snippets/configuration/console-di/IOperation.cs":::

<span data-ttu-id="8a6d3-121">`IOperation` 接口会定义一个 `OperationId` 属性。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-121">The `IOperation` interface defines a single `OperationId` property.</span></span>

<span data-ttu-id="8a6d3-122">IOperation.cs *Transient*</span><span class="sxs-lookup"><span data-stu-id="8a6d3-122">*ITransientOperation.cs*</span></span>

<span data-ttu-id="8a6d3-123">:::code language="csharp" source="snippets/configuration/console-di/ITransientOperation.cs":::</span><span class="sxs-lookup"><span data-stu-id="8a6d3-123">:::code language="csharp" source="snippets/configuration/console-di/ITransientOperation.cs":::</span></span>

<span data-ttu-id="8a6d3-124">IOperation.cs *Scoped*</span><span class="sxs-lookup"><span data-stu-id="8a6d3-124">*IScopedOperation.cs*</span></span>

<span data-ttu-id="8a6d3-125">:::code language="csharp" source="snippets/configuration/console-di/IScopedOperation.cs":::</span><span class="sxs-lookup"><span data-stu-id="8a6d3-125">:::code language="csharp" source="snippets/configuration/console-di/IScopedOperation.cs":::</span></span>

<span data-ttu-id="8a6d3-126">IOperation.cs *Singleton*</span><span class="sxs-lookup"><span data-stu-id="8a6d3-126">*ISingletonOperation.cs*</span></span>

<span data-ttu-id="8a6d3-127">:::code language="csharp" source="snippets/configuration/console-di/ISingletonOperation.cs":::</span><span class="sxs-lookup"><span data-stu-id="8a6d3-127">:::code language="csharp" source="snippets/configuration/console-di/ISingletonOperation.cs":::</span></span>

<span data-ttu-id="8a6d3-128">`IOperation` 的所有子接口会会命名其预期服务生存期。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-128">All of the subinterfaces of `IOperation` name their intended service lifetime.</span></span> <span data-ttu-id="8a6d3-129">例如，“Transient”或“Singleton”。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-129">For example, "Transient" or "Singleton".</span></span>

## <a name="add-default-implementation"></a><span data-ttu-id="8a6d3-130">添加默认实现</span><span class="sxs-lookup"><span data-stu-id="8a6d3-130">Add default implementation</span></span>

<span data-ttu-id="8a6d3-131">添加以下默认实现来进行各种操作：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-131">Add the following default implementation for the various operations:</span></span>

<span data-ttu-id="8a6d3-132">DefaultOperation.cs</span><span class="sxs-lookup"><span data-stu-id="8a6d3-132">*DefaultOperation.cs*</span></span>

:::code language="csharp" source="snippets/configuration/console-di/DefaultOperation.cs":::

<span data-ttu-id="8a6d3-133">`DefaultOperation` 会实现所有已命名的标记接口，并将 `OperationId` 属性初始化为新的全局唯一标识符 (GUID) 的最后 4 个字符。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-133">The `DefaultOperation` implements all of the named marker interfaces and initializes the `OperationId` property to the last four characters of a new globally unique identifier (GUID).</span></span>

## <a name="add-service-that-requires-di"></a><span data-ttu-id="8a6d3-134">添加需要 DI 的服务</span><span class="sxs-lookup"><span data-stu-id="8a6d3-134">Add service that requires DI</span></span>

<span data-ttu-id="8a6d3-135">添加以下操作记录器对象，它作为服务添加到控制台应用：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-135">Add the following operation logger object, which acts as a service to the console app:</span></span>

<span data-ttu-id="8a6d3-136">OperationLogger.cs</span><span class="sxs-lookup"><span data-stu-id="8a6d3-136">*OperationLogger.cs*</span></span>

:::code language="csharp" source="snippets/configuration/console-di/OperationLogger.cs":::

<span data-ttu-id="8a6d3-137">`OperationLogger` 会定义一个构造函数，该函数需要上述每一个标记接口（即 `ITransientOperation`、`IScopedOperation` 和 `ISingletonOperation`）。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-137">The `OperationLogger` defines a constructor that requires each of the aforementioned marker interfaces, that is; `ITransientOperation`, `IScopedOperation`, and `ISingletonOperation`.</span></span> <span data-ttu-id="8a6d3-138">对象会公开一个方法，使用者可通过该方法使用给定的 `scope` 参数记录操作。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-138">The object exposes a single method that allows the consumer to log the operations with a given `scope` parameter.</span></span> <span data-ttu-id="8a6d3-139">被调用时，`LogOperations` 方法会使用范围字符串和消息记录每个操作的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-139">When invoked, the `LogOperations` method logs each operation's unique identifier with the scope string and message.</span></span>

## <a name="register-services-for-di"></a><span data-ttu-id="8a6d3-140">为 DI 注册服务</span><span class="sxs-lookup"><span data-stu-id="8a6d3-140">Register services for DI</span></span>

<span data-ttu-id="8a6d3-141">使用以下代码更新 Program.cs：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-141">Update *Program.cs* with the following code:</span></span>

:::code language="csharp" source="snippets/configuration/console-di/Program.cs" range="1-18,35-60" highlight="22-26":::

<span data-ttu-id="8a6d3-142">每个 `services.Add{SERVICE_NAME}` 扩展方法添加（并可能配置）服务。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-142">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="8a6d3-143">我们建议应用遵循此约定。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="8a6d3-144">将扩展方法置于 <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> 命名空间中以封装服务注册的组。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-144">Place extension methods in the <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> namespace to encapsulate groups of service registrations.</span></span> <span data-ttu-id="8a6d3-145">还包括用于 DI 扩展方法的命名空间部分 `Microsoft.Extensions.DependencyInjection`：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-145">Including the namespace portion `Microsoft.Extensions.DependencyInjection` for DI extension methods also:</span></span>

- <span data-ttu-id="8a6d3-146">允许在不添加其他 `using` 块的情况下在 [IntelliSense](/visualstudio/ide/using-intellisense) 中显示它们。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-146">Allows them to be displayed in [IntelliSense](/visualstudio/ide/using-intellisense) without adding additional `using` blocks.</span></span>
- <span data-ttu-id="8a6d3-147">在通常会调用这些扩展方法的 `Program` 或 `Startup` 类中，避免出现过多的 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-147">Prevents excessive `using` statements in the `Program` or `Startup` classes where these extension methods are typically called.</span></span>

<span data-ttu-id="8a6d3-148">应用会执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-148">The app:</span></span>

- <span data-ttu-id="8a6d3-149">使用[默认活页夹设置](generic-host.md#default-builder-settings)创建一个 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 实例。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-149">Creates an <xref:Microsoft.Extensions.Hosting.IHostBuilder> instance with the [default binder settings](generic-host.md#default-builder-settings).</span></span>
- <span data-ttu-id="8a6d3-150">配置服务并对其添加相应的服务生存期。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-150">Configures services and adds them with their corresponding service lifetime.</span></span>
- <span data-ttu-id="8a6d3-151">调用 <xref:Microsoft.Extensions.Hosting.IHostBuilder.Build> 并分配 <xref:Microsoft.Extensions.Hosting.IHost> 的实例。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-151">Calls <xref:Microsoft.Extensions.Hosting.IHostBuilder.Build> and assigns an instance of <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>
- <span data-ttu-id="8a6d3-152">调用 `ExemplifyScoping`，传入 <xref:Microsoft.Extensions.Hosting.IHost.Services?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-152">Calls `ExemplifyScoping`, passing in the <xref:Microsoft.Extensions.Hosting.IHost.Services?displayProperty=nameWithType>.</span></span>

## <a name="conclusion"></a><span data-ttu-id="8a6d3-153">结束语</span><span class="sxs-lookup"><span data-stu-id="8a6d3-153">Conclusion</span></span>

<span data-ttu-id="8a6d3-154">应用会显示如下例所示的输出：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-154">The app displays output similar to the following example:</span></span>

```console
Scope 1-Call 1 .GetRequiredService<OperationLogger>(): ITransientOperation [ 80f4...Always different        ]
Scope 1-Call 1 .GetRequiredService<OperationLogger>(): IScopedOperation    [ c878...Changes only with scope ]
Scope 1-Call 1 .GetRequiredService<OperationLogger>(): ISingletonOperation [ 1586...Always the same         ]
...
Scope 1-Call 2 .GetRequiredService<OperationLogger>(): ITransientOperation [ f3c0...Always different        ]
Scope 1-Call 2 .GetRequiredService<OperationLogger>(): IScopedOperation    [ c878...Changes only with scope ]
Scope 1-Call 2 .GetRequiredService<OperationLogger>(): ISingletonOperation [ 1586...Always the same         ]

Scope 2-Call 1 .GetRequiredService<OperationLogger>(): ITransientOperation [ f9af...Always different        ]
Scope 2-Call 1 .GetRequiredService<OperationLogger>(): IScopedOperation    [ 2bd0...Changes only with scope ]
Scope 2-Call 1 .GetRequiredService<OperationLogger>(): ISingletonOperation [ 1586...Always the same         ]
...
Scope 2-Call 2 .GetRequiredService<OperationLogger>(): ITransientOperation [ fa65...Always different        ]
Scope 2-Call 2 .GetRequiredService<OperationLogger>(): IScopedOperation    [ 2bd0...Changes only with scope ]
Scope 2-Call 2 .GetRequiredService<OperationLogger>(): ISingletonOperation [ 1586...Always the same         ]
```

<span data-ttu-id="8a6d3-155">在应用输出中，可看到：</span><span class="sxs-lookup"><span data-stu-id="8a6d3-155">From the app output, you can see that:</span></span>

- <span data-ttu-id="8a6d3-156">Transient 操作总是不同，每次检索服务时，都会创建一个新实例。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-156">Transient operations are always different, a new instance is created with every retrieval of the service.</span></span>
- <span data-ttu-id="8a6d3-157">Scoped 仅随着新范围更改，但在一个范围中是相同的实例。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-157">Scoped operations change only with a new scope, but are the same instance within a scope.</span></span>
- <span data-ttu-id="8a6d3-158">Singleton 操作总是相同，新实例仅被创建一次。</span><span class="sxs-lookup"><span data-stu-id="8a6d3-158">Singleton operations are always the same, a new instance is only created once.</span></span>

## <a name="see-also"></a><span data-ttu-id="8a6d3-159">另请参阅</span><span class="sxs-lookup"><span data-stu-id="8a6d3-159">See also</span></span>

* [<span data-ttu-id="8a6d3-160">依赖关系注入指南</span><span class="sxs-lookup"><span data-stu-id="8a6d3-160">Dependency injection guidelines</span></span>](dependency-injection-guidelines.md)
* [<span data-ttu-id="8a6d3-161">ASP.NET Core 中的依赖注入</span><span class="sxs-lookup"><span data-stu-id="8a6d3-161">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
