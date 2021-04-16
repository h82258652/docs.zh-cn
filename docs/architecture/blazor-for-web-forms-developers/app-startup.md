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
# <a name="app-startup"></a><span data-ttu-id="a7d58-103">应用启动</span><span class="sxs-lookup"><span data-stu-id="a7d58-103">App startup</span></span>

<span data-ttu-id="a7d58-104">为 ASP.NET 编写的应用程序通常包含一个 `global.asax.cs` 文件，该文件定义 `Application_Start` 事件来确定针对 HTML 呈现和 .NET 处理应配置和提供哪些服务。</span><span class="sxs-lookup"><span data-stu-id="a7d58-104">Applications that are written for ASP.NET typically have a `global.asax.cs` file that defines the `Application_Start` event that controls which services are configured and made available for both HTML rendering and .NET processing.</span></span> <span data-ttu-id="a7d58-105">本章介绍 ASP.NET Core 和 Blazor Server 之间的不同之处。</span><span class="sxs-lookup"><span data-stu-id="a7d58-105">This chapter looks at how things are slightly different with ASP.NET Core and Blazor Server.</span></span>

## <a name="application_start-and-web-forms"></a><span data-ttu-id="a7d58-106">Application_Start 和 Web Forms</span><span class="sxs-lookup"><span data-stu-id="a7d58-106">Application_Start and Web Forms</span></span>

<span data-ttu-id="a7d58-107">默认的 Web Forms `Application_Start` 方法经过多年的验证和改进，可用于处理许多配置任务。</span><span class="sxs-lookup"><span data-stu-id="a7d58-107">The default web forms `Application_Start` method has grown in purpose over years to handle many configuration tasks.</span></span>  <span data-ttu-id="a7d58-108">Visual Studio 2019 中带有默认模板的全新 Web Forms 项目现在包含以下配置逻辑：</span><span class="sxs-lookup"><span data-stu-id="a7d58-108">A fresh web forms project with the default template in Visual Studio 2019 now contains the following configuration logic:</span></span>

- <span data-ttu-id="a7d58-109">`RouteConfig` - 应用程序 URL 路由</span><span class="sxs-lookup"><span data-stu-id="a7d58-109">`RouteConfig` - Application URL routing</span></span>
- <span data-ttu-id="a7d58-110">`BundleConfig` - CSS 与 JavaScript 的捆绑和缩小</span><span class="sxs-lookup"><span data-stu-id="a7d58-110">`BundleConfig` - CSS and JavaScript bundling and minification</span></span>

<span data-ttu-id="a7d58-111">所有文件都驻留在 `App_Start` 文件夹中，并且仅在应用程序启动时运行一次。</span><span class="sxs-lookup"><span data-stu-id="a7d58-111">Each of these individual files resides in the `App_Start` folder and run only once at the start of our application.</span></span>  <span data-ttu-id="a7d58-112">默认项目模板中的 `RouteConfig` 为 Web Forms 添加了 `FriendlyUrlSettings`，允许应用程序 URL 省略 `.ASPX` 文件扩展名。</span><span class="sxs-lookup"><span data-stu-id="a7d58-112">`RouteConfig` in the default project template adds the `FriendlyUrlSettings` for web forms to allow application URLs to omit the `.ASPX` file extension.</span></span>  <span data-ttu-id="a7d58-113">默认模板还包含一个指令，该指令为易记 URL（其名称省略扩展名）提供 `.ASPX` 页面的永久 HTTP 重定向状态代码 (HTTP 301)。</span><span class="sxs-lookup"><span data-stu-id="a7d58-113">The default template also contains a directive that provides permanent HTTP redirect status codes (HTTP 301) for the `.ASPX` pages to the friendly URL with the file name that omits the extension.</span></span>

<span data-ttu-id="a7d58-114">通过 ASP.NET Core 和 Blazor，这些方法可以简化并合并到 `Startup` 类中，或被通用 Web 技术替代。</span><span class="sxs-lookup"><span data-stu-id="a7d58-114">With ASP.NET Core and Blazor, these methods are either simplified and consolidated into the `Startup` class or they are eliminated in favor of common web technologies.</span></span>

