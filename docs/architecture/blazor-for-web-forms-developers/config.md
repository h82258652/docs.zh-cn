---
title: 应用配置
description: 了解如何在不使用 ConfigurationManager 的情况下配置 Blazor 应用。
author: csharpfritz
ms.author: jefritz
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: 360d9077bc981a2e9875bb1f86b49c0029424d6e
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509788"
---
# <a name="app-configuration"></a><span data-ttu-id="60668-103">应用配置</span><span class="sxs-lookup"><span data-stu-id="60668-103">App configuration</span></span>

<span data-ttu-id="60668-104">在 Web Forms 中加载应用配置的主要方法是使用服务器上的 web.config 文件中的条目或使用 web.config 引用的相关配置文件。你可以使用静态 `ConfigurationManager` 对象与应用设置、数据存储库连接字符串以及添加到应用的其他扩展配置提供程序进行交互 。</span><span class="sxs-lookup"><span data-stu-id="60668-104">The primary way to load app configuration in Web Forms is with entries in the *web.config* file&mdash;either on the server or a related configuration file referenced by *web.config*. You can use the static `ConfigurationManager` object to interact with app settings, data repository connection strings, and other extended configuration providers that are added into the app.</span></span> <span data-ttu-id="60668-105">通常使用以下代码查看与应用配置的交互：</span><span class="sxs-lookup"><span data-stu-id="60668-105">It's typical to see interactions with app configuration as seen in the following code:</span></span>

```csharp
var configurationValue = ConfigurationManager.AppSettings["ConfigurationSettingName"];
var connectionString = ConfigurationManager.ConnectionStrings["MyDatabaseConnectionName"].ConnectionString;
```

<span data-ttu-id="60668-106">通过 ASP.NET Core 和服务器端的 Blazor，应用托管在 Windows IIS 服务器上时，可能会出现 web.config 文件。</span><span class="sxs-lookup"><span data-stu-id="60668-106">With ASP.NET Core and server-side Blazor, the *web.config* file MAY be present if your app is hosted on a Windows IIS server.</span></span> <span data-ttu-id="60668-107">但是 `ConfigurationManager` 与此配置没有交互，你可以接收更多来自其他源的结构化应用配置。</span><span class="sxs-lookup"><span data-stu-id="60668-107">However, there's no `ConfigurationManager` interaction with this configuration, and you can receive more structured app configuration from other sources.</span></span> <span data-ttu-id="60668-108">我们来看看如何收集配置，以及如何继续访问 web.config 文件的配置信息。</span><span class="sxs-lookup"><span data-stu-id="60668-108">Let's take a look at how configuration is gathered and how you can still access configuration information from a *web.config* file.</span></span>

## <a name="configuration-sources"></a><span data-ttu-id="60668-109">配置源</span><span class="sxs-lookup"><span data-stu-id="60668-109">Configuration sources</span></span>

<span data-ttu-id="60668-110">ASP.NET Core 识别到有许多配置源可用于你的应用。</span><span class="sxs-lookup"><span data-stu-id="60668-110">ASP.NET Core recognizes there are many configuration sources you may want to use for your app.</span></span> <span data-ttu-id="60668-111">框架尝试为你提供默认下的最佳功能。</span><span class="sxs-lookup"><span data-stu-id="60668-111">The framework attempts to offer you the best of these features by default.</span></span> <span data-ttu-id="60668-112">ASP.NET Core 从各种源读取并聚合配置。</span><span class="sxs-lookup"><span data-stu-id="60668-112">Configuration is read and aggregated from these various sources by ASP.NET Core.</span></span> <span data-ttu-id="60668-113">以后加载的相同配置键的值优先于以前的值。</span><span class="sxs-lookup"><span data-stu-id="60668-113">Later loaded values for the same configuration key take precedence over earlier values.</span></span>

<span data-ttu-id="60668-114">ASP.NET Core 旨在能够识别云，并使操作员和开发人员便于配置应用。</span><span class="sxs-lookup"><span data-stu-id="60668-114">ASP.NET Core was designed to be cloud-aware and to make the configuration of apps easier for both operators and developers.</span></span> <span data-ttu-id="60668-115">ASP.NET Core 可以识别环境，并且能够感知自己是在 `Production` 中还是在 `Development` 环境中运行。</span><span class="sxs-lookup"><span data-stu-id="60668-115">ASP.NET Core is environment-aware and knows if it's running in your `Production` or `Development` environment.</span></span> <span data-ttu-id="60668-116">环境指示器在 `ASPNETCORE_ENVIRONMENT` 系统环境变量中进行设置。</span><span class="sxs-lookup"><span data-stu-id="60668-116">The environment indicator is set in the `ASPNETCORE_ENVIRONMENT` system environment variable.</span></span> <span data-ttu-id="60668-117">如果未配置任何值，应用默认在 `Production` 环境中运行。</span><span class="sxs-lookup"><span data-stu-id="60668-117">If no value is configured, the app defaults to running in the `Production` environment.</span></span>

