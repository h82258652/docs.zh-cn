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
# <a name="dependency-injection-in-net"></a>.NET 中的依赖关系注入

.NET 支持依赖关系注入 (DI) 软件设计模式，这是一种在类及其依赖项之间实现[控制反转 (IoC)](../../architecture/modern-web-apps-azure/architectural-principles.md#dependency-inversion) 的技术。 .NET 中的依赖关系注入是“[一等公民](https://en.wikipedia.org/wiki/First-class_citizen)”，提供配置、日志记录和选项模式。

依赖项是指另一个对象所依赖的对象。 使用其他类所依赖的 `Write` 方法检查以下 `MessageWriter` 类：

```csharp
public class MessageWriter
{
    public void Write(string message)
    {
        Console.WriteLine($"MessageWriter.Write(message: \"{message}\")");
    }
}
```

类可以创建 `MessageWriter` 类的实例，以便利用其 `Write` 方法。 在以下示例中，`MessageWriter` 类是 `Worker` 类的依赖项：

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

该类创建并直接依赖于 `MessageWriter` 类。 硬编码的依赖项（如前面的示例）会产生问题，应避免使用，原因如下：

- 要用不同的实现替换 `MessageWriter`，必须修改 `Worker` 类。
- 如果 `MessageWriter` 具有依赖项，则必须由 `Worker` 类对其进行配置。 在具有多个依赖于 `MessageWriter` 的类的大型项目中，配置代码将分散在整个应用中。
- 这种实现很难进行单元测试。 应用需使用模拟或存根 `MessageWriter` 类，而该类不能使用此方法。

依赖关系注入通过以下方式解决了这些问题：

- 使用接口或基类将依赖关系实现抽象化。
- 在服务容器中注册依赖关系。 .NET 提供了一个内置的服务容器 <xref:System.IServiceProvider>。 服务通常在应用启动时注册，并追加到 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>。 添加所有服务后，可以使用 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> 创建服务容器。
- 将服务注入到使用它的类的构造函数中。 框架负责创建依赖关系的实例，并在不再需要时将其释放。

例如，`IMessageWriter` 接口定义 `Write` 方法：

:::code language="csharp" source="snippets/configuration/dependency-injection/IMessageWriter.cs":::

此接口由具体类型 `MessageWriter` 实现：

:::code language="csharp" source="snippets/configuration/dependency-injection/MessageWriter.cs":::

示例代码使用具体类型 `MessageWriter` 注册 `IMessageWriter` 服务。 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped%2A> 方法使用范围内生存期（单个请求的生存期）注册服务。 本文后面将介绍[服务生存期](#service-lifetimes)。

:::code language="csharp" source="snippets/configuration/dependency-injection/Program.cs" highlight="16":::

在示例应用中，请求 `IMessageWriter` 服务并用于调用 `Write` 方法：

:::code language="csharp" source="snippets/configuration/dependency-injection/Worker.cs":::

通过使用 DI 模式，辅助角色服务：

- 不使用具体类型 `MessageWriter`，只使用实现它的 `IMessageWriter` 接口。 这样可以轻松地更改控制器使用的实现，而无需修改控制器。
- 不创建 `MessageWriter` 的实例，这由 DI 容器创建。

可以通过使用内置日志记录 API 来改善 `IMessageWriter` 接口的实现：

:::code language="csharp" source="snippets/configuration/dependency-injection/LoggingMessageWriter.cs":::

更新的 `ConfigureServices` 方法注册新的 `IMessageWriter` 实现：

```csharp
static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureServices((_, services) =>
            services.AddHostedService<Worker>()
                    .AddScoped<IMessageWriter, LoggingMessageWriter>());
```

`LoggingMessageWriter` 依赖于 <xref:Microsoft.Extensions.Logging.ILogger%601>，并在构造函数中对其进行请求。 `ILogger<TCategoryName>` 是[框架提供的服务](#framework-provided-services)。

以链式方式使用依赖关系注入并不罕见。 每个请求的依赖关系相应地请求其自己的依赖关系。 容器解析图中的依赖关系并返回完全解析的服务。 必须被解析的依赖关系的集合通常被称为“依赖关系树”、“依赖关系图”或“对象图”。

容器通过利用[（泛型）开放类型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)解析 `ILogger<TCategoryName>`，而无需注册每个[（泛型）构造类型](/dotnet/csharp/language-reference/language-specification/types#constructed-types)。

在依赖项注入术语中，服务：

- 通常是向其他对象提供服务的对象，如 `IMessageWriter` 服务。
- 与 Web 服务无关，尽管服务可能使用 Web 服务。

框架提供可靠的日志记录系统。 编写上述示例中的 `IMessageWriter` 实现来演示基本的 DI，而不是来实现日志记录。 大多数应用都不需要编写记录器。 下面的代码展示了如何使用默认日志记录，只需要将 `Worker` 在 `ConfigureServices` 中注册为托管服务 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionHostedServiceExtensions.AddHostedService%2A>：

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

使用前面的代码时，无需更新 `ConfigureServices`，因为框架提供日志记录。

## <a name="register-groups-of-services-with-extension-methods"></a>使用扩展方法注册服务组

Microsoft 扩展使用一种约定来注册一组相关服务。 约定使用单个 `Add{GROUP_NAME}` 扩展方法来注册该框架功能所需的所有服务。 例如，<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%2A> 扩展方法会注册使用选项所需的所有服务。

## <a name="framework-provided-services"></a>框架提供的服务

`ConfigureServices` 方法注册应用使用的服务，包括平台功能。 最初，提供给 `ConfigureServices` 的 `IServiceCollection` 具有框架定义的服务（具体取决于[主机配置方式](generic-host.md#host-configuration)）。 对于基于 .NET 模板的应用，该框架会注册数百个服务。

下表列出了框架注册的这些服务的一小部分：

| 服务类型                                                                       | 生存期  |
|------------------------------------------------------------------------------------|-----------|
| <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime>                       | 单例 |
| <xref:Microsoft.Extensions.Logging.ILogger%601?displayProperty=fullName>           | 单例 |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName>        | 单例 |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | 单例 |
| <xref:Microsoft.Extensions.Options.IConfigureOptions%601?displayProperty=fullName> | 暂时 |
| <xref:Microsoft.Extensions.Options.IOptions%601?displayProperty=fullName>          | 单例 |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName>              | 单例 |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName>                | 单例 |

## <a name="service-lifetimes"></a>服务生存期

可以使用以下任一生存期注册服务：

- 暂时
- 作用域
- 单例

下列各部分描述了上述每个生存期。 为每个注册的服务选择适当的生存期。

### <a name="transient"></a>暂时

暂时生存期服务是每次从服务容器进行请求时创建的。 这种生存期适合轻量级、 无状态的服务。 向 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient%2A> 注册暂时性服务。

在处理请求的应用中，在请求结束时会释放暂时服务。

### <a name="scoped"></a>范围内

对于 Web 应用，指定了作用域的生存期指明了每个客户端请求（连接）创建一次服务。 向 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped%2A> 注册范围内服务。

在处理请求的应用中，在请求结束时会释放有作用域的服务。

使用 Entity Framework Core 时，默认情况下 <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> 扩展方法使用范围内生存期来注册 `DbContext` 类型。

> [!NOTE]
> 不要从单一实例解析限定范围的服务，并小心不要间接地这样做，例如通过暂时性服务。 当处理后续请求时，它可能会导致服务处于不正确的状态。 可以：
>
> - 从范围内或暂时性服务解析单一实例服务。
> - 从其他范围内或暂时性服务解析范围内服务。

默认情况下在开发环境中，从具有较长生存期的其他服务解析服务将引发异常。 有关详细信息，请参阅[作用域验证](#scope-validation)。

### <a name="singleton"></a>单例

创建单例生命周期服务的情况如下：

- 在首次请求它们时进行创建；或者
- 在向容器直接提供实现实例时由开发人员进行创建。 很少用到此方法。

来自依赖关系注入容器的服务实现的每一个后续请求都使用同一个实例。 如果应用需要单一实例行为，则允许服务容器管理服务的生存期。 不要实现单一实例设计模式，或提供代码来释放单一实例。 服务永远不应由解析容器服务的代码释放。 如果类型或工厂注册为单一实例，则容器自动释放单一实例。

向 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton%2A> 注册单一实例服务。 单一实例服务必须是线程安全的，并且通常在无状态服务中使用。

在处理请求的应用中，当应用关闭并释放 <xref:Microsoft.Extensions.DependencyInjection.ServiceProvider> 时，会释放单一实例服务。 由于应用关闭之前不释放内存，因此请考虑单一实例服务的内存使用。

## <a name="service-registration-methods"></a>服务注册方法

框架提供了适用于特定场景的服务注册扩展方法：

| 方法 | 自动<br>对象 (object)<br>释放 | 多种<br>实现 | 传递参数 |
|--|:-:|:-:|:-:|
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><br>示例：<br><br>`services.AddSingleton<IMyDep, MyDep>();` | 是 | 是 | 否 |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><br>示例：<br><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep(99));` | 是 | 是 | 是 |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><br>示例：<br><br>`services.AddSingleton<MyDep>();` | 是 | 否 | 否 |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><br>示例：<br><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep(99));` | 否 | 是 | 是 |
| `AddSingleton(new {IMPLEMENTATION})`<br><br>示例：<br><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep(99));` | 否 | 否 | 是 |

要详细了解释放类型，请参阅[服务释放](dependency-injection-guidelines.md#disposal-of-services)部分。

仅使用实现类型注册服务等效于使用相同的实现和服务类型注册该服务。 因此，我们不能使用捕获显式服务类型的方法来注册服务的多个实现。 这些方法可以注册服务的多个实例，但它们都具有相同的实现类型 。

上述任何服务注册方法都可用于注册同一服务类型的多个服务实例。 下面的示例以 `IMessageWriter` 作为服务类型调用 `AddSingleton` 两次。 第二次对 `AddSingleton` 的调用在解析为 `IMessageWriter` 时替代上一次调用，在通过 `IEnumerable<IMessageWriter>` 解析多个服务时添加到上一次调用。 通过 `IEnumerable<{SERVICE}>` 解析服务时，服务按其注册顺序显示。

:::code language="csharp" source="snippets/configuration/console-di-ienumerable/Program.cs" highlight="19-24":::

前面的示例源代码注册了 `IMessageWriter` 的两个实现。

:::code language="csharp" source="snippets/configuration/console-di-ienumerable/ExampleService.cs" highlight="9-18":::

`ExampleService` 定义两个构造函数参数：一个是 `IMessageWriter`，另一个是 `IEnumerable<IMessageWriter>`。 第一个 `IMessageWriter` 是已注册的最后一个实现，而 `IEnumerable<IMessageWriter>` 表示所有已注册的实现。

框架还提供 `TryAdd{LIFETIME}` 扩展方法，只有当尚未注册某个实现时，才注册该服务。

在下面的示例中，对 `AddSingleton` 的调用会将 `ConsoleMessageWriter` 注册为 `IMessageWriter`的实现。 对 `TryAddSingleton` 的调用没有任何作用，因为 `IMessageWriter` 已有一个已注册的实现：

```csharp
services.AddSingleton<IMessageWriter, ConsoleMessageWriter>();
services.TryAddSingleton<IMessageWriter, LoggingMessageWriter>();
```

`TryAddSingleton` 不起作用，因为已添加它并且“try”将失败。 `ExampleService` 将断言以下内容：

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

有关详细信息，请参阅：

- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd%2A>
- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient%2A>
- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped%2A>
- <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton%2A>

[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable%2A) 方法仅会在没有同一类型实现的情况下才注册该服务。 多个服务通过 `IEnumerable<{SERVICE}>` 解析。 注册服务时，如果还没有添加相同类型的实例，就添加一个实例。 库作者使用 `TryAddEnumerable` 来避免在容器中注册一个实现的多个副本。

在下面的示例中，对 `TryAddEnumerable` 的第一次调用会将 `MessageWriter` 注册为 `IMessageWriter1`的实现。 第二次调用向 `IMessageWriter2` 注册 `MessageWriter`。 第三次调用没有任何作用，因为 `IMessageWriter1` 已有一个 `MessageWriter` 的已注册的实现：

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

服务注册通常与顺序无关，除了注册同一类型的多个实现时。

`IServiceCollection` 是 <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor> 对象的集合。 以下示例演示如何通过创建和添加 `ServiceDescriptor` 来注册服务：

```csharp
string secretKey = Configuration["SecretKey"];
var descriptor = new ServiceDescriptor(
    typeof(IMessageWriter),
    _ => new DefaultMessageWriter(secretKey),
    ServiceLifetime.Transient);

services.Add(descriptor);
```

内置 `Add{LIFETIME}` 方法使用同一种方式。 相关示例请参阅 [AddScoped 源代码](https://github.com/dotnet/extensions/blob/v3.1.8/src/DependencyInjection/DI.Abstractions/src/ServiceCollectionServiceExtensions.cs#L216-L237)。

### <a name="constructor-injection-behavior"></a>构造函数注入行为

服务可使用以下方式来解析：

- <xref:System.IServiceProvider>
- <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>:
  - 创建未在容器中注册的对象。
  - 用于某些框架功能。

构造函数可以接受非依赖关系注入提供的参数，但参数必须分配默认值。

当服务由 `IServiceProvider` 或 `ActivatorUtilities` 解析时，*构造函数注入* 需要 public 构造函数。

当服务由 `ActivatorUtilities` 解析时，构造函数注入要求只存在一个适用的构造函数。 支持构造函数重载，但其参数可以全部通过依赖注入来实现的重载只能存在一个。

## <a name="scope-validation"></a>作用域验证

如果应用在 `Development` 环境中运行，并调用 [CreateDefaultBuilder](generic-host.md#default-builder-settings) 以生成主机，默认服务提供程序会执行检查，以确认以下内容：

- 没有从根服务提供程序解析到范围内服务。
- 未将范围内服务注入单一实例。

调用 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> 时创建根服务提供程序。 在启动提供程序和应用时，根服务提供程序的生存期对应于应用的生存期，并在关闭应用时释放。

有作用域的服务由创建它们的容器释放。 如果范围内服务创建于根容器，则该服务的生存期实际上提升至单一实例，因为根容器只会在应用关闭时将其释放。 验证服务作用域，将在调用 `BuildServiceProvider` 时收集这类情况。

## <a name="see-also"></a>另请参阅

- [在 .NET 中使用依赖关系注入](dependency-injection-usage.md)
- [依赖关系注入指南](dependency-injection-guidelines.md)
- [ASP.NET Core 中的依赖关系注入](/aspnet/core/fundamentals/dependency-injection)
- [用于 DI 应用开发的 NDC 会议模式](https://www.youtube.com/watch?v=x-C-CNBVTaY)
- [显式依赖关系原则](../../architecture/modern-web-apps-azure/architectural-principles.md#explicit-dependencies)
- [控制反转容器和依赖关系注入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)
- 应在 [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) 存储库中创建 DI bug