## <a name="blazor-server-startup-structure"></a><span data-ttu-id="a7d58-115">Blazor Server 启动结构</span><span class="sxs-lookup"><span data-stu-id="a7d58-115">Blazor Server Startup Structure</span></span>

<span data-ttu-id="a7d58-116">Blazor Server 应用程序在 ASP.NET Core 3.0 或更高版本中受支持。</span><span class="sxs-lookup"><span data-stu-id="a7d58-116">Blazor Server applications reside on top of an ASP.NET Core 3.0 or later version.</span></span>  <span data-ttu-id="a7d58-117">ASP.NET Core Web 应用程序通过应用程序根文件夹的 `Startup.cs` 类中的一对方法进行配置。</span><span class="sxs-lookup"><span data-stu-id="a7d58-117">ASP.NET Core web applications are configured through a pair of methods in the `Startup.cs` class on the root folder of the application.</span></span>  <span data-ttu-id="a7d58-118">启动类的默认内容如下所示：</span><span class="sxs-lookup"><span data-stu-id="a7d58-118">The Startup class's default content is listed below</span></span>

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

<span data-ttu-id="a7d58-119">与 ASP.NET Core 的其余部分一样，也可使用依赖注入原则创建启动类。</span><span class="sxs-lookup"><span data-stu-id="a7d58-119">Like the rest of ASP.NET Core, the Startup class is created with dependency injection principles.</span></span>  <span data-ttu-id="a7d58-120">`IConfiguration` 提供给构造函数并存储在公共属性中，以便以后在配置期间进行访问。</span><span class="sxs-lookup"><span data-stu-id="a7d58-120">The `IConfiguration` is provided to the constructor and stashed in a public property for later access during configuration.</span></span>

<span data-ttu-id="a7d58-121">ASP.NET Core 中引入的 `ConfigureServices` 方法可用于为框架的内置依赖注入容器配置各种 ASP.NET Core 框架服务。</span><span class="sxs-lookup"><span data-stu-id="a7d58-121">The `ConfigureServices` method introduced in ASP.NET Core allows for the various ASP.NET Core framework services to be configured for the framework's built-in dependency injection container.</span></span>  <span data-ttu-id="a7d58-122">各种 `services.Add*` 方法添加了一些服务，这些服务支持身份验证、Razor Pages、MVC 控制器路由、SignalR 以及 Blazor Server 与其他许多服务器的交互等功能。</span><span class="sxs-lookup"><span data-stu-id="a7d58-122">The various `services.Add*` methods add services that enable features such as authentication, razor pages, MVC controller routing, SignalR, and Blazor Server interactions among many others.</span></span>  <span data-ttu-id="a7d58-123">Web Forms 无需此方法，因为对 ASPX、ASCX、ASHX 和 ASMX 文件的分析和处理是通过引用 web.config 配置文件中的 ASP.NET 来定义的。</span><span class="sxs-lookup"><span data-stu-id="a7d58-123">This method was not needed in web forms, as the parsing and handling of the ASPX, ASCX, ASHX, and ASMX files were defined by referencing ASP.NET in the web.config configuration file.</span></span>  <span data-ttu-id="a7d58-124">有关 ASP.NET Core 中的依赖注入的详细信息，请参阅[在线文档](/aspnet/core/fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="a7d58-124">More information about dependency injection in ASP.NET Core is available in the [online documentation](/aspnet/core/fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a7d58-125">`Configure` 方法将 HTTP 管道的概念引入 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="a7d58-125">The `Configure` method introduces the concept of the HTTP pipeline to ASP.NET Core.</span></span>  <span data-ttu-id="a7d58-126">通过此方法，我们对[中间件](middleware.md)进行了从上而下的声明，该中间件将处理发送到应用程序的所有请求。</span><span class="sxs-lookup"><span data-stu-id="a7d58-126">In this method, we declare from top to bottom the [Middleware](middleware.md) that will handle every request sent to our application.</span></span> <span data-ttu-id="a7d58-127">默认配置中的大多数功能之前都是分散在各个 Web Forms 配置文件中的，为便于参阅，现在它们都在位于同一位置。</span><span class="sxs-lookup"><span data-stu-id="a7d58-127">Most of these features in the default configuration were scattered across the web forms configuration files and are now in one place for ease of reference.</span></span>