<span data-ttu-id="60668-118">应用可根据环境名称从多个源触发并添加配置。</span><span class="sxs-lookup"><span data-stu-id="60668-118">Your app can trigger and add configuration from several sources based on the environment's name.</span></span> <span data-ttu-id="60668-119">默认情况下，将按照列出的顺序从以下各种资源加载配置：</span><span class="sxs-lookup"><span data-stu-id="60668-119">By default, the configuration is loaded from the following resources in the order listed:</span></span>

1. <span data-ttu-id="60668-120">appsettings.json 文件（如果存在）</span><span class="sxs-lookup"><span data-stu-id="60668-120">*appsettings.json* file, if present</span></span>
1. <span data-ttu-id="60668-121">appsettings.{ENVIRONMENT_NAME}.json 文件（如果存在）</span><span class="sxs-lookup"><span data-stu-id="60668-121">*appsettings.{ENVIRONMENT_NAME}.json* file, if present</span></span>
1. <span data-ttu-id="60668-122">磁盘上的用户机密文件（如果存在）</span><span class="sxs-lookup"><span data-stu-id="60668-122">User secrets file on disk, if present</span></span>
1. <span data-ttu-id="60668-123">环境变量</span><span class="sxs-lookup"><span data-stu-id="60668-123">Environment variables</span></span>
1. <span data-ttu-id="60668-124">命令行参数</span><span class="sxs-lookup"><span data-stu-id="60668-124">Command-line arguments</span></span>

## <a name="appsettingsjson-format-and-access"></a><span data-ttu-id="60668-125">appsettings.json 格式和访问</span><span class="sxs-lookup"><span data-stu-id="60668-125">appsettings.json format and access</span></span>

<span data-ttu-id="60668-126">appsettings.json 文件是分层的，其值的结构如以下 JSON 所示：</span><span class="sxs-lookup"><span data-stu-id="60668-126">The *appsettings.json* file can be hierarchical with values structured like the following JSON:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="60668-127">出现上述 JSON 时，配置系统合并子值，并引用其完全限定的分层路径。</span><span class="sxs-lookup"><span data-stu-id="60668-127">When presented with the preceding JSON, the configuration system flattens child values and references their fully qualified hierarchical paths.</span></span> <span data-ttu-id="60668-128">冒号 (`:`) 字符用于分隔层次结构中的每个属性。</span><span class="sxs-lookup"><span data-stu-id="60668-128">A colon (`:`) character separates each property in the hierarchy.</span></span> <span data-ttu-id="60668-129">例如，配置键 `section1:key0` 访问 `section1` 对象文字的 `key0` 值。</span><span class="sxs-lookup"><span data-stu-id="60668-129">For example, the configuration key `section1:key0` accesses the `section1` object literal's `key0` value.</span></span>

## <a name="user-secrets"></a><span data-ttu-id="60668-130">用户机密</span><span class="sxs-lookup"><span data-stu-id="60668-130">User secrets</span></span>

<span data-ttu-id="60668-131">用户机密是：</span><span class="sxs-lookup"><span data-stu-id="60668-131">User secrets are:</span></span>

* <span data-ttu-id="60668-132">配置值，存储在应用开发文件夹之外的开发人员工作站的 JSON 文件中。</span><span class="sxs-lookup"><span data-stu-id="60668-132">Configuration values that are stored in a JSON file on the developer's workstation, outside of the app development folder.</span></span>
* <span data-ttu-id="60668-133">仅在 `Development` 环境中运行时加载。</span><span class="sxs-lookup"><span data-stu-id="60668-133">Only loaded when running in the `Development` environment.</span></span>
* <span data-ttu-id="60668-134">与特定应用相关联。</span><span class="sxs-lookup"><span data-stu-id="60668-134">Associated with a specific app.</span></span>
* <span data-ttu-id="60668-135">通过 .NET CLI 的 `user-secrets` 命令进行管理。</span><span class="sxs-lookup"><span data-stu-id="60668-135">Managed with the .NET CLI's `user-secrets` command.</span></span>

