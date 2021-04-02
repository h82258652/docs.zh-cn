---
title: ASP.NET MVC 和 ASP.NET Core 之间的配置差异
description: 如何在 ASP.NET 与 ASP.NET Core 之间显著地存储和读取配置值。 本部分将讨论详细信息以及如何将配置从 ASP.NET 迁移到 ASP.NET Core。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 3d721c028b1e760a6227855451e2194d9e471a58
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122945"
---
# <a name="configuration-differences-between-aspnet-mvc-and-aspnet-core"></a><span data-ttu-id="b6867-104">ASP.NET MVC 和 ASP.NET Core 之间的配置差异</span><span class="sxs-lookup"><span data-stu-id="b6867-104">Configuration differences between ASP.NET MVC and ASP.NET Core</span></span>

<span data-ttu-id="b6867-105">如何在 ASP.NET 与 ASP.NET Core 之间显著地存储和读取配置值。</span><span class="sxs-lookup"><span data-stu-id="b6867-105">How configuration values are stored and read changed dramatically between ASP.NET and ASP.NET Core.</span></span>

## <a name="aspnet-mvc-configuration"></a><span data-ttu-id="b6867-106">ASP.NET MVC 配置</span><span class="sxs-lookup"><span data-stu-id="b6867-106">ASP.NET MVC configuration</span></span>

<span data-ttu-id="b6867-107">在 ASP.NET 应用中，配置使用内置的 .NET 配置文件， *web.config* 在应用程序文件夹中，并在服务器上 *machine.config* 。</span><span class="sxs-lookup"><span data-stu-id="b6867-107">In ASP.NET apps, configuration uses the built-in .NET configuration files, *web.config* in the app folder and *machine.config* on the server.</span></span> <span data-ttu-id="b6867-108">大多数 ASP.NET MVC 和 Web API 应用将其设置存储在配置文件的 `appSettings` 或 `connectionStrings` 元素中。</span><span class="sxs-lookup"><span data-stu-id="b6867-108">Most ASP.NET MVC and Web API apps store their settings in the configuration file's `appSettings` or `connectionStrings` elements.</span></span> <span data-ttu-id="b6867-109">某些情况还使用可映射到 settings 类的自定义配置节。</span><span class="sxs-lookup"><span data-stu-id="b6867-109">Some also use custom configuration sections that can be mapped to a settings class.</span></span>

<span data-ttu-id="b6867-110">使用类访问 .NET Framework 应用中的配置 `System.Configuration.ConfigurationManager` 。</span><span class="sxs-lookup"><span data-stu-id="b6867-110">Configuration in a .NET Framework app is accessed using the `System.Configuration.ConfigurationManager` class.</span></span> <span data-ttu-id="b6867-111">遗憾的是，此类提供对配置元素的静态访问。</span><span class="sxs-lookup"><span data-stu-id="b6867-111">Unfortunately, this class provides static access to the configuration elements.</span></span> <span data-ttu-id="b6867-112">因此，很少有 ASP.NET MVC 应用程序通常会抽象地访问其配置设置，或在需要时将其注入。</span><span class="sxs-lookup"><span data-stu-id="b6867-112">As a result, very few ASP.NET MVC apps tend to abstract access to their configuration settings or inject them where needed.</span></span> <span data-ttu-id="b6867-113">相反，大多数 .NET Framework 应用通常会直接在应用需要使用设置的任何位置访问配置系统。</span><span class="sxs-lookup"><span data-stu-id="b6867-113">Instead, most .NET Framework apps tend to directly access the configuration system anywhere the app needs to use a setting.</span></span>

<span data-ttu-id="b6867-114">典型的 ASP.NET MVC 配置访问：</span><span class="sxs-lookup"><span data-stu-id="b6867-114">Typical ASP.NET MVC configuration access:</span></span>

```csharp
string connectionString =
    ConfigurationManager.ConnectionStrings["DefaultConnection"]
        .ConnectionString;
```

## <a name="aspnet-core-configuration"></a><span data-ttu-id="b6867-115">ASP.NET Core 配置</span><span class="sxs-lookup"><span data-stu-id="b6867-115">ASP.NET Core configuration</span></span>

