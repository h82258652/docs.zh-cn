---
title: ASP.NET MVC 和 ASP.NET Core 之间的配置差异
description: 如何在 ASP.NET 与 ASP.NET Core 之间显著地存储和读取配置值。 本部分将讨论详细信息以及如何将配置从 ASP.NET 迁移到 ASP.NET Core。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 0e788c8606af05570d64bba257236feea11d5768
ms.sourcegitcommit: 5cad087ed0cfc4cf632b6d8270ed311e4d8b5217
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2021
ms.locfileid: "107928704"
---
# <a name="configuration-differences-between-aspnet-mvc-and-aspnet-core"></a>ASP.NET MVC 和 ASP.NET Core 之间的配置差异

如何在 ASP.NET 与 ASP.NET Core 之间显著地存储和读取配置值。

## <a name="aspnet-mvc-configuration"></a>ASP.NET MVC 配置

在 ASP.NET 应用中，配置使用内置的 .NET 配置文件， *web.config* 在应用程序文件夹中，并在服务器上 *machine.config* 。 大多数 ASP.NET MVC 和 Web API 应用将其设置存储在配置文件的 `appSettings` 或 `connectionStrings` 元素中。 某些情况还使用可映射到 settings 类的自定义配置节。

使用类访问 .NET Framework 应用中的配置 `System.Configuration.ConfigurationManager` 。 遗憾的是，此类提供对配置元素的静态访问。 因此，很少有 ASP.NET MVC 应用程序通常会抽象地访问其配置设置，或在需要时将其注入。 相反，大多数 .NET Framework 应用通常会直接在应用需要使用设置的任何位置访问配置系统。

典型的 ASP.NET MVC 配置访问：

```csharp
string connectionString =
    ConfigurationManager.ConnectionStrings["DefaultConnection"]
        .ConnectionString;
```

## <a name="aspnet-core-configuration"></a>ASP.NET Core 配置

在 ASP.NET Core 应用程序中，配置为，本身就是可配置的。 但是，大多数应用程序都使用一组默认提供的默认设置，作为标准项目模板和 `ConfigureWebHostDefaults` 其中使用的方法。 默认设置使用 JSON 格式的文件，并且能够用环境特定的文件（如 *appsettings.Development.js在上*）覆盖基本 *appsettings.js* 文件中的设置。 此外，默认配置系统会进一步覆盖所有基于文件的设置，其中包含针对相同命名设置存在的任何环境变量。 这在许多情况下非常有用，并且在部署到托管环境时特别有用，因为这样无需担心部署配置文件是否会意外覆盖重要的生产配置设置。 配置值还可以作为命令行参数提供。

可以通过多种方式在 .NET Core 中访问配置值。 由于依赖关系注入内置于 .NET Core 中，因此通常会通过插入到需要它们的类的接口来访问配置值。 这可能涉及传递接口（如 <xref:Microsoft.Extensions.Configuration.IConfiguration> ），但通常最好只传递使用 [options 模式](/aspnet/core/fundamentals/configuration/options)的类所需的设置。

图2-2 显示了如何传递 `IConfiguration` 到 Razor 页面并从其访问配置设置：

```csharp
using Microsoft.Extensions.Configuration;

public class TestModel : PageModel
{
    private readonly IConfiguration _configuration;

    public TestModel(IConfiguration configuration)
    {
        _configuration= configuration;
    }

    public ContentResult OnGet()
    {
        var myKeyValue = _configuration["MyKey"];

        // ...
    }
}
```

**图 2-2。** 访问配置值 `IConfiguration` 。

使用 [options 模式](../../core/extensions/options.md)时，设置访问类似于，但它是强类型的，更特定于使用类 (s) 所需的设置，如图2-3 所示。

```csharp
public class PositionOptions
{
    public const string Position = nameof(Position);

    public string Title { get; set; }
    public string Name { get; set; }
}

public class Test2Model : PageModel
{
    private readonly PositionOptions _options;

    public Test2Model(IOptions<PositionOptions> options)
    {
        _options = options.Value;
    }

    public ContentResult OnGet()
    {
        return Content($"Title: {_options.Title}\nName: {_options.Name}");
    }
}
```

**图 2-3**。 使用 ASP.NET Core 中的选项模式。

若要使用 options 模式，在应用启动时必须在中配置选项类型 `ConfigureServices` ：

```csharp
// required in ConfigureServices
services.Configure<PositionOptions>(Configuration.GetSection(PositionOptions.Position));
```

## <a name="migrate-configuration"></a>迁移配置

考虑如何将应用的配置设置从 .NET Framework 移植到 .NET Core 时，第一步是确定正在使用的所有配置设置。 其中的大多数将位于应用程序根文件夹中的 *web.config* 文件中，但某些应用程序也需要在共享的 *machine.config* 文件中找到设置。 这些设置将包括元素的元素 `appSettings` 、元素以及 `connectionStrings` 任何自定义配置元素。 在 .NET Core 中，所有这些设置通常存储在文件 *appsettings.js上* 。

在配置文件中的所有设置都已分类后，下一步应该是确定在应用程序本身中使用设置的位置和方式。 如果未使用某些设置，则可能会在迁移时省略这些设置。 对于每个设置，请注意正在使用的所有位置，因此可以确保在迁移代码时不会丢失任何位置。

如果仍在维护 ASP.NET 应用程序，则避免静态引用 `ConfigurationManager` 并将其替换为通过接口进行访问可能会很有帮助。 这将简化到 ASP.NET Core 配置系统的转换。 通常，对外部资源的静态访问使代码更难以测试和维护，因此在 lookout 上，其他任何应用程序都可以遵循此模式。

## <a name="references"></a>参考

- [ASP.NET Core 中的配置](/aspnet/core/fundamentals/configuration/)
- [ASP.NET Core 中的选项模式](/aspnet/core/fundamentals/configuration/options)
- [将配置迁移到 ASP.NET Core](/aspnet/core/migration/configuration)
- [重构静态配置访问](https://ardalis.com/refactoring-static-config-access/)

>[!div class="step-by-step"]
>[上一页](middleware-modules-handlers.md)
>[下一页](routing-differences.md)