<span data-ttu-id="60668-136">通过执行以下 `user-secrets` 命令为应用配置机密存储：</span><span class="sxs-lookup"><span data-stu-id="60668-136">Configure your app for secrets storage by executing the `user-secrets` command:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="60668-137">上面的命令将 `UserSecretsId` 元素添加到项目文件。</span><span class="sxs-lookup"><span data-stu-id="60668-137">The preceding command adds a `UserSecretsId` element to the project file.</span></span> <span data-ttu-id="60668-138">该元素包含用于将机密与应用相关联的 GUID。</span><span class="sxs-lookup"><span data-stu-id="60668-138">The element contains a GUID, which is used to associate secrets with the app.</span></span> <span data-ttu-id="60668-139">然后可使用 `set` 命令定义机密。</span><span class="sxs-lookup"><span data-stu-id="60668-139">You can then define a secret with the `set` command.</span></span> <span data-ttu-id="60668-140">例如：</span><span class="sxs-lookup"><span data-stu-id="60668-140">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Parent:ApiKey" "12345"
```

<span data-ttu-id="60668-141">上面的命令使 `Parent:ApiKey` 配置键在值为 `12345` 的开发人员工作站上可用。</span><span class="sxs-lookup"><span data-stu-id="60668-141">The preceding command makes the `Parent:ApiKey` configuration key available on a developer's workstation with the value `12345`.</span></span>

<span data-ttu-id="60668-142">有关创建、存储和管理用户机密的详细信息，请参阅[在 ASP.NET Core 中进行开发期间安全存储应用机密](/aspnet/core/security/app-secrets)文档。</span><span class="sxs-lookup"><span data-stu-id="60668-142">For more information about creating, storing, and managing user secrets, see the [Safe storage of app secrets in development in ASP.NET Core](/aspnet/core/security/app-secrets) document.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="60668-143">环境变量</span><span class="sxs-lookup"><span data-stu-id="60668-143">Environment variables</span></span>

<span data-ttu-id="60668-144">加载到应用配置的下一组值是系统的环境变量。</span><span class="sxs-lookup"><span data-stu-id="60668-144">The next set of values loaded into your app configuration is the system's environment variables.</span></span> <span data-ttu-id="60668-145">你现在可以通过配置 API 访问系统的所有环境变量设置。</span><span class="sxs-lookup"><span data-stu-id="60668-145">All of your system's environment variable settings are now accessible to you through the configuration API.</span></span> <span data-ttu-id="60668-146">在应用中读取分层值时，将合并分层值并用冒号分隔。</span><span class="sxs-lookup"><span data-stu-id="60668-146">Hierarchical values are flattened and separated by colon characters when read inside your app.</span></span> <span data-ttu-id="60668-147">但某些操作系统的环境变量名称不支持冒号。</span><span class="sxs-lookup"><span data-stu-id="60668-147">However, some operating systems don't allow the colon character environment variable names.</span></span> <span data-ttu-id="60668-148">ASP.NET Core 解决了此限制，读取值时，它可将值的双下划线 (`__`) 转换为冒号。</span><span class="sxs-lookup"><span data-stu-id="60668-148">ASP.NET Core addresses this limitation by converting values that have double-underscores (`__`) into a colon when they're accessed.</span></span> <span data-ttu-id="60668-149">上面的“用户机密”部分的 `Parent:ApiKey` 值可以使用环境变量 `Parent__ApiKey` 重写。</span><span class="sxs-lookup"><span data-stu-id="60668-149">The `Parent:ApiKey` value from the user secrets section above can be overridden with the environment variable `Parent__ApiKey`.</span></span>

## <a name="command-line-arguments"></a><span data-ttu-id="60668-150">命令行参数</span><span class="sxs-lookup"><span data-stu-id="60668-150">Command-line arguments</span></span>

<span data-ttu-id="60668-151">启动应用时，配置还可作为命令行参数提供。</span><span class="sxs-lookup"><span data-stu-id="60668-151">Configuration can also be provided as command-line arguments when your app is started.</span></span> <span data-ttu-id="60668-152">可使用双划线 (`--`) 或正斜杠 (`/`) 表示法来表示要设置的配置值以及要配置的值的名称。</span><span class="sxs-lookup"><span data-stu-id="60668-152">Use the double-dash (`--`) or forward-slash (`/`) notation to indicate the name of the configuration value to set and the value to be configured.</span></span> <span data-ttu-id="60668-153">语法与以下命令类似：</span><span class="sxs-lookup"><span data-stu-id="60668-153">The syntax resembles the following commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run Parent:ApiKey=67890
```