<span data-ttu-id="a7d58-128">自定义错误页的配置不再位于 `web.config` 文件中，而是配置为在应用程序环境未标记 `Development` 的情况下始终显示。</span><span class="sxs-lookup"><span data-stu-id="a7d58-128">No longer is the configuration of the custom error page placed in a `web.config` file, but now is configured to always be shown if the application environment is not labeled `Development`.</span></span>  <span data-ttu-id="a7d58-129">ASP.NET Core 应用程序现已配置为默认使用 `UseHttpsRedirection` 方法调用为安全页面提供 TLS。</span><span class="sxs-lookup"><span data-stu-id="a7d58-129">Additionally, ASP.NET Core applications are now configured to serve secure pages with TLS by default with the `UseHttpsRedirection` method call.</span></span>

<span data-ttu-id="a7d58-130">接下来，将意外配置方法列入 `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="a7d58-130">Next, an unexpected configuration method is listed to `UseStaticFiles`.</span></span>  <span data-ttu-id="a7d58-131">在 ASP.NET Core 中，必须显式启用对静态文件的请求的支持，并且默认情况下，只有应用的 wwwroot 文件夹中的文件才是公开可寻址的。</span><span class="sxs-lookup"><span data-stu-id="a7d58-131">In ASP.NET Core, support for requests for static files (like JavaScript, CSS, and image files) must be explicitly enabled, and only files in the app's *wwwroot* folder are publicly addressable by default.</span></span>

<span data-ttu-id="a7d58-132">下一行是复制 Web Forms `UseRouting` 的配置选项之一的第一行。</span><span class="sxs-lookup"><span data-stu-id="a7d58-132">The next line is the first that replicates one of the configuration options from web forms: `UseRouting`.</span></span>  <span data-ttu-id="a7d58-133">此方法将 ASP.NET Core 路由器添加到管道，该路由器可在此处或在其考虑路由到的单个文件中进行配置。</span><span class="sxs-lookup"><span data-stu-id="a7d58-133">This method adds the ASP.NET Core router to the pipeline and it can be either configured here or in the individual files that it can consider routing to.</span></span>  <span data-ttu-id="a7d58-134">有关路由配置的详细信息，请参阅[路由部分](pages-routing-layouts.md)。</span><span class="sxs-lookup"><span data-stu-id="a7d58-134">More information about routing configuration can be found in the [Routing section](pages-routing-layouts.md).</span></span>

<span data-ttu-id="a7d58-135">此方法中的最后一个语句定义 ASP.NET Core 侦听的终结点。</span><span class="sxs-lookup"><span data-stu-id="a7d58-135">The final statement in this method defines the endpoints that ASP.NET Core is listening on.</span></span>  <span data-ttu-id="a7d58-136">这些路由是可以在 Web 服务器上访问的 Web 可访问位置，并且可以接收由 .NET 处理并返回给你的某些内容。</span><span class="sxs-lookup"><span data-stu-id="a7d58-136">These routes are the web accessible locations that you can access on the web server and receive some content handled by .NET and returned to you.</span></span>  <span data-ttu-id="a7d58-137">第一个项 `MapBlazorHub` 配置 SignalR 集线器，使其可用于为服务器提供实时且持久的连接，以便在其中处理 Blazor 组件的状态和呈现。</span><span class="sxs-lookup"><span data-stu-id="a7d58-137">The first entry, `MapBlazorHub` configures a SignalR hub for use in providing the real-time and persistent connection to the server where the state and rendering of Blazor components is handled.</span></span>  <span data-ttu-id="a7d58-138">`MapFallbackToPage` 方法调用指示启动 Blazor 应用程序的页面的 Web 可访问的位置，还将应用程序配置为处理来自客户端的深层链接请求。</span><span class="sxs-lookup"><span data-stu-id="a7d58-138">The `MapFallbackToPage` method call indicates the web-accessible location of the page that starts the Blazor application and also configures the application to handle deep-linking requests from the client-side.</span></span>  <span data-ttu-id="a7d58-139">打开浏览器并直接导航到应用程序中 Blazor 处理的路由时（例如默认模板中的 `/counter`），你将看到正是这一功能在起作用。</span><span class="sxs-lookup"><span data-stu-id="a7d58-139">You will see this feature at work if you open a browser and navigate directly to Blazor handled route in your application, such as `/counter` in the default project template.</span></span> <span data-ttu-id="a7d58-140">请求由 _Host.cshtml 回退页面处理，该页面随后会运行 Blazor 路由器并呈现路由器页面。</span><span class="sxs-lookup"><span data-stu-id="a7d58-140">The request gets handled by the *_Host.cshtml* fallback page, which then runs the Blazor router and renders the counter page.</span></span>

## <a name="upgrading-the-bundleconfig-process"></a><span data-ttu-id="a7d58-141">升级 BundleConfig 进程</span><span class="sxs-lookup"><span data-stu-id="a7d58-141">Upgrading the BundleConfig Process</span></span>

<span data-ttu-id="a7d58-142">捆绑 CSS 样式表和 JavaScript 文件等资产的技术已取得显著进步，同时其他技术也在提供不断改进的工具和技术来管理这些资源。</span><span class="sxs-lookup"><span data-stu-id="a7d58-142">Technologies for bundling assets like CSS stylesheets and JavaScript files have evolved significantly, with other technologies providing quickly evolving tools and techniques for managing these resources.</span></span>  <span data-ttu-id="a7d58-143">为此，建议使用 Grunt、Gulp 或 WebPack 等 Node 命令行工具打包静态资产。</span><span class="sxs-lookup"><span data-stu-id="a7d58-143">To this end, we recommend using a Node command-line tool such as Grunt / Gulp / WebPack to package your static assets.</span></span>

<span data-ttu-id="a7d58-144">可以将 Grunt、Gulp 和 WebPack 命令行工具以及相关配置添加到你的应用程序，ASP.NET Core 将在应用程序生成过程中忽略这些文件。</span><span class="sxs-lookup"><span data-stu-id="a7d58-144">The Grunt, Gulp, and WebPack command-line tools and their associated configurations can be added to your application and ASP.NET Core will quietly ignore those files during the application build process.</span></span>  <span data-ttu-id="a7d58-145">可通过在项目文件中添加 `Target` 来添加运行其任务的调用。该项目文件的语法与以下语法类似，会触发 gulp 脚本以及该脚本中的 `min` 目标</span><span class="sxs-lookup"><span data-stu-id="a7d58-145">You can add a call to run their tasks by adding a `Target` inside your project file with syntax similar to the following that would trigger a gulp script and the `min` target inside that script</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="a7d58-146">有关管理 CSS 和 JavaScript 文件的两种策略的详细信息，请参阅[捆绑和缩小 ASP.NET Core 中的静态资产](/aspnet/core/client-side/bundling-and-minification)文档。</span><span class="sxs-lookup"><span data-stu-id="a7d58-146">More details about both strategies to manage your CSS and JavaScript files are available in the [Bundle and minify static assets in ASP.NET Core](/aspnet/core/client-side/bundling-and-minification) documentation.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="a7d58-147">[上一页](project-structure.md)
>[下一页](components.md)</span><span class="sxs-lookup"><span data-stu-id="a7d58-147">[Previous](project-structure.md)
[Next](components.md)</span></span>