<span data-ttu-id="b6867-116">在 ASP.NET Core 应用程序中，配置为，本身就是可配置的。</span><span class="sxs-lookup"><span data-stu-id="b6867-116">In ASP.NET Core apps, configuration is, itself, configurable.</span></span> <span data-ttu-id="b6867-117">但是，大多数应用程序都使用一组默认提供的默认设置，作为标准项目模板和 `ConfigureWebHostDefaults` 其中使用的方法。</span><span class="sxs-lookup"><span data-stu-id="b6867-117">However, most apps use a set of defaults provided as part of the standard project templates and the `ConfigureWebHostDefaults` method used in them.</span></span> <span data-ttu-id="b6867-118">默认设置使用 JSON 格式的文件，并且能够用环境特定的文件（如 *appsettings.Development.js在上*）覆盖基本 *appsettings.js* 文件中的设置。</span><span class="sxs-lookup"><span data-stu-id="b6867-118">The default settings use JSON formatted files, with the ability to override settings in the base *appsettings.json* file with environment-specific files like *appsettings.Development.json*.</span></span> <span data-ttu-id="b6867-119">此外，默认配置系统会进一步覆盖所有基于文件的设置，其中包含针对相同命名设置存在的任何环境变量。</span><span class="sxs-lookup"><span data-stu-id="b6867-119">Additionally, the default configuration system further overrides all file-based settings with any environment variable that exists for the same named setting.</span></span> <span data-ttu-id="b6867-120">这在许多情况下非常有用，并且在部署到托管环境时特别有用，因为这样无需担心部署配置文件是否会意外覆盖重要的生产配置设置。</span><span class="sxs-lookup"><span data-stu-id="b6867-120">This is useful in many scenarios and is especially useful when deploying to a hosting environment, since it eliminates the need to worry about whether deploying configuration files will accidentally overwrite important production configuration settings.</span></span> <span data-ttu-id="b6867-121">配置值还可以作为命令行参数提供。</span><span class="sxs-lookup"><span data-stu-id="b6867-121">Configuration values can also be provided as command line arguments.</span></span>

<span data-ttu-id="b6867-122">可以通过多种方式在 .NET Core 中访问配置值。</span><span class="sxs-lookup"><span data-stu-id="b6867-122">Accessing configuration values can be done in many ways in .NET Core.</span></span> <span data-ttu-id="b6867-123">由于依赖关系注入内置于 .NET Core 中，因此通常会通过插入到需要它们的类的接口来访问配置值。</span><span class="sxs-lookup"><span data-stu-id="b6867-123">Because dependency injection is built into .NET Core, configuration values are generally accessed through an interface that is injected into classes that need them.</span></span> <span data-ttu-id="b6867-124">这可能涉及传递接口（如 <xref:Microsoft.Extensions.Configuration.IConfiguration> ），但通常最好只传递使用 [options 模式](/aspnet/core/fundamentals/configuration/options)的类所需的设置。</span><span class="sxs-lookup"><span data-stu-id="b6867-124">This can involve passing a interface like <xref:Microsoft.Extensions.Configuration.IConfiguration>, but usually it's better to pass just the settings required by the class using the [options pattern](/aspnet/core/fundamentals/configuration/options).</span></span>

<span data-ttu-id="b6867-125">图2-2 显示了如何传递 `IConfiguration` 到 Razor 页面并从其访问配置设置：</span><span class="sxs-lookup"><span data-stu-id="b6867-125">Figure 2-2 shows how to pass `IConfiguration` into a Razor Page and access configuration settings from it:</span></span>

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

<span data-ttu-id="b6867-126">**图 2-2。**</span><span class="sxs-lookup"><span data-stu-id="b6867-126">**Figure 2-2.**</span></span> <span data-ttu-id="b6867-127">访问配置值 `IConfiguration` 。</span><span class="sxs-lookup"><span data-stu-id="b6867-127">Accessing configuration values with `IConfiguration`.</span></span>

<span data-ttu-id="b6867-128">使用 [options 模式](/dotnet/core/extensions/options)时，设置访问类似于，但它是强类型的，更特定于使用类 (s) 所需的设置，如图2-3 所示。</span><span class="sxs-lookup"><span data-stu-id="b6867-128">Using the [options pattern](/dotnet/core/extensions/options), settings access is similar but is strongly typed and more specific to the setting(s) needed by the consuming class, as Figure 2-3 demonstrates.</span></span>

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

<span data-ttu-id="b6867-129">**图 2-3**。</span><span class="sxs-lookup"><span data-stu-id="b6867-129">**Figure 2-3.**</span></span> <span data-ttu-id="b6867-130">使用 ASP.NET Core 中的选项模式。</span><span class="sxs-lookup"><span data-stu-id="b6867-130">Using the options pattern in ASP.NET Core.</span></span>

<span data-ttu-id="b6867-131">若要使用 options 模式，在应用启动时必须在中配置选项类型 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="b6867-131">For the options pattern to work, the options type must be configured in `ConfigureServices` when the app starts up:</span></span>

```csharp
// required in ConfigureServices
services.Configure<PositionOptions>(Configuration.GetSection(PositionOptions.Position));
```

## <a name="migrate-configuration"></a><span data-ttu-id="b6867-132">迁移配置</span><span class="sxs-lookup"><span data-stu-id="b6867-132">Migrate configuration</span></span>