## <a name="the-return-of-webconfig"></a><span data-ttu-id="60668-154">返回 web.config</span><span class="sxs-lookup"><span data-stu-id="60668-154">The return of web.config</span></span>

<span data-ttu-id="60668-155">如果已将应用部署到 IIS 上的 Windows，web.config 仍会对管理应用的 IIS 进行配置。</span><span class="sxs-lookup"><span data-stu-id="60668-155">If you've deployed your app to Windows on IIS, the *web.config* file still configures IIS to manage your app.</span></span> <span data-ttu-id="60668-156">默认情况下，IIS 添加对 ASP.NET Core 模块 (ANCM) 的引用。</span><span class="sxs-lookup"><span data-stu-id="60668-156">By default, IIS adds a reference to the ASP.NET Core Module (ANCM).</span></span> <span data-ttu-id="60668-157">ANCM 是一个本机 IIS 模块，代替 Kestrel Web 服务器托管应用。</span><span class="sxs-lookup"><span data-stu-id="60668-157">ANCM is a native IIS module that hosts your app in place of the Kestrel web server.</span></span> <span data-ttu-id="60668-158">此 web.config 部分与以下 XML 标记类似：</span><span class="sxs-lookup"><span data-stu-id="60668-158">This *web.config* section resembles the following XML markup:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="60668-159">应用特定的配置可以通过在 `aspNetCore` 元素中嵌套 `environmentVariables` 元素进行定义。</span><span class="sxs-lookup"><span data-stu-id="60668-159">App-specific configuration can be defined by nesting an `environmentVariables` element in the `aspNetCore` element.</span></span> <span data-ttu-id="60668-160">在此部分中定义的值以环境变量的形式提供给 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="60668-160">The values defined in this section are presented to the ASP.NET Core app as environment variables.</span></span> <span data-ttu-id="60668-161">在应用启动期间，环境变量会适当加载。</span><span class="sxs-lookup"><span data-stu-id="60668-161">The environment variables load appropriately during that segment of app startup.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="Parent:ApiKey" value="67890" />
  </environmentVariables>
</aspNetCore>
```

## <a name="read-configuration-in-the-app"></a><span data-ttu-id="60668-162">读取应用中的配置</span><span class="sxs-lookup"><span data-stu-id="60668-162">Read configuration in the app</span></span>

<span data-ttu-id="60668-163">ASP.NET Core 通过 <xref:Microsoft.Extensions.Configuration.IConfiguration> 接口提供应用配置。</span><span class="sxs-lookup"><span data-stu-id="60668-163">ASP.NET Core provides app configuration through the <xref:Microsoft.Extensions.Configuration.IConfiguration> interface.</span></span> <span data-ttu-id="60668-164">此配置接口应由 Blazor 组件、Blazor 页面以及需要访问配置的其他任何 ASP.NET Core 托管的类请求。</span><span class="sxs-lookup"><span data-stu-id="60668-164">This configuration interface should be requested by your Blazor components, Blazor pages, and any other ASP.NET Core-managed class that needs access to configuration.</span></span> <span data-ttu-id="60668-165">ASP.NET Core 框架使用先前配置的解析配置自动填充此接口。</span><span class="sxs-lookup"><span data-stu-id="60668-165">The ASP.NET Core framework will automatically populate this interface with the resolved configuration configured earlier.</span></span> <span data-ttu-id="60668-166">在 Blazor 页面或组件的 Razor 标记上，可以使用 .razor 文件顶部的 `@inject` 指令注入 `IConfiguration` 对象，如下所示：</span><span class="sxs-lookup"><span data-stu-id="60668-166">On a Blazor page or a component's Razor markup, you can inject the `IConfiguration` object with an `@inject` directive at the top of the *.razor* file like this:</span></span>

```razor
@inject IConfiguration Configuration
```

<span data-ttu-id="60668-167">上面的语句使 `IConfiguration` 对象在 Razor 模板的其余部分中可作为 `Configuration` 变量提供。</span><span class="sxs-lookup"><span data-stu-id="60668-167">This preceding statement makes the `IConfiguration` object available as the `Configuration` variable throughout the rest of the Razor template.</span></span>

<span data-ttu-id="60668-168">可通过指定作为索引器参数查找的配置设置层次结构，读取各个配置设置：</span><span class="sxs-lookup"><span data-stu-id="60668-168">Individual configuration settings can be read by specifying the configuration setting hierarchy sought as an indexer parameter:</span></span>

```csharp
var mySetting = Configuration["section1:key0"];
```

<span data-ttu-id="60668-169">你可通过使用 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> 方法检索特定位置的键集合来提取整个配置部分，该方法的语法与前面的示例中检索 section1 的配置的 `GetSection("section1")` 类似。</span><span class="sxs-lookup"><span data-stu-id="60668-169">You can fetch entire configuration sections by using the <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> method to retrieve a collection of keys at a specific location with a syntax similar to `GetSection("section1")` to retrieve the configuration for section1 from the earlier example.</span></span>

## <a name="strongly-typed-configuration"></a><span data-ttu-id="60668-170">强类型配置</span><span class="sxs-lookup"><span data-stu-id="60668-170">Strongly typed configuration</span></span>

<span data-ttu-id="60668-171">通过 Web Forms，可以创建继承自 <xref:System.Configuration.ConfigurationSection> 类型和关联类型的强类型配置类型。</span><span class="sxs-lookup"><span data-stu-id="60668-171">With Web Forms, it was possible to create a strongly typed configuration type that inherited from the <xref:System.Configuration.ConfigurationSection> type and associated types.</span></span> <span data-ttu-id="60668-172">通过 `ConfigurationSection`，可以为这些配置值配置业务规则和流程。</span><span class="sxs-lookup"><span data-stu-id="60668-172">A `ConfigurationSection` allowed you to configure some business rules and processing for those configuration values.</span></span>

<span data-ttu-id="60668-173">在 ASP.NET Core 中，可以指定接收配置值的类层次结构。</span><span class="sxs-lookup"><span data-stu-id="60668-173">In ASP.NET Core, you can specify a class hierarchy that will receive the configuration values.</span></span> <span data-ttu-id="60668-174">这些类：</span><span class="sxs-lookup"><span data-stu-id="60668-174">These classes:</span></span>

* <span data-ttu-id="60668-175">无需继承自父类。</span><span class="sxs-lookup"><span data-stu-id="60668-175">Don't need to inherit from a parent class.</span></span>
* <span data-ttu-id="60668-176">应包括与你要捕获的配置结构的属性和类型引用相匹配的 `public` 属性。</span><span class="sxs-lookup"><span data-stu-id="60668-176">Should include `public` properties that match the properties and type references for the configuration structure you wish to capture.</span></span>

<span data-ttu-id="60668-177">对于前面的 appsettings.js 示例，可以定义以下类来捕获值：</span><span class="sxs-lookup"><span data-stu-id="60668-177">For the earlier *appsettings.json* sample, you could define the following classes to capture the values:</span></span>

```csharp
public class MyConfig
{
    public MyConfigSection section0 { get; set;}

