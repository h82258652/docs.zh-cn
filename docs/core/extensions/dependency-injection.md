---
title: .NET 中的依赖关系注入
description: 了解 .NET 如何实现依赖关系注入以及如何使用它。
author: IEvangelist
ms.author: dapine
ms.date: 10/28/2020
ms.topic: overview
ms.openlocfilehash: 0b5526f24f3ac658123acd030c3adf32c346422a
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079513"
---
# <a name="dependency-injection-in-net"></a><span data-ttu-id="0b982-103">.NET 中的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="0b982-103">Dependency injection in .NET</span></span>

<span data-ttu-id="0b982-104">.NET 支持依赖关系注入 (DI) 软件设计模式，这是一种在类及其依赖项之间实现[控制反转 (IoC)](../../architecture/modern-web-apps-azure/architectural-principles.md#dependency-inversion) 的技术。</span><span class="sxs-lookup"><span data-stu-id="0b982-104">.NET supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](../../architecture/modern-web-apps-azure/architectural-principles.md#dependency-inversion) between classes and their dependencies.</span></span> <span data-ttu-id="0b982-105">.NET 中的依赖关系注入是“[一等公民](https://en.wikipedia.org/wiki/First-class_citizen)”，提供配置、日志记录和选项模式。</span><span class="sxs-lookup"><span data-stu-id="0b982-105">Dependency injection in .NET is a [first-class citizen](https://en.wikipedia.org/wiki/First-class_citizen), along with configuration, logging, and the options pattern.</span></span>

<span data-ttu-id="0b982-106">依赖项是指另一个对象所依赖的对象。</span><span class="sxs-lookup"><span data-stu-id="0b982-106">A *dependency* is an object that another object depends on.</span></span> <span data-ttu-id="0b982-107">使用其他类所依赖的 `Write` 方法检查以下 `MessageWriter` 类：</span><span class="sxs-lookup"><span data-stu-id="0b982-107">Examine the following `MessageWriter` class with a `Write` method that other classes depend on:</span></span>

```csharp
public class MessageWriter
{
    public void Write(string message)
    {
        Console.WriteLine($"MessageWriter.Write(message: \"{message}\")");
    }
}
```

<span data-ttu-id="0b982-108">类可以创建 `MessageWriter` 类的实例，以便利用其 `Write` 方法。</span><span class="sxs-lookup"><span data-stu-id="0b982-108">A class can create an instance of the `MessageWriter` class to make use of its `Write` method.</span></span> <span data-ttu-id="0b982-109">在以下示例中，`MessageWriter` 类是 `Worker` 类的依赖项：</span><span class="sxs-lookup"><span data-stu-id="0b982-109">In the following example, the `MessageWriter` class is a dependency of the `Worker` class:</span></span>

```csharp
public class Worker : BackgroundService
{
    private readonly MessageWriter _messageWriter = new MessageWriter();

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            _messageWriter.Write($"Worker running at: {DateTimeOffset.Now}");
            await Task.Delay(1000, stoppingToken);
        }
    }
}
```

<span data-ttu-id="0b982-110">该类创建并直接依赖于 `MessageWriter` 类。</span><span class="sxs-lookup"><span data-stu-id="0b982-110">The class creates and directly depends on the `MessageWriter` class.</span></span> <span data-ttu-id="0b982-111">硬编码的依赖项（如前面的示例）会产生问题，应避免使用，原因如下：</span><span class="sxs-lookup"><span data-stu-id="0b982-111">Hard-coded dependencies, such as in the previous example, are problematic and should be avoided for the following reasons:</span></span>

- <span data-ttu-id="0b982-112">要用不同的实现替换 `MessageWriter`，必须修改 `Worker` 类。</span><span class="sxs-lookup"><span data-stu-id="0b982-112">To replace `MessageWriter` with a different implementation, the `Worker` class must be modified.</span></span>
- <span data-ttu-id="0b982-113">如果 `MessageWriter` 具有依赖项，则必须由 `Worker` 类对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="0b982-113">If `MessageWriter` has dependencies, they must also be configured by the `Worker` class.</span></span> <span data-ttu-id="0b982-114">在具有多个依赖于 `MessageWriter` 的类的大型项目中，配置代码将分散在整个应用中。</span><span class="sxs-lookup"><span data-stu-id="0b982-114">In a large project with multiple classes depending on `MessageWriter`, the configuration code becomes scattered across the app.</span></span>
- <span data-ttu-id="0b982-115">这种实现很难进行单元测试。</span><span class="sxs-lookup"><span data-stu-id="0b982-115">This implementation is difficult to unit test.</span></span> <span data-ttu-id="0b982-116">应用需使用模拟或存根 `MessageWriter` 类，而该类不能使用此方法。</span><span class="sxs-lookup"><span data-stu-id="0b982-116">The app should use a mock or stub `MessageWriter` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="0b982-117">依赖关系注入通过以下方式解决了这些问题：</span><span class="sxs-lookup"><span data-stu-id="0b982-117">Dependency injection addresses these problems through:</span></span>

- <span data-ttu-id="0b982-118">使用接口或基类将依赖关系实现抽象化。</span><span class="sxs-lookup"><span data-stu-id="0b982-118">The use of an interface or base class to abstract the dependency implementation.</span></span>
- <span data-ttu-id="0b982-119">在服务容器中注册依赖关系。</span><span class="sxs-lookup"><span data-stu-id="0b982-119">Registration of the dependency in a service container.</span></span> <span data-ttu-id="0b982-120">.NET 提供了一个内置的服务容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="0b982-120">.NET provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="0b982-121">服务通常在应用启动时注册，并追加到 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>。</span><span class="sxs-lookup"><span data-stu-id="0b982-121">Services are typically registered at the app's start-up, and appended to an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="0b982-122">添加所有服务后，可以使用 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> 创建服务容器。</span><span class="sxs-lookup"><span data-stu-id="0b982-122">Once all services are added, you use <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> to create the service container.</span></span>
- <span data-ttu-id="0b982-123">将服务注入到使用它的类的构造函数中。</span><span class="sxs-lookup"><span data-stu-id="0b982-123">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="0b982-124">框架负责创建依赖关系的实例，并在不再需要时将其释放。</span><span class="sxs-lookup"><span data-stu-id="0b982-124">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="0b982-125">例如，`IMessageWriter` 接口定义 `Write` 方法：</span><span class="sxs-lookup"><span data-stu-id="0b982-125">As an example, the `IMessageWriter` interface defines the `Write` method:</span></span>

:::code language="csharp" source="snippets/configuration/dependency-injection/IMessageWriter.cs":::

<span data-ttu-id="0b982-126">此接口由具体类型 `MessageWriter` 实现：</span><span class="sxs-lookup"><span data-stu-id="0b982-126">This interface is implemented by a concrete type, `MessageWriter`:</span></span>

:::code language="csharp" source="snippets/configuration/dependency-injection/MessageWriter.cs":::

<span data-ttu-id="0b982-127">示例代码使用具体类型 `MessageWriter` 注册 `IMessageWriter` 服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-127">The sample code registers the `IMessageWriter` service with the concrete type `MessageWriter`.</span></span> <span data-ttu-id="0b982-128"><xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped%2A> 方法使用范围内生存期（单个请求的生存期）注册服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-128">The <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped%2A> method registers the service with a scoped lifetime, the lifetime of a single request.</span></span> <span data-ttu-id="0b982-129">本文后面将介绍[服务生存期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="0b982-129">[Service lifetimes](#service-lifetimes) are described later in this article.</span></span>

:::code language="csharp" source="snippets/configuration/dependency-injection/Program.cs" highlight="16":::

<span data-ttu-id="0b982-130">在示例应用中，请求 `IMessageWriter` 服务并用于调用 `Write` 方法：</span><span class="sxs-lookup"><span data-stu-id="0b982-130">In the sample app, the `IMessageWriter` service is requested and used to call the `Write` method:</span></span>

:::code language="csharp" source="snippets/configuration/dependency-injection/Worker.cs":::

<span data-ttu-id="0b982-131">通过使用 DI 模式，辅助角色服务：</span><span class="sxs-lookup"><span data-stu-id="0b982-131">By using the DI pattern, the worker service:</span></span>

- <span data-ttu-id="0b982-132">不使用具体类型 `MessageWriter`，只使用实现它的 `IMessageWriter` 接口。</span><span class="sxs-lookup"><span data-stu-id="0b982-132">Doesn't use the concrete type `MessageWriter`, only the `IMessageWriter` interface that implements it.</span></span> <span data-ttu-id="0b982-133">这样可以轻松地更改控制器使用的实现，而无需修改控制器。</span><span class="sxs-lookup"><span data-stu-id="0b982-133">That makes it easy to change the implementation that the controller uses without modifying the controller.</span></span>
- <span data-ttu-id="0b982-134">不创建 `MessageWriter` 的实例，这由 DI 容器创建。</span><span class="sxs-lookup"><span data-stu-id="0b982-134">Doesn't create an instance of `MessageWriter`, it's created by the DI container.</span></span>

<span data-ttu-id="0b982-135">可以通过使用内置日志记录 API 来改善 `IMessageWriter` 接口的实现：</span><span class="sxs-lookup"><span data-stu-id="0b982-135">The implementation of the `IMessageWriter` interface can be improved by using the built-in logging API:</span></span>

:::code language="csharp" source="snippets/configuration/dependency-injection/LoggingMessageWriter.cs":::

<span data-ttu-id="0b982-136">更新的 `ConfigureServices` 方法注册新的 `IMessageWriter` 实现：</span><span class="sxs-lookup"><span data-stu-id="0b982-136">The updated `ConfigureServices` method registers the new `IMessageWriter` implementation:</span></span>

```csharp
static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureServices((_, services) =>
            services.AddHostedService<Worker>()
                    .AddScoped<IMessageWriter, LoggingMessageWriter>());
```

<span data-ttu-id="0b982-137">`LoggingMessageWriter` 依赖于 <xref:Microsoft.Extensions.Logging.ILogger%601>，并在构造函数中对其进行请求。</span><span class="sxs-lookup"><span data-stu-id="0b982-137">`LoggingMessageWriter` depends on <xref:Microsoft.Extensions.Logging.ILogger%601>, which it requests in the constructor.</span></span> <span data-ttu-id="0b982-138">`ILogger<TCategoryName>` 是[框架提供的服务](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="0b982-138">`ILogger<TCategoryName>` is a [framework-provided service](#framework-provided-services).</span></span>

<span data-ttu-id="0b982-139">以链式方式使用依赖关系注入并不罕见。</span><span class="sxs-lookup"><span data-stu-id="0b982-139">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="0b982-140">每个请求的依赖关系相应地请求其自己的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="0b982-140">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="0b982-141">容器解析图中的依赖关系并返回完全解析的服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-141">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="0b982-142">必须被解析的依赖关系的集合通常被称为“依赖关系树”、“依赖关系图”或“对象图”。</span><span class="sxs-lookup"><span data-stu-id="0b982-142">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="0b982-143">容器通过利用[（泛型）开放类型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)解析 `ILogger<TCategoryName>`，而无需注册每个[（泛型）构造类型](/dotnet/csharp/language-reference/language-specification/types#constructed-types)。</span><span class="sxs-lookup"><span data-stu-id="0b982-143">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types).</span></span>

<span data-ttu-id="0b982-144">在依赖项注入术语中，服务：</span><span class="sxs-lookup"><span data-stu-id="0b982-144">In dependency injection terminology, a service:</span></span>

- <span data-ttu-id="0b982-145">通常是向其他对象提供服务的对象，如 `IMessageWriter` 服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-145">Is typically an object that provides a service to other objects, such as the `IMessageWriter` service.</span></span>
- <span data-ttu-id="0b982-146">与 Web 服务无关，尽管服务可能使用 Web 服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-146">Is not related to a web service, although the service may use a web service.</span></span>

<span data-ttu-id="0b982-147">框架提供可靠的日志记录系统。</span><span class="sxs-lookup"><span data-stu-id="0b982-147">The framework provides a robust logging system.</span></span> <span data-ttu-id="0b982-148">编写上述示例中的 `IMessageWriter` 实现来演示基本的 DI，而不是来实现日志记录。</span><span class="sxs-lookup"><span data-stu-id="0b982-148">The `IMessageWriter` implementations shown in the preceding examples were written to demonstrate basic DI, not to implement logging.</span></span> <span data-ttu-id="0b982-149">大多数应用都不需要编写记录器。</span><span class="sxs-lookup"><span data-stu-id="0b982-149">Most apps shouldn't need to write loggers.</span></span> <span data-ttu-id="0b982-150">下面的代码展示了如何使用默认日志记录，只需要将 `Worker` 在 `ConfigureServices` 中注册为托管服务 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionHostedServiceExtensions.AddHostedService%2A>：</span><span class="sxs-lookup"><span data-stu-id="0b982-150">The following code demonstrates using the default logging, which only requires the `Worker` to be registered in `ConfigureServices` as a hosted service <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionHostedServiceExtensions.AddHostedService%2A>:</span></span>

```csharp
public class Worker : BackgroundService
{
    private readonly ILogger<Worker> _logger;

    public Worker(ILogger<Worker> logger) =>
        _logger = logger;

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
            await Task.Delay(1000, stoppingToken);
        }
    }
}
```

<span data-ttu-id="0b982-151">使用前面的代码时，无需更新 `ConfigureServices`，因为框架提供日志记录。</span><span class="sxs-lookup"><span data-stu-id="0b982-151">Using the preceding code, there is no need to update `ConfigureServices`, because logging is provided by the framework.</span></span>

## <a name="register-groups-of-services-with-extension-methods"></a><span data-ttu-id="0b982-152">使用扩展方法注册服务组</span><span class="sxs-lookup"><span data-stu-id="0b982-152">Register groups of services with extension methods</span></span>

<span data-ttu-id="0b982-153">Microsoft 扩展使用一种约定来注册一组相关服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-153">Microsoft Extensions uses a convention for registering a group of related services.</span></span> <span data-ttu-id="0b982-154">约定使用单个 `Add{GROUP_NAME}` 扩展方法来注册该框架功能所需的所有服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-154">The convention is to use a single `Add{GROUP_NAME}` extension method to register all of the services required by a framework feature.</span></span> <span data-ttu-id="0b982-155">例如，<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%2A> 扩展方法会注册使用选项所需的所有服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-155">For example, the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%2A> extension method registers all of the services required for using options.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="0b982-156">框架提供的服务</span><span class="sxs-lookup"><span data-stu-id="0b982-156">Framework-provided services</span></span>

<span data-ttu-id="0b982-157">`ConfigureServices` 方法注册应用使用的服务，包括平台功能。</span><span class="sxs-lookup"><span data-stu-id="0b982-157">The `ConfigureServices` method registers services that the app uses, including platform features.</span></span> <span data-ttu-id="0b982-158">最初，提供给 `ConfigureServices` 的 `IServiceCollection` 具有框架定义的服务（具体取决于[主机配置方式](generic-host.md#host-configuration)）。</span><span class="sxs-lookup"><span data-stu-id="0b982-158">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](generic-host.md#host-configuration).</span></span> <span data-ttu-id="0b982-159">对于基于 .NET 模板的应用，该框架会注册数百个服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-159">For apps based on the .NET templates, the framework registers hundreds of services.</span></span>

<span data-ttu-id="0b982-160">下表列出了框架注册的这些服务的一小部分：</span><span class="sxs-lookup"><span data-stu-id="0b982-160">The following table lists a small sample of these framework-registered services:</span></span>

| <span data-ttu-id="0b982-161">服务类型</span><span class="sxs-lookup"><span data-stu-id="0b982-161">Service Type</span></span>                                                                       | <span data-ttu-id="0b982-162">生存期</span><span class="sxs-lookup"><span data-stu-id="0b982-162">Lifetime</span></span>  |
|------------------------------------------------------------------------------------|-----------|
| <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime>                       | <span data-ttu-id="0b982-163">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger%601?displayProperty=fullName>           | <span data-ttu-id="0b982-164">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-164">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName>        | <span data-ttu-id="0b982-165">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-165">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="0b982-166">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-166">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions%601?displayProperty=fullName> | <span data-ttu-id="0b982-167">暂时</span><span class="sxs-lookup"><span data-stu-id="0b982-167">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions%601?displayProperty=fullName>          | <span data-ttu-id="0b982-168">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-168">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName>              | <span data-ttu-id="0b982-169">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-169">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName>                | <span data-ttu-id="0b982-170">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-170">Singleton</span></span> |

## <a name="service-lifetimes"></a><span data-ttu-id="0b982-171">服务生存期</span><span class="sxs-lookup"><span data-stu-id="0b982-171">Service lifetimes</span></span>

<span data-ttu-id="0b982-172">可以使用以下任一生存期注册服务：</span><span class="sxs-lookup"><span data-stu-id="0b982-172">Services can be registered with one of the following lifetimes:</span></span>

- <span data-ttu-id="0b982-173">暂时</span><span class="sxs-lookup"><span data-stu-id="0b982-173">Transient</span></span>
- <span data-ttu-id="0b982-174">作用域</span><span class="sxs-lookup"><span data-stu-id="0b982-174">Scoped</span></span>
- <span data-ttu-id="0b982-175">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-175">Singleton</span></span>

<span data-ttu-id="0b982-176">下列各部分描述了上述每个生存期。</span><span class="sxs-lookup"><span data-stu-id="0b982-176">The following sections describe each of the preceding lifetimes.</span></span> <span data-ttu-id="0b982-177">为每个注册的服务选择适当的生存期。</span><span class="sxs-lookup"><span data-stu-id="0b982-177">Choose an appropriate lifetime for each registered service.</span></span>

### <a name="transient"></a><span data-ttu-id="0b982-178">暂时</span><span class="sxs-lookup"><span data-stu-id="0b982-178">Transient</span></span>

<span data-ttu-id="0b982-179">暂时生存期服务是每次从服务容器进行请求时创建的。</span><span class="sxs-lookup"><span data-stu-id="0b982-179">Transient lifetime services are created each time they're requested from the service container.</span></span> <span data-ttu-id="0b982-180">这种生存期适合轻量级、 无状态的服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-180">This lifetime works best for lightweight, stateless services.</span></span> <span data-ttu-id="0b982-181">向 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient%2A> 注册暂时性服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-181">Register transient services with <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient%2A>.</span></span>

<span data-ttu-id="0b982-182">在处理请求的应用中，在请求结束时会释放暂时服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-182">In apps that process requests, transient services are disposed at the end of the request.</span></span>

### <a name="scoped"></a><span data-ttu-id="0b982-183">范围内</span><span class="sxs-lookup"><span data-stu-id="0b982-183">Scoped</span></span>

<span data-ttu-id="0b982-184">对于 Web 应用，指定了作用域的生存期指明了每个客户端请求（连接）创建一次服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-184">For web applications, a scoped lifetime indicates that services are created once per client request (connection).</span></span> <span data-ttu-id="0b982-185">向 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped%2A> 注册范围内服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-185">Register scoped services with <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped%2A>.</span></span>

<span data-ttu-id="0b982-186">在处理请求的应用中，在请求结束时会释放有作用域的服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-186">In apps that process requests, scoped services are disposed at the end of the request.</span></span>

<span data-ttu-id="0b982-187">使用 Entity Framework Core 时，默认情况下 <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> 扩展方法使用范围内生存期来注册 `DbContext` 类型。</span><span class="sxs-lookup"><span data-stu-id="0b982-187">When using Entity Framework Core, the <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> extension method registers `DbContext` types with a scoped lifetime by default.</span></span>

> [!NOTE]
> <span data-ttu-id="0b982-188">不要从单一实例解析限定范围的服务，并小心不要间接地这样做，例如通过暂时性服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-188">Do ***not*** resolve a scoped service from a singleton and be careful not to do so indirectly, for example, through a transient service.</span></span> <span data-ttu-id="0b982-189">当处理后续请求时，它可能会导致服务处于不正确的状态。</span><span class="sxs-lookup"><span data-stu-id="0b982-189">It may cause the service to have incorrect state when processing subsequent requests.</span></span> <span data-ttu-id="0b982-190">可以：</span><span class="sxs-lookup"><span data-stu-id="0b982-190">It's fine to:</span></span>
>
> - <span data-ttu-id="0b982-191">从范围内或暂时性服务解析单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-191">Resolve a singleton service from a scoped or transient service.</span></span>
> - <span data-ttu-id="0b982-192">从其他范围内或暂时性服务解析范围内服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-192">Resolve a scoped service from another scoped or transient service.</span></span>

<span data-ttu-id="0b982-193">默认情况下在开发环境中，从具有较长生存期的其他服务解析服务将引发异常。</span><span class="sxs-lookup"><span data-stu-id="0b982-193">By default, in the development environment, resolving a service from another service with a longer lifetime throws an exception.</span></span> <span data-ttu-id="0b982-194">有关详细信息，请参阅[作用域验证](#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="0b982-194">For more information, see [Scope validation](#scope-validation).</span></span>

### <a name="singleton"></a><span data-ttu-id="0b982-195">单例</span><span class="sxs-lookup"><span data-stu-id="0b982-195">Singleton</span></span>

<span data-ttu-id="0b982-196">创建单例生命周期服务的情况如下：</span><span class="sxs-lookup"><span data-stu-id="0b982-196">Singleton lifetime services are created either:</span></span>

- <span data-ttu-id="0b982-197">在首次请求它们时进行创建；或者</span><span class="sxs-lookup"><span data-stu-id="0b982-197">The first time they're requested.</span></span>
- <span data-ttu-id="0b982-198">在向容器直接提供实现实例时由开发人员进行创建。</span><span class="sxs-lookup"><span data-stu-id="0b982-198">By the developer, when providing an implementation instance directly to the container.</span></span> <span data-ttu-id="0b982-199">很少用到此方法。</span><span class="sxs-lookup"><span data-stu-id="0b982-199">This approach is rarely needed.</span></span>

<span data-ttu-id="0b982-200">来自依赖关系注入容器的服务实现的每一个后续请求都使用同一个实例。</span><span class="sxs-lookup"><span data-stu-id="0b982-200">Every subsequent request of the service implementation from the dependency injection container uses the same instance.</span></span> <span data-ttu-id="0b982-201">如果应用需要单一实例行为，则允许服务容器管理服务的生存期。</span><span class="sxs-lookup"><span data-stu-id="0b982-201">If the app requires singleton behavior, allow the service container to manage the service's lifetime.</span></span> <span data-ttu-id="0b982-202">不要实现单一实例设计模式，或提供代码来释放单一实例。</span><span class="sxs-lookup"><span data-stu-id="0b982-202">Don't implement the singleton design pattern and provide code to dispose of the singleton.</span></span> <span data-ttu-id="0b982-203">服务永远不应由解析容器服务的代码释放。</span><span class="sxs-lookup"><span data-stu-id="0b982-203">Services should never be disposed by code that resolved the service from the container.</span></span> <span data-ttu-id="0b982-204">如果类型或工厂注册为单一实例，则容器自动释放单一实例。</span><span class="sxs-lookup"><span data-stu-id="0b982-204">If a type or factory is registered as a singleton, the container disposes the singleton automatically.</span></span>

<span data-ttu-id="0b982-205">向 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton%2A> 注册单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-205">Register singleton services with <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton%2A>.</span></span> <span data-ttu-id="0b982-206">单一实例服务必须是线程安全的，并且通常在无状态服务中使用。</span><span class="sxs-lookup"><span data-stu-id="0b982-206">Singleton services must be thread safe and are often used in stateless services.</span></span>

<span data-ttu-id="0b982-207">在处理请求的应用中，当应用关闭并释放 <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> 时，会释放单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-207">In apps that process requests, singleton services are disposed when the <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> is disposed on application shutdown.</span></span> <span data-ttu-id="0b982-208">由于应用关闭之前不释放内存，因此请考虑单一实例服务的内存使用。</span><span class="sxs-lookup"><span data-stu-id="0b982-208">Because memory is not released until the app is shut down, consider memory use with a singleton service.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="0b982-209">服务注册方法</span><span class="sxs-lookup"><span data-stu-id="0b982-209">Service registration methods</span></span>

<span data-ttu-id="0b982-210">框架提供了适用于特定场景的服务注册扩展方法：</span><span class="sxs-lookup"><span data-stu-id="0b982-210">The framework provides service registration extension methods that are useful in specific scenarios:</span></span>

| <span data-ttu-id="0b982-211">方法</span><span class="sxs-lookup"><span data-stu-id="0b982-211">Method</span></span> | <span data-ttu-id="0b982-212">自动</span><span class="sxs-lookup"><span data-stu-id="0b982-212">Automatic</span></span><br><span data-ttu-id="0b982-213">对象 (object)</span><span class="sxs-lookup"><span data-stu-id="0b982-213">object</span></span><br><span data-ttu-id="0b982-214">释放</span><span class="sxs-lookup"><span data-stu-id="0b982-214">disposal</span></span> | <span data-ttu-id="0b982-215">多种</span><span class="sxs-lookup"><span data-stu-id="0b982-215">Multiple</span></span><br><span data-ttu-id="0b982-216">实现</span><span class="sxs-lookup"><span data-stu-id="0b982-216">implementations</span></span> | <span data-ttu-id="0b982-217">传递参数</span><span class="sxs-lookup"><span data-stu-id="0b982-217">Pass args</span></span> |
|--|:-:|:-:|:-:|
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><br><span data-ttu-id="0b982-218">示例：</span><span class="sxs-lookup"><span data-stu-id="0b982-218">Example:</span></span><br><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="0b982-219">是</span><span class="sxs-lookup"><span data-stu-id="0b982-219">Yes</span></span> | <span data-ttu-id="0b982-220">是</span><span class="sxs-lookup"><span data-stu-id="0b982-220">Yes</span></span> | <span data-ttu-id="0b982-221">否</span><span class="sxs-lookup"><span data-stu-id="0b982-221">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><br><span data-ttu-id="0b982-222">示例：</span><span class="sxs-lookup"><span data-stu-id="0b982-222">Examples:</span></span><br><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep(99));` | <span data-ttu-id="0b982-223">是</span><span class="sxs-lookup"><span data-stu-id="0b982-223">Yes</span></span> | <span data-ttu-id="0b982-224">是</span><span class="sxs-lookup"><span data-stu-id="0b982-224">Yes</span></span> | <span data-ttu-id="0b982-225">是</span><span class="sxs-lookup"><span data-stu-id="0b982-225">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><br><span data-ttu-id="0b982-226">示例：</span><span class="sxs-lookup"><span data-stu-id="0b982-226">Example:</span></span><br><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="0b982-227">是</span><span class="sxs-lookup"><span data-stu-id="0b982-227">Yes</span></span> | <span data-ttu-id="0b982-228">否</span><span class="sxs-lookup"><span data-stu-id="0b982-228">No</span></span> | <span data-ttu-id="0b982-229">否</span><span class="sxs-lookup"><span data-stu-id="0b982-229">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><br><span data-ttu-id="0b982-230">示例：</span><span class="sxs-lookup"><span data-stu-id="0b982-230">Examples:</span></span><br><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep(99));` | <span data-ttu-id="0b982-231">否</span><span class="sxs-lookup"><span data-stu-id="0b982-231">No</span></span> | <span data-ttu-id="0b982-232">是</span><span class="sxs-lookup"><span data-stu-id="0b982-232">Yes</span></span> | <span data-ttu-id="0b982-233">是</span><span class="sxs-lookup"><span data-stu-id="0b982-233">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><br><span data-ttu-id="0b982-234">示例：</span><span class="sxs-lookup"><span data-stu-id="0b982-234">Examples:</span></span><br><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep(99));` | <span data-ttu-id="0b982-235">否</span><span class="sxs-lookup"><span data-stu-id="0b982-235">No</span></span> | <span data-ttu-id="0b982-236">否</span><span class="sxs-lookup"><span data-stu-id="0b982-236">No</span></span> | <span data-ttu-id="0b982-237">是</span><span class="sxs-lookup"><span data-stu-id="0b982-237">Yes</span></span> |

<span data-ttu-id="0b982-238">要详细了解释放类型，请参阅[服务释放](dependency-injection-guidelines.md#disposal-of-services)部分。</span><span class="sxs-lookup"><span data-stu-id="0b982-238">For more information on type disposal, see the [Disposal of services](dependency-injection-guidelines.md#disposal-of-services) section.</span></span>

<span data-ttu-id="0b982-239">仅使用实现类型注册服务等效于使用相同的实现和服务类型注册该服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-239">Registering a service with only an implementation type is equivalent to registering that service with the same implementation and service type.</span></span> <span data-ttu-id="0b982-240">因此，我们不能使用捕获显式服务类型的方法来注册服务的多个实现。</span><span class="sxs-lookup"><span data-stu-id="0b982-240">This is why multiple implementations of a service cannot be registered using the methods that don't take an explicit service type.</span></span> <span data-ttu-id="0b982-241">这些方法可以注册服务的多个实例，但它们都具有相同的实现类型 。</span><span class="sxs-lookup"><span data-stu-id="0b982-241">These methods can register multiple *instances* of a service, but they will all have the same *implementation* type.</span></span>

<span data-ttu-id="0b982-242">上述任何服务注册方法都可用于注册同一服务类型的多个服务实例。</span><span class="sxs-lookup"><span data-stu-id="0b982-242">Any of the above service registration methods can be used to register multiple service instances of the same service type.</span></span> <span data-ttu-id="0b982-243">下面的示例以 `IMessageWriter` 作为服务类型调用 `AddSingleton` 两次。</span><span class="sxs-lookup"><span data-stu-id="0b982-243">In the following example, `AddSingleton` is called twice with `IMessageWriter` as the service type.</span></span> <span data-ttu-id="0b982-244">第二次对 `AddSingleton` 的调用在解析为 `IMessageWriter` 时替代上一次调用，在通过 `IEnumerable<IMessageWriter>` 解析多个服务时添加到上一次调用。</span><span class="sxs-lookup"><span data-stu-id="0b982-244">The second call to `AddSingleton` overrides the previous one when resolved as `IMessageWriter` and adds to the previous one when multiple services are resolved via `IEnumerable<IMessageWriter>`.</span></span> <span data-ttu-id="0b982-245">通过 `IEnumerable<{SERVICE}>` 解析服务时，服务按其注册顺序显示。</span><span class="sxs-lookup"><span data-stu-id="0b982-245">Services appear in the order they were registered when resolved via `IEnumerable<{SERVICE}>`.</span></span>

:::code language="csharp" source="snippets/configuration/console-di-ienumerable/Program.cs" highlight="19-24":::

<span data-ttu-id="0b982-246">前面的示例源代码注册了 `IMessageWriter` 的两个实现。</span><span class="sxs-lookup"><span data-stu-id="0b982-246">The preceding sample source code registers two implementations of the `IMessageWriter`.</span></span>

:::code language="csharp" source="snippets/configuration/console-di-ienumerable/ExampleService.cs" highlight="9-18":::

<span data-ttu-id="0b982-247">`ExampleService` 定义两个构造函数参数：一个是 `IMessageWriter`，另一个是 `IEnumerable<IMessageWriter>`。</span><span class="sxs-lookup"><span data-stu-id="0b982-247">The `ExampleService` defines two constructor parameters; a single `IMessageWriter`, and an `IEnumerable<IMessageWriter>`.</span></span> <span data-ttu-id="0b982-248">第一个 `IMessageWriter` 是已注册的最后一个实现，而 `IEnumerable<IMessageWriter>` 表示所有已注册的实现。</span><span class="sxs-lookup"><span data-stu-id="0b982-248">The single `IMessageWriter` is the last implementation to have been registered, whereas the `IEnumerable<IMessageWriter>` represents all registered implementations.</span></span>

<span data-ttu-id="0b982-249">框架还提供 `TryAdd{LIFETIME}` 扩展方法，只有当尚未注册某个实现时，才注册该服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-249">The framework also provides `TryAdd{LIFETIME}` extension methods, which register the service only if there isn't already an implementation registered.</span></span>

<span data-ttu-id="0b982-250">在下面的示例中，对 `AddSingleton` 的调用会将 `ConsoleMessageWriter` 注册为 `IMessageWriter`的实现。</span><span class="sxs-lookup"><span data-stu-id="0b982-250">In the following example, the call to `AddSingleton` registers `ConsoleMessageWriter` as an implementation for `IMessageWriter`.</span></span> <span data-ttu-id="0b982-251">对 `TryAddSingleton` 的调用没有任何作用，因为 `IMessageWriter` 已有一个已注册的实现：</span><span class="sxs-lookup"><span data-stu-id="0b982-251">The call to `TryAddSingleton` has no effect because `IMessageWriter` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMessageWriter, ConsoleMessageWriter>();
services.TryAddSingleton<IMessageWriter, LoggingMessageWriter>();
```

<span data-ttu-id="0b982-252">`TryAddSingleton` 不起作用，因为已添加它并且“try”将失败。</span><span class="sxs-lookup"><span data-stu-id="0b982-252">The `TryAddSingleton` has no effect, as it was already added and the "try" will fail.</span></span> <span data-ttu-id="0b982-253">`ExampleService` 将断言以下内容：</span><span class="sxs-lookup"><span data-stu-id="0b982-253">The `ExampleService` would assert the following:</span></span>

```csharp
public class ExampleService
{
    public ExampleService(
        IMessageWriter messageWriter,
        IEnumerable<IMessageWriter> messageWriters)
    {
        Trace.Assert(messageWriter is ConsoleMessageWriter);
        Trace.Assert(messageWriters.Single() is ConsoleMessageWriter);
    }
}
```

<span data-ttu-id="0b982-254">有关详细信息，请参阅：</span><span class="sxs-lookup"><span data-stu-id="0b982-254">For more information, see:</span></span>

- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd%2A>
- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient%2A>
- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped%2A>
- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton%2A>

<span data-ttu-id="0b982-255">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable%2A) 方法仅会在没有同一类型实现的情况下才注册该服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-255">The [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable%2A) methods register the service only if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="0b982-256">多个服务通过 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="0b982-256">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="0b982-257">注册服务时，如果还没有添加相同类型的实例，就添加一个实例。</span><span class="sxs-lookup"><span data-stu-id="0b982-257">When registering services, add an instance if one of the same types hasn't already been added.</span></span> <span data-ttu-id="0b982-258">库作者使用 `TryAddEnumerable` 来避免在容器中注册一个实现的多个副本。</span><span class="sxs-lookup"><span data-stu-id="0b982-258">Library authors use `TryAddEnumerable` to avoid registering multiple copies of an implementation in the container.</span></span>

<span data-ttu-id="0b982-259">在下面的示例中，对 `TryAddEnumerable` 的第一次调用会将 `MessageWriter` 注册为 `IMessageWriter1`的实现。</span><span class="sxs-lookup"><span data-stu-id="0b982-259">In the following example, the first call to `TryAddEnumerable` registers `MessageWriter` as an implementation for `IMessageWriter1`.</span></span> <span data-ttu-id="0b982-260">第二次调用向 `IMessageWriter2` 注册 `MessageWriter`。</span><span class="sxs-lookup"><span data-stu-id="0b982-260">The second call registers `MessageWriter` for `IMessageWriter2`.</span></span> <span data-ttu-id="0b982-261">第三次调用没有任何作用，因为 `IMessageWriter1` 已有一个 `MessageWriter` 的已注册的实现：</span><span class="sxs-lookup"><span data-stu-id="0b982-261">The third call has no effect because `IMessageWriter1` already has a registered implementation of `MessageWriter`:</span></span>

```csharp
public interface IMessageWriter1 { }
public interface IMessageWriter2 { }

public class MessageWriter : IMessageWriter1, IMessageWriter2
{
}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMessageWriter1, MessageWriter>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMessageWriter2, MessageWriter>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMessageWriter1, MessageWriter>());
```

<span data-ttu-id="0b982-262">服务注册通常与顺序无关，除了注册同一类型的多个实现时。</span><span class="sxs-lookup"><span data-stu-id="0b982-262">Service registration is generally order independent except when registering multiple implementations of the same type.</span></span>

<span data-ttu-id="0b982-263">`IServiceCollection` 是 <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor> 对象的集合。</span><span class="sxs-lookup"><span data-stu-id="0b982-263">`IServiceCollection` is a collection of <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor> objects.</span></span> <span data-ttu-id="0b982-264">以下示例演示如何通过创建和添加 `ServiceDescriptor` 来注册服务：</span><span class="sxs-lookup"><span data-stu-id="0b982-264">The following example shows how to register a service by creating and adding a `ServiceDescriptor`:</span></span>

```csharp
string secretKey = Configuration["SecretKey"];
var descriptor = new ServiceDescriptor(
    typeof(IMessageWriter),
    _ => new DefaultMessageWriter(secretKey),
    ServiceLifetime.Transient);

services.Add(descriptor);
```

<span data-ttu-id="0b982-265">内置 `Add{LIFETIME}` 方法使用同一种方式。</span><span class="sxs-lookup"><span data-stu-id="0b982-265">The built-in `Add{LIFETIME}` methods use the same approach.</span></span> <span data-ttu-id="0b982-266">相关示例请参阅 [AddScoped 源代码](https://github.com/dotnet/extensions/blob/v3.1.8/src/DependencyInjection/DI.Abstractions/src/ServiceCollectionServiceExtensions.cs#L216-L237)。</span><span class="sxs-lookup"><span data-stu-id="0b982-266">For example, see the [AddScoped source code](https://github.com/dotnet/extensions/blob/v3.1.8/src/DependencyInjection/DI.Abstractions/src/ServiceCollectionServiceExtensions.cs#L216-L237).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="0b982-267">构造函数注入行为</span><span class="sxs-lookup"><span data-stu-id="0b982-267">Constructor injection behavior</span></span>

<span data-ttu-id="0b982-268">服务可使用以下方式来解析：</span><span class="sxs-lookup"><span data-stu-id="0b982-268">Services can be resolved by using:</span></span>

- <xref:System.IServiceProvider>
- <span data-ttu-id="0b982-269"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>:</span><span class="sxs-lookup"><span data-stu-id="0b982-269"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>:</span></span>
  - <span data-ttu-id="0b982-270">创建未在容器中注册的对象。</span><span class="sxs-lookup"><span data-stu-id="0b982-270">Creates objects that aren't registered in the container.</span></span>
  - <span data-ttu-id="0b982-271">用于某些框架功能。</span><span class="sxs-lookup"><span data-stu-id="0b982-271">Used with some framework features.</span></span>

<span data-ttu-id="0b982-272">构造函数可以接受非依赖关系注入提供的参数，但参数必须分配默认值。</span><span class="sxs-lookup"><span data-stu-id="0b982-272">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="0b982-273">当服务由 `IServiceProvider` 或 `ActivatorUtilities` 解析时，*构造函数注入* 需要 public 构造函数。</span><span class="sxs-lookup"><span data-stu-id="0b982-273">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="0b982-274">当服务由 `ActivatorUtilities` 解析时，构造函数注入要求只存在一个适用的构造函数。</span><span class="sxs-lookup"><span data-stu-id="0b982-274">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="0b982-275">支持构造函数重载，但其参数可以全部通过依赖注入来实现的重载只能存在一个。</span><span class="sxs-lookup"><span data-stu-id="0b982-275">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="scope-validation"></a><span data-ttu-id="0b982-276">作用域验证</span><span class="sxs-lookup"><span data-stu-id="0b982-276">Scope validation</span></span>

<span data-ttu-id="0b982-277">如果应用在 `Development` 环境中运行，并调用 [CreateDefaultBuilder](generic-host.md#default-builder-settings) 以生成主机，默认服务提供程序会执行检查，以确认以下内容：</span><span class="sxs-lookup"><span data-stu-id="0b982-277">When the app runs in the `Development` environment and calls [CreateDefaultBuilder](generic-host.md#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

- <span data-ttu-id="0b982-278">没有从根服务提供程序解析到范围内服务。</span><span class="sxs-lookup"><span data-stu-id="0b982-278">Scoped services aren't resolved from the root service provider.</span></span>
- <span data-ttu-id="0b982-279">未将范围内服务注入单一实例。</span><span class="sxs-lookup"><span data-stu-id="0b982-279">Scoped services aren't injected into singletons.</span></span>

<span data-ttu-id="0b982-280">调用 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> 时创建根服务提供程序。</span><span class="sxs-lookup"><span data-stu-id="0b982-280">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> is called.</span></span> <span data-ttu-id="0b982-281">在启动提供程序和应用时，根服务提供程序的生存期对应于应用的生存期，并在关闭应用时释放。</span><span class="sxs-lookup"><span data-stu-id="0b982-281">The root service provider's lifetime corresponds to the app's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="0b982-282">有作用域的服务由创建它们的容器释放。</span><span class="sxs-lookup"><span data-stu-id="0b982-282">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="0b982-283">如果范围内服务创建于根容器，则该服务的生存期实际上提升至单一实例，因为根容器只会在应用关闭时将其释放。</span><span class="sxs-lookup"><span data-stu-id="0b982-283">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when the app shuts down.</span></span> <span data-ttu-id="0b982-284">验证服务作用域，将在调用 `BuildServiceProvider` 时收集这类情况。</span><span class="sxs-lookup"><span data-stu-id="0b982-284">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

## <a name="see-also"></a><span data-ttu-id="0b982-285">另请参阅</span><span class="sxs-lookup"><span data-stu-id="0b982-285">See also</span></span>

- [<span data-ttu-id="0b982-286">在 .NET 中使用依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="0b982-286">Use dependency injection in .NET</span></span>](dependency-injection-usage.md)
- [<span data-ttu-id="0b982-287">依赖关系注入指南</span><span class="sxs-lookup"><span data-stu-id="0b982-287">Dependency injection guidelines</span></span>](dependency-injection-guidelines.md)
- [<span data-ttu-id="0b982-288">ASP.NET Core 中的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="0b982-288">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
- [<span data-ttu-id="0b982-289">用于 DI 应用开发的 NDC 会议模式</span><span class="sxs-lookup"><span data-stu-id="0b982-289">NDC Conference Patterns for DI app development</span></span>](https://www.youtube.com/watch?v=x-C-CNBVTaY)
- [<span data-ttu-id="0b982-290">显式依赖关系原则</span><span class="sxs-lookup"><span data-stu-id="0b982-290">Explicit dependencies principle</span></span>](../../architecture/modern-web-apps-azure/architectural-principles.md#explicit-dependencies)
- [<span data-ttu-id="0b982-291">控制反转容器和依赖关系注入模式 (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="0b982-291">Inversion of control containers and the dependency injection pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
- <span data-ttu-id="0b982-292">应在 [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) 存储库中创建 DI bug</span><span class="sxs-lookup"><span data-stu-id="0b982-292">DI bugs should be created in the [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) repo</span></span>
