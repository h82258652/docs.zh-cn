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
# <a name="app-configuration"></a>应用配置

在 Web Forms 中加载应用配置的主要方法是使用服务器上的 web.config 文件中的条目或使用 web.config 引用的相关配置文件。你可以使用静态 `ConfigurationManager` 对象与应用设置、数据存储库连接字符串以及添加到应用的其他扩展配置提供程序进行交互 。 通常使用以下代码查看与应用配置的交互：

```csharp
var configurationValue = ConfigurationManager.AppSettings["ConfigurationSettingName"];
var connectionString = ConfigurationManager.ConnectionStrings["MyDatabaseConnectionName"].ConnectionString;
```

通过 ASP.NET Core 和服务器端的 Blazor，应用托管在 Windows IIS 服务器上时，可能会出现 web.config 文件。 但是 `ConfigurationManager` 与此配置没有交互，你可以接收更多来自其他源的结构化应用配置。 我们来看看如何收集配置，以及如何继续访问 web.config 文件的配置信息。

## <a name="configuration-sources"></a>配置源

ASP.NET Core 识别到有许多配置源可用于你的应用。 框架尝试为你提供默认下的最佳功能。 ASP.NET Core 从各种源读取并聚合配置。 以后加载的相同配置键的值优先于以前的值。

ASP.NET Core 旨在能够识别云，并使操作员和开发人员便于配置应用。 ASP.NET Core 可以识别环境，并且能够感知自己是在 `Production` 中还是在 `Development` 环境中运行。 环境指示器在 `ASPNETCORE_ENVIRONMENT` 系统环境变量中进行设置。 如果未配置任何值，应用默认在 `Production` 环境中运行。

应用可根据环境名称从多个源触发并添加配置。 默认情况下，将按照列出的顺序从以下各种资源加载配置：

1. appsettings.json 文件（如果存在）
1. appsettings.{ENVIRONMENT_NAME}.json 文件（如果存在）
1. 磁盘上的用户机密文件（如果存在）
1. 环境变量
1. 命令行参数

## <a name="appsettingsjson-format-and-access"></a>appsettings.json 格式和访问

appsettings.json 文件是分层的，其值的结构如以下 JSON 所示：

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

出现上述 JSON 时，配置系统合并子值，并引用其完全限定的分层路径。 冒号 (`:`) 字符用于分隔层次结构中的每个属性。 例如，配置键 `section1:key0` 访问 `section1` 对象文字的 `key0` 值。

## <a name="user-secrets"></a>用户机密

用户机密是：

* 配置值，存储在应用开发文件夹之外的开发人员工作站的 JSON 文件中。
* 仅在 `Development` 环境中运行时加载。
* 与特定应用相关联。
* 通过 .NET CLI 的 `user-secrets` 命令进行管理。

通过执行以下 `user-secrets` 命令为应用配置机密存储：

```dotnetcli
dotnet user-secrets init
```

上面的命令将 `UserSecretsId` 元素添加到项目文件。 该元素包含用于将机密与应用相关联的 GUID。 然后可使用 `set` 命令定义机密。 例如：

```dotnetcli
dotnet user-secrets set "Parent:ApiKey" "12345"
```

上面的命令使 `Parent:ApiKey` 配置键在值为 `12345` 的开发人员工作站上可用。

有关创建、存储和管理用户机密的详细信息，请参阅[在 ASP.NET Core 中进行开发期间安全存储应用机密](/aspnet/core/security/app-secrets)文档。

## <a name="environment-variables"></a>环境变量

加载到应用配置的下一组值是系统的环境变量。 你现在可以通过配置 API 访问系统的所有环境变量设置。 在应用中读取分层值时，将合并分层值并用冒号分隔。 但某些操作系统的环境变量名称不支持冒号。 ASP.NET Core 解决了此限制，读取值时，它可将值的双下划线 (`__`) 转换为冒号。 上面的“用户机密”部分的 `Parent:ApiKey` 值可以使用环境变量 `Parent__ApiKey` 重写。

## <a name="command-line-arguments"></a>命令行参数

启动应用时，配置还可作为命令行参数提供。 可使用双划线 (`--`) 或正斜杠 (`/`) 表示法来表示要设置的配置值以及要配置的值的名称。 语法与以下命令类似：

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run Parent:ApiKey=67890
```

## <a name="the-return-of-webconfig"></a>返回 web.config

如果已将应用部署到 IIS 上的 Windows，web.config 仍会对管理应用的 IIS 进行配置。 默认情况下，IIS 添加对 ASP.NET Core 模块 (ANCM) 的引用。 ANCM 是一个本机 IIS 模块，代替 Kestrel Web 服务器托管应用。 此 web.config 部分与以下 XML 标记类似：

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

应用特定的配置可以通过在 `aspNetCore` 元素中嵌套 `environmentVariables` 元素进行定义。 在此部分中定义的值以环境变量的形式提供给 ASP.NET Core 应用。 在应用启动期间，环境变量会适当加载。

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

## <a name="read-configuration-in-the-app"></a>读取应用中的配置

ASP.NET Core 通过 <xref:Microsoft.Extensions.Configuration.IConfiguration> 接口提供应用配置。 此配置接口应由 Blazor 组件、Blazor 页面以及需要访问配置的其他任何 ASP.NET Core 托管的类请求。 ASP.NET Core 框架使用先前配置的解析配置自动填充此接口。 在 Blazor 页面或组件的 Razor 标记上，可以使用 .razor 文件顶部的 `@inject` 指令注入 `IConfiguration` 对象，如下所示：

```razor
@inject IConfiguration Configuration
```

上面的语句使 `IConfiguration` 对象在 Razor 模板的其余部分中可作为 `Configuration` 变量提供。

可通过指定作为索引器参数查找的配置设置层次结构，读取各个配置设置：

```csharp
var mySetting = Configuration["section1:key0"];
```

你可通过使用 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> 方法检索特定位置的键集合来提取整个配置部分，该方法的语法与前面的示例中检索 section1 的配置的 `GetSection("section1")` 类似。

## <a name="strongly-typed-configuration"></a>强类型配置

通过 Web Forms，可以创建继承自 <xref:System.Configuration.ConfigurationSection> 类型和关联类型的强类型配置类型。 通过 `ConfigurationSection`，可以为这些配置值配置业务规则和流程。

在 ASP.NET Core 中，可以指定接收配置值的类层次结构。 这些类：

* 无需继承自父类。
* 应包括与你要捕获的配置结构的属性和类型引用相匹配的 `public` 属性。

对于前面的 appsettings.js 示例，可以定义以下类来捕获值：

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

可通过将以下行添加到 `Startup.ConfigureServices` 方法来填充此类层次结构：

```csharp
services.Configure<MyConfig>(Configuration);
```

在应用的其余部分，可将输入参数添加到 `IOptions<MyConfig>` 类型的 Razor 模板中的类或 `@inject` 指令，来接收强类型配置设置。 `IOptions<MyConfig>.Value` 属性将生成从配置设置填充的 `MyConfig` 值。

```razor
@inject IOptions<MyConfig> options
@code {
    var MyConfiguration = options.Value;
    var theSetting = MyConfiguration.section1.key0;
}
```

有关选项功能的详细信息，请参阅 [ASP.NET Core 的选项模式](/aspnet/core/fundamentals/configuration/options#options-interfaces)文档。

>[!div class="step-by-step"]
>[上一页](middleware.md)
>[下一页](security-authentication-authorization.md)