    public MyConfigSection section1 { get; set;}
}

public class MyConfigSection
{
    public string key0 { get; set; }

    public string key1 { get; set; }
}
```

<span data-ttu-id="60668-178">可通过将以下行添加到 `Startup.ConfigureServices` 方法来填充此类层次结构：</span><span class="sxs-lookup"><span data-stu-id="60668-178">This class hierarchy can be populated by adding the following line to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.Configure<MyConfig>(Configuration);
```

<span data-ttu-id="60668-179">在应用的其余部分，可将输入参数添加到 `IOptions<MyConfig>` 类型的 Razor 模板中的类或 `@inject` 指令，来接收强类型配置设置。</span><span class="sxs-lookup"><span data-stu-id="60668-179">In the rest of the app, you can add an input parameter to classes or an `@inject` directive in Razor templates of type `IOptions<MyConfig>` to receive the strongly typed configuration settings.</span></span> <span data-ttu-id="60668-180">`IOptions<MyConfig>.Value` 属性将生成从配置设置填充的 `MyConfig` 值。</span><span class="sxs-lookup"><span data-stu-id="60668-180">The `IOptions<MyConfig>.Value` property will yield the `MyConfig` value populated from the configuration settings.</span></span>

```razor
@inject IOptions<MyConfig> options
@code {
    var MyConfiguration = options.Value;
    var theSetting = MyConfiguration.section1.key0;
}
```

<span data-ttu-id="60668-181">有关选项功能的详细信息，请参阅 [ASP.NET Core 的选项模式](/aspnet/core/fundamentals/configuration/options#options-interfaces)文档。</span><span class="sxs-lookup"><span data-stu-id="60668-181">More information about the Options feature can be found in the [Options pattern in ASP.NET Core](/aspnet/core/fundamentals/configuration/options#options-interfaces) document.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="60668-182">[上一页](middleware.md)
>[下一页](security-authentication-authorization.md)</span><span class="sxs-lookup"><span data-stu-id="60668-182">[Previous](middleware.md)
[Next](security-authentication-authorization.md)</span></span>
