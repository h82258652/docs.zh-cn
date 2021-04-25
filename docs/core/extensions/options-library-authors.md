---
title: 面向 .NET 库创建者的选项模式指南
author: IEvangelist
description: 了解如何在 .NET 中以库创建者的身份公开选项模式。
ms.author: dapine
ms.date: 04/12/2021
ms.openlocfilehash: 7e1bfeadff92f5d0d979ef2d7da11d7c1b47c58d
ms.sourcegitcommit: bbc724b72fb6c978905ac715e4033efa291f84dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107369616"
---
# <a name="options-pattern-guidance-for-net-library-authors"></a>面向 .NET 库创建者的选项模式指南

借助依赖关系注入，在注册服务及其相应的配置时，可以使用选项模式。 通过选项模式，库（和服务）的使用者可以要求提供[选项接口](options.md#options-interfaces)的实例，其中 `TOptions` 是你的选项类。 通过强类型对象使用配置选项有助于确保值表示方法的一致性，让你无需再费力手动分析字符串值。 有许多[配置提供程序](configuration-providers.md)可供库使用者使用。 借助这些提供程序，使用者可以通过多种方式配置库。

作为 .NET 库的创建者，你将了解有关如何正确向库的使用者公开选项模式的一般指南。 有很多种方法都会达到同样的效果，并有几个注意事项。

## <a name="naming-conventions"></a>命名约定

按照约定，负责注册服务的扩展方法被命名为 `Add{Service}`，其中 `{Service}` 是一个有意义的描述性名称。 服务注册可能附带 `Use{Service}` 扩展方法，具体取决于包。 `Use{Service}` 扩展方法在 [ASP.NET Core](/aspnet) 中是通用的。

✔️ 考虑使用将你的服务与其他产品/服务区分开来的名称。

❌ 不要使用已是 Microsoft 官方包中 .NET 生态系统的一部分的名称。

✔️ 考虑将公开扩展方法的静态类命名为 `{Type}Extensions`，其中 `{Type}` 是你要扩展的类型。

### <a name="namespace-guidance"></a>命名空间指南

Microsoft 包利用 `Microsoft.Extensions.DependencyInjection` 命名空间来统一各种服务产品/服务的注册。

✔️ 考虑使用清楚标识包产品/服务的命名空间。

❌ 不要将 `Microsoft.Extensions.DependencyInjection` 命名空间用于非官方的 Microsoft 包。

## <a name="parameterless"></a>无参数

如果你的服务可以用最少的显式配置或不需要显式配置来工作，请考虑使用无参数扩展方法。

:::code language="csharp" source="snippets/configuration/options-noparams/ServiceCollectionExtensions.cs" highlight="10-14":::

在上述代码中，`AddMyLibraryService` 执行以下操作：

- 扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例
- 调用类型参数为 `LibraryOptions` 的 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection)?displayProperty=nameWithType>
- 链接对 <xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A> 的调用，用来指定默认选项值

## <a name="iconfiguration-parameter"></a>`IConfiguration` 参数

在创建向使用者公开许多选项的库时，可能需要考虑要求使用 `IConfiguration` 参数扩展方法。 应使用 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A?displayProperty=nameWithType> 函数，将预期的 `IConfiguration` 实例的作用域限定为此配置的已命名部分。

:::code language="csharp" source="snippets/configuration/options-configparam/ServiceCollectionExtensions.cs" highlight="10,12-14":::

在上述代码中，`AddMyLibraryService` 执行以下操作：

- 扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例
- 定义 <xref:Microsoft.Extensions.Configuration.IConfiguration> 参数 `namedConfigurationSection`
- 调用要传递 `LibraryOptions` 的泛型类型参数和 `namedConfigurationSection` 实例的 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection,Microsoft.Extensions.Configuration.IConfiguration)> 以进行配置

此模式中的使用者提供已命名部分已限定作用域的 `IConfiguration` 实例：