<span data-ttu-id="b6867-133">考虑如何将应用的配置设置从 .NET Framework 移植到 .NET Core 时，第一步是确定正在使用的所有配置设置。</span><span class="sxs-lookup"><span data-stu-id="b6867-133">When considering how to port an app's configuration settings from .NET Framework to .NET Core, the first step is to identify all of the configuration settings that are being used.</span></span> <span data-ttu-id="b6867-134">其中的大多数将位于应用程序根文件夹中的 *web.config* 文件中，但某些应用程序也需要在共享的 *machine.config* 文件中找到设置。</span><span class="sxs-lookup"><span data-stu-id="b6867-134">Most of these will be in the *web.config* file in the app's root folder, but some apps expect settings to be found in the shared *machine.config* file as well.</span></span> <span data-ttu-id="b6867-135">这些设置将包括元素的元素 `appSettings` 、元素以及 `connectionStrings` 任何自定义配置元素。</span><span class="sxs-lookup"><span data-stu-id="b6867-135">These settings will include elements of the `appSettings` element, the `connectionStrings` element, and any custom configuration elements as well.</span></span> <span data-ttu-id="b6867-136">在 .NET Core 中，所有这些设置通常存储在文件 *appsettings.js上* 。</span><span class="sxs-lookup"><span data-stu-id="b6867-136">In .NET Core, all of these settings are typically stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="b6867-137">在配置文件中的所有设置都已分类后，下一步应该是确定在应用程序本身中使用设置的位置和方式。</span><span class="sxs-lookup"><span data-stu-id="b6867-137">Once all settings in the config files have been cataloged, the next step should be to identify where and how the settings are used in the app itself.</span></span> <span data-ttu-id="b6867-138">如果未使用某些设置，则可能会在迁移时省略这些设置。</span><span class="sxs-lookup"><span data-stu-id="b6867-138">If some settings aren't being used, these can probably be omitted from the migration.</span></span> <span data-ttu-id="b6867-139">对于每个设置，请注意正在使用的所有位置，因此可以确保在迁移代码时不会丢失任何位置。</span><span class="sxs-lookup"><span data-stu-id="b6867-139">For each setting, note all of the places it's being used so you can be sure you don't miss any when you migrate the code.</span></span>

<span data-ttu-id="b6867-140">如果仍在维护 ASP.NET 应用程序，则避免静态引用 `ConfigurationManager` 并将其替换为通过接口进行访问可能会很有帮助。</span><span class="sxs-lookup"><span data-stu-id="b6867-140">If you're still maintaining the ASP.NET app, it may be helpful to avoid static references to `ConfigurationManager` and replace them with access through interfaces.</span></span> <span data-ttu-id="b6867-141">这将简化到 ASP.NET Core 配置系统的转换。</span><span class="sxs-lookup"><span data-stu-id="b6867-141">This will ease the transition to ASP.NET Core's configuration system.</span></span> <span data-ttu-id="b6867-142">通常，对外部资源的静态访问使代码更难以测试和维护，因此在 lookout 上，其他任何应用程序都可以遵循此模式。</span><span class="sxs-lookup"><span data-stu-id="b6867-142">In general, static access to external resources makes code harder to test and maintain, so be on the lookout for anywhere else the app may be following this pattern.</span></span>

## <a name="references"></a><span data-ttu-id="b6867-143">参考</span><span class="sxs-lookup"><span data-stu-id="b6867-143">References</span></span>

- [<span data-ttu-id="b6867-144">ASP.NET Core 中的配置</span><span class="sxs-lookup"><span data-stu-id="b6867-144">Configuration in ASP.NET Core</span></span>](/aspnet/core/fundamentals/configuration/)
- [<span data-ttu-id="b6867-145">ASP.NET Core 中的选项模式</span><span class="sxs-lookup"><span data-stu-id="b6867-145">Options pattern in ASP.NET Core</span></span>](/aspnet/core/fundamentals/configuration/options)
- [<span data-ttu-id="b6867-146">将配置迁移到 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6867-146">Migrate configuration to ASP.NET Core</span></span>](/aspnet/core/migration/configuration)
- [<span data-ttu-id="b6867-147">重构静态配置访问</span><span class="sxs-lookup"><span data-stu-id="b6867-147">Refactoring Static Config Access</span></span>](https://ardalis.com/refactoring-static-config-access/)

>[!div class="step-by-step"]
><span data-ttu-id="b6867-148">[上一页](middleware-modules-handlers.md)
>[下一页](routing-differences.md)</span><span class="sxs-lookup"><span data-stu-id="b6867-148">[Previous](middleware-modules-handlers.md)
[Next](routing-differences.md)</span></span>
