---
title: 应用启动
description: 了解如何定义应用的启动逻辑。
author: csharpfritz
ms.author: jefritz
ms.date: 11/20/2020
ms.openlocfilehash: d812079f84f67409334d07c4c10c5577446503be
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509697"
---
# <a name="app-startup"></a>应用启动

为 ASP.NET 编写的应用程序通常包含一个 `global.asax.cs` 文件，该文件定义 `Application_Start` 事件来确定针对 HTML 呈现和 .NET 处理应配置和提供哪些服务。 本章介绍 ASP.NET Core 和 Blazor Server 之间的不同之处。

## <a name="application_start-and-web-forms"></a>Application_Start 和 Web Forms

默认的 Web Forms `Application_Start` 方法经过多年的验证和改进，可用于处理许多配置任务。  Visual Studio 2019 中带有默认模板的全新 Web Forms 项目现在包含以下配置逻辑：

- `RouteConfig` - 应用程序 URL 路由
- `BundleConfig` - CSS 与 JavaScript 的捆绑和缩小

所有文件都驻留在 `App_Start` 文件夹中，并且仅在应用程序启动时运行一次。  默认项目模板中的 `RouteConfig` 为 Web Forms 添加了 `FriendlyUrlSettings`，允许应用程序 URL 省略 `.ASPX` 文件扩展名。  默认模板还包含一个指令，该指令为易记 URL（其名称省略扩展名）提供 `.ASPX` 页面的永久 HTTP 重定向状态代码 (HTTP 301)。

通过 ASP.NET Core 和 Blazor，这些方法可以简化并合并到 `Startup` 类中，或被通用 Web 技术替代。

## <a name="blazor-server-startup-structure"></a>Blazor Server 启动结构

Blazor Server 应用程序在 ASP.NET Core 3.0 或更高版本中受支持。  ASP.NET Core Web 应用程序通过应用程序根文件夹的 `Startup.cs` 类中的一对方法进行配置。  启动类的默认内容如下所示：

```csharp
public class Startup
{
  public Startup(IConfiguration configuration)
  {
    Configuration = configuration;
  }

  public IConfiguration Configuration { get; }

  // This method gets called by the runtime. Use this method to add services to the container.
  // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
  public void ConfigureServices(IServiceCollection services)
  {
    services.AddRazorPages();
    services.AddServerSideBlazor();
    services.AddSingleton<WeatherForecastService>();
  }

  // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
    if (env.IsDevelopment())
    {
      app.UseDeveloperExceptionPage();
    }
    else
    {
      app.UseExceptionHandler("/Error");
      // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
      app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
      endpoints.MapBlazorHub();
      endpoints.MapFallbackToPage("/_Host");
   });
  }
 }
```

与 ASP.NET Core 的其余部分一样，也可使用依赖注入原则创建启动类。  `IConfiguration` 提供给构造函数并存储在公共属性中，以便以后在配置期间进行访问。

ASP.NET Core 中引入的 `ConfigureServices` 方法可用于为框架的内置依赖注入容器配置各种 ASP.NET Core 框架服务。  各种 `services.Add*` 方法添加了一些服务，这些服务支持身份验证、Razor Pages、MVC 控制器路由、SignalR 以及 Blazor Server 与其他许多服务器的交互等功能。  Web Forms 无需此方法，因为对 ASPX、ASCX、ASHX 和 ASMX 文件的分析和处理是通过引用 web.config 配置文件中的 ASP.NET 来定义的。  有关 ASP.NET Core 中的依赖注入的详细信息，请参阅[在线文档](/aspnet/core/fundamentals/dependency-injection)。

`Configure` 方法将 HTTP 管道的概念引入 ASP.NET Core。  通过此方法，我们对[中间件](middleware.md)进行了从上而下的声明，该中间件将处理发送到应用程序的所有请求。 默认配置中的大多数功能之前都是分散在各个 Web Forms 配置文件中的，为便于参阅，现在它们都在位于同一位置。

自定义错误页的配置不再位于 `web.config` 文件中，而是配置为在应用程序环境未标记 `Development` 的情况下始终显示。  ASP.NET Core 应用程序现已配置为默认使用 `UseHttpsRedirection` 方法调用为安全页面提供 TLS。

接下来，将意外配置方法列入 `UseStaticFiles`。  在 ASP.NET Core 中，必须显式启用对静态文件的请求的支持，并且默认情况下，只有应用的 wwwroot 文件夹中的文件才是公开可寻址的。

下一行是复制 Web Forms `UseRouting` 的配置选项之一的第一行。  此方法将 ASP.NET Core 路由器添加到管道，该路由器可在此处或在其考虑路由到的单个文件中进行配置。  有关路由配置的详细信息，请参阅[路由部分](pages-routing-layouts.md)。

此方法中的最后一个语句定义 ASP.NET Core 侦听的终结点。  这些路由是可以在 Web 服务器上访问的 Web 可访问位置，并且可以接收由 .NET 处理并返回给你的某些内容。  第一个项 `MapBlazorHub` 配置 SignalR 集线器，使其可用于为服务器提供实时且持久的连接，以便在其中处理 Blazor 组件的状态和呈现。  `MapFallbackToPage` 方法调用指示启动 Blazor 应用程序的页面的 Web 可访问的位置，还将应用程序配置为处理来自客户端的深层链接请求。  打开浏览器并直接导航到应用程序中 Blazor 处理的路由时（例如默认模板中的 `/counter`），你将看到正是这一功能在起作用。 请求由 _Host.cshtml 回退页面处理，该页面随后会运行 Blazor 路由器并呈现路由器页面。

## <a name="upgrading-the-bundleconfig-process"></a>升级 BundleConfig 进程

捆绑 CSS 样式表和 JavaScript 文件等资产的技术已取得显著进步，同时其他技术也在提供不断改进的工具和技术来管理这些资源。  为此，建议使用 Grunt、Gulp 或 WebPack 等 Node 命令行工具打包静态资产。

可以将 Grunt、Gulp 和 WebPack 命令行工具以及相关配置添加到你的应用程序，ASP.NET Core 将在应用程序生成过程中忽略这些文件。  可通过在项目文件中添加 `Target` 来添加运行其任务的调用。该项目文件的语法与以下语法类似，会触发 gulp 脚本以及该脚本中的 `min` 目标

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp min" />
</Target>
```

有关管理 CSS 和 JavaScript 文件的两种策略的详细信息，请参阅[捆绑和缩小 ASP.NET Core 中的静态资产](/aspnet/core/client-side/bundling-and-minification)文档。

>[!div class="step-by-step"]
>[上一页](project-structure.md)
>[下一页](components.md)