:::code language="csharp" source="snippets/configuration/options-configparam/Program.cs" highlight="22-23":::

对 `.AddMyLibraryService` 的调用是在 <xref:Microsoft.Extensions.Hosting.IHostBuilder.ConfigureServices%2A> 方法中进行的。 同样地，使用 `Startup` 类时，会在 `ConfigureServices` 中添加注册的服务。

作为库创建者，由你指定默认值。

> [!NOTE]
> 可以将配置绑定到选项实例。 但是，存在名称冲突的风险，冲突将导致错误。 此外，当以这种方式手动绑定时，将选项模式的使用限制为读取一次。 对设置的更改将不会被重新绑定，因为这样的话，使用者就无法使用 [IOptionsMonitor](options.md#ioptionsmonitor) 接口。
>
> ```csharp
> services.AddOptions<LibraryOptions>()
>     .Configure<IConfiguration>(
>         (options, configuration) =>
>             configuration.GetSection("LibraryOptions").Bind(options));
> ```

## <a name="actiontoptions-parameter"></a>`Action<TOptions>` 参数

库的使用者可能有兴趣提供一个 Lambda 表达式来生成选项类的实例。 在此场景中，你在你的扩展方法中定义了一个 `Action<LibraryOptions>` 参数。

:::code language="csharp" source="snippets/configuration/options-action/ServiceCollectionExtensions.cs" highlight="10,12":::

在上述代码中，`AddMyLibraryService` 执行以下操作：

- 扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例
- 定义一个 <xref:System.Action%601>，其中 `T` 为 `LibraryOptions` 参数 `configureOptions`
- 根据 `configureOptions` 操作调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure%2A>

此模式下的使用者提供一个 Lambda 表达式（或一个符合 `Action<LibraryOptions>` 参数的委托）：

:::code language="csharp" source="snippets/configuration/options-action/Program.cs" highlight="22-26":::

## <a name="options-instance-parameter"></a>选项实例参数

库的使用者可能更倾向于提供一个内联的选项实例。 在此场景中，你公开一个扩展方法，此方法采用选项对象的实例 `LibraryOptions`。

:::code language="csharp" source="snippets/configuration/options-object/ServiceCollectionExtensions.cs" highlight="9,11-17":::

在上述代码中，`AddMyLibraryService` 执行以下操作：

- 扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例
- 调用类型参数为 `LibraryOptions` 的 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection)?displayProperty=nameWithType>
- 链接对 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure%2A> 的调用，指定可从给定 `userOptions` 实例中替代的默认选项值

此模式下的使用者提供 `LibraryOptions` 类的实例，以内联方式定义所需的属性值：

:::code language="csharp" source="snippets/configuration/options-object/Program.cs" highlight="22-26":::

## <a name="post-configuration"></a>配置后

绑定或指定所有配置选项值后，便可以使用发布配置功能。 通过公开前面详述的同一 [`Action<TOptions>` 参数](#actiontoptions-parameter)，可以选择调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigure%2A>。 发布配置在进行所有 `.Configure` 调用之后运行。

:::code language="csharp" source="snippets/configuration/options-postconfig/ServiceCollectionExtensions.cs" highlight="10,12":::

在上述代码中，`AddMyLibraryService` 执行以下操作：

- 扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例
- 定义一个 <xref:System.Action%601>，其中 `T` 为 `LibraryOptions` 参数 `configureOptions`
- 根据 `configureOptions` 操作调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigure%2A>

此模式下的使用者提供一个 Lambda 表达式（或一个符合 `Action<LibraryOptions>` 参数的委托），就如同在非发布配置场景中，使用者使用 [`Action<TOptions>` 参数] 提供一样：

:::code language="csharp" source="snippets/configuration/options-postconfig/Program.cs" highlight="22-26":::

## <a name="see-also"></a>另请参阅

- [.NET 中的选项模式](options.md)
- [.NET 中的依赖关系注入](dependency-injection.md)
- [依赖关系注入指南](dependency-injection-guidelines.md)
