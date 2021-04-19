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
# <a name="tutorial-use-dependency-injection-in-net"></a>教程：在 .NET 中使用依赖注入

本教程介绍如何[在 .NET 中使用依赖注入 (DI)](dependency-injection.md)。 使用 Microsoft 扩展时，DI 是“一等公民”，其中服务是在 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 中添加和配置的。 <xref:Microsoft.Extensions.Hosting.IHost> 接口会公开 <xref:System.IServiceProvider> 实例，它充当所有已注册的服务的容器。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
>
> - 创建一个使用依赖注入的 .NET 控制台应用
> - 生成和配置[通用主机](generic-host.md)
> - 编写多个接口及相应的实现
> - 为 DI 使用服务生存期和范围设定

## <a name="prerequisites"></a>先决条件

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本。
- 熟悉如何创建新的 .NET 应用程序以及如何安装 NuGet 包。

## <a name="create-a-new-console-application"></a>创建新的控制台应用程序

通过 [dotnet new](../tools/dotnet-new.md) 命令或 IDE 的“新建项目”向导，新建一个名为 ConsoleDI 的 .NET 控制台应用程序 **Example** 。 将 NuGet 包 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 添加到项目。

## <a name="add-interfaces"></a>添加接口

将以下接口添加到项目根目录：

IOperation.cs

:::code language="csharp" source="snippets/configuration/console-di/IOperation.cs":::

`IOperation` 接口会定义一个 `OperationId` 属性。

IOperation.cs *Transient*

:::code language="csharp" source="snippets/configuration/console-di/ITransientOperation.cs":::

IOperation.cs *Scoped*

:::code language="csharp" source="snippets/configuration/console-di/IScopedOperation.cs":::

IOperation.cs *Singleton*

:::code language="csharp" source="snippets/configuration/console-di/ISingletonOperation.cs":::

`IOperation` 的所有子接口会会命名其预期服务生存期。 例如，“Transient”或“Singleton”。

## <a name="add-default-implementation"></a>添加默认实现

添加以下默认实现来进行各种操作：

DefaultOperation.cs

:::code language="csharp" source="snippets/configuration/console-di/DefaultOperation.cs":::

`DefaultOperation` 会实现所有已命名的标记接口，并将 `OperationId` 属性初始化为新的全局唯一标识符 (GUID) 的最后 4 个字符。

## <a name="add-service-that-requires-di"></a>添加需要 DI 的服务

添加以下操作记录器对象，它作为服务添加到控制台应用：

OperationLogger.cs

:::code language="csharp" source="snippets/configuration/console-di/OperationLogger.cs":::

`OperationLogger` 会定义一个构造函数，该函数需要上述每一个标记接口（即 `ITransientOperation`、`IScopedOperation` 和 `ISingletonOperation`）。 对象会公开一个方法，使用者可通过该方法使用给定的 `scope` 参数记录操作。 被调用时，`LogOperations` 方法会使用范围字符串和消息记录每个操作的唯一标识符。

## <a name="register-services-for-di"></a>为 DI 注册服务

使用以下代码更新 Program.cs：

:::code language="csharp" source="snippets/configuration/console-di/Program.cs" range="1-18,35-60" highlight="22-26":::

每个 `services.Add{SERVICE_NAME}` 扩展方法添加（并可能配置）服务。 我们建议应用遵循此约定。 将扩展方法置于 <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> 命名空间中以封装服务注册的组。 还包括用于 DI 扩展方法的命名空间部分 `Microsoft.Extensions.DependencyInjection`：

- 允许在不添加其他 `using` 块的情况下在 [IntelliSense](/visualstudio/ide/using-intellisense) 中显示它们。
- 在通常会调用这些扩展方法的 `Program` 或 `Startup` 类中，避免出现过多的 `using` 语句。

应用会执行以下操作：

- 使用[默认活页夹设置](generic-host.md#default-builder-settings)创建一个 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 实例。
- 配置服务并对其添加相应的服务生存期。
- 调用 <xref:Microsoft.Extensions.Hosting.IHostBuilder.Build> 并分配 <xref:Microsoft.Extensions.Hosting.IHost> 的实例。
- 调用 `ExemplifyScoping`，传入 <xref:Microsoft.Extensions.Hosting.IHost.Services?displayProperty=nameWithType>。

## <a name="conclusion"></a>结束语

应用会显示如下例所示的输出：

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

在应用输出中，可看到：

- Transient 操作总是不同，每次检索服务时，都会创建一个新实例。
- Scoped 仅随着新范围更改，但在一个范围中是相同的实例。
- Singleton 操作总是相同，新实例仅被创建一次。

## <a name="see-also"></a>另请参阅

* [依赖关系注入指南](dependency-injection-guidelines.md)
* [ASP.NET Core 中的依赖注入](/aspnet/core/fundamentals/dependency-injection)
