---
title: 从 ASP.NET Web Forms 迁移到 Blazor
description: 了解如何将现有的 ASP.NET Web Forms 应用迁移到 Blazor。
author: twsouthwick
ms.author: tasou
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: 893b6f851681ec540629fe160749b2622b6d5440
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509827"
---
# <a name="migrate-from-aspnet-web-forms-to-blazor"></a>从 ASP.NET Web Forms 迁移到 Blazor

将基本代码从 ASP.NET Web Forms 迁移到 Blazor 是一项需要规划的耗时任务。 本章概述了此过程。 为了简化过渡，应确保应用遵循 N 层体系结构，将应用模型（本例中为 Web Forms）与业务逻辑分开。 通过这种层之间的逻辑分离，你可以清楚地确定需要迁移到 .NET Core 和 Blazor 的内容。

本示例使用 [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) 上提供的 eShop 应用。 eShop 是一种目录服务，它通过窗体输入和验证提供 CRUD 功能。

为什么要将可正常运行的应用迁移到 Blazor？ 很多时候，并无必要。 即使许多年后，我们还是会支持 ASP.NET Web Forms。 但是，Blazor 提供的许多功能仅在已迁移的应用上受支持。 此类功能包括：

- `Span<T>` 等框架的性能改进
- 能够作为 WebAssembly 运行
- 对 Linux 和 macOS 的跨平台支持
- 应用本地部署或共享框架部署，不影响其他应用

如果这些功能或其他新功能足够吸引你，那么迁移应用就有价值。 迁移可以采取不同的形式；可以迁移整个应用，也可以只迁移需要更改的某些终结点。 迁移决策最终取决于开发人员要解决的业务问题。

## <a name="server-side-versus-client-side-hosting"></a>服务器端与客户端托管

如[托管模型](hosting-models.md)一章中所述，可以通过两种不同的方式托管 Blazor 应用：服务器端和客户端。 服务器端模型使用 ASP.NET Core SignalR 连接来管理 DOM 更新，同时在服务器上运行所有实际代码。 客户端模型在浏览器中作为 WebAssembly 运行，并且不需要服务器连接。 哪种模型最适合某个特定应用可能会受到很多差异的影响：

- 目前，作为 WebAssembly 运行尚不支持所有功能（如线程）
- 客户端和服务器之间的聊天通信可能会导致服务器端模式出现延迟问题
- 访问数据库和内部服务或受保护的服务需要带有客户端托管的单独服务

在撰写本文时，服务器端模型更接近于 Web Forms。 本章的大部分内容重点介绍服务器端托管模型，因为它已经可以投入生产。

## <a name="create-a-new-project"></a>创建新项目

最初的迁移步骤是创建一个新项目。 此项目类型基于 .NET 的 SDK 样式项目，并简化了以前项目格式中使用的许多样本。 有关更多详细信息，请参阅[项目结构](project-structure.md)一章。

创建项目后，安装上一个项目中使用的库。 在较早的 Web Forms 项目中，你可能已经使用 packages.config 文件列出了所需的 NuGet 包。 在新的 SDK 样式项目中，项目文件中的 packages.config 已替换为 `<PackageReference>` 元素。 这种方法的好处是，所有依赖项都以可传递的方式安装。 你只需列出你关心的顶级依赖项。

你要使用的许多依赖项都适用于 .NET，包括实体框架 6 和 log4net。 如果没有可用的 .NET 或 .NET Standard 版本，通常可以使用 .NET Framework 版本。 你的里程可能会有所不同。 使用任何在 .NET 中不可用的 API 都会导致运行时错误。 如果有这种包，Visual Studio 会通知你。 在解决方案资源管理器中，项目的“引用”节点上会出现一个黄色图标。

在基于 Blazor 的 eShop 项目中，你可以看到已安装的包。 以前，packages.config 文件会列出项目中使用的每一个包，导致文件长达近 50 行。 下面是 packages.config 的一个代码片段：

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  ...
  <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Web" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls.Core" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.MSAjax" version="5.0.0" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.WebForms" version="5.0.0" targetFramework="net472" />
  ...
  <package id="System.Memory" version="4.5.1" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Channels" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Tasks.Extensions" version="4.5.1" targetFramework="net472" />
  <package id="WebGrease" version="1.6.0" targetFramework="net472" />
</packages>
```

`<packages>` 元素包含所有必要的依赖项。 你很难确定要添加其中哪些包，因为这些包都是必需的。 某些 `<package>` 元素只是为了满足所需的依赖项需求而列出。

Blazor 项目在项目文件的 `<ItemGroup>` 元素中列出了所需的依赖项：

```xml
<ItemGroup>
    <PackageReference Include="Autofac" Version="4.9.3" />
    <PackageReference Include="EntityFramework" Version="6.4.4" />
    <PackageReference Include="log4net" Version="2.0.12" />
    <PackageReference Include="Microsoft.Extensions.Logging.Log4Net.AspNetCore" Version="2.2.12" />
</ItemGroup>
```

有一个 NuGet 包简化了 Web Forms 开发人员的生活，那就是 [Windows 兼容包](../../core/porting/windows-compat-pack.md)。 虽然 .NET 是跨平台的，但有些功能只能在 Windows 上使用。 通过安装兼容包，可以使用 Windows 特定功能。 此类功能的示例包括注册表、WMI 和目录服务。 该包添加了大约 20,000 个 API，并激活了许多你可能已经熟悉的服务。 eShop 项目不需要兼容包；但如果你的项目使用 Windows 特定功能，该包可以简化迁移工作。

## <a name="enable-startup-process"></a>启用启动过程

从 Web Forms 开始，Blazor 的启动过程发生了变化，它遵循其他 ASP.NET Core 服务的类似设置。 当在服务器端托管时，Blazor 组件作为正常 ASP.NET Core 应用的一部分运行。 通过 WebAssembly 在浏览器中托管时，Blazor 组件使用类似的托管模型。 不同之处在于，这些组件作为独立于任何后端进程的服务运行。 无论采用哪种方式，启动过程都差不多。

Global.asax.cs 文件是 Web Forms 项目的默认启动页面。 在 eShop 项目中，此文件将配置控制反转 (IoC) 容器，并处理应用或请求的各种生命周期事件。 其中一些事件通过中间件处理（例如 `Application_BeginRequest`）。 其他事件则需要通过依赖关系注入 (DI) 来覆盖特定服务。

举个例子，eShop 的 Global.asax.cs 文件包含以下代码：

```csharp
public class Global : HttpApplication, IContainerProviderAccessor
{
    private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

    static IContainerProvider _containerProvider;
    IContainer container;

    public IContainerProvider ContainerProvider
    {
        get { return _containerProvider; }
    }

    protected void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on app startup
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ConfigureContainer();
        ConfigDataBase();
    }

    /// <summary>
    /// Track the machine name and the start time for the session inside the current session
    /// </summary>
    protected void Session_Start(Object sender, EventArgs e)
    {
        HttpContext.Current.Session["MachineName"] = Environment.MachineName;
        HttpContext.Current.Session["SessionStartTime"] = DateTime.Now;
    }

    /// <summary>
    /// https://autofaccn.readthedocs.io/en/latest/integration/webforms.html
    /// </summary>
    private void ConfigureContainer()
    {
        var builder = new ContainerBuilder();
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
        builder.RegisterModule(new ApplicationModule(mockData));
        container = builder.Build();
        _containerProvider = new ContainerProvider(container);
    }

    private void ConfigDataBase()
    {
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);

        if (!mockData)
        {
            Database.SetInitializer<CatalogDBContext>(container.Resolve<CatalogDBInitializer>());
        }
    }

    protected void Application_BeginRequest(object sender, EventArgs e)
    {
        //set the property to our new object
        LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper();

        LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo();

        _log.Debug("Application_BeginRequest");
    }
}
```

前面的文件变成服务器端 Blazor 中的 `Startup` 类：

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    public IConfiguration Configuration { get; }

    public IWebHostEnvironment Env { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();
        services.AddServerSideBlazor();

        if (Configuration.GetValue<bool>("UseMockData"))
        {
            services.AddSingleton<ICatalogService, CatalogServiceMock>();
        }
        else
        {
            services.AddScoped<ICatalogService, CatalogService>();
            services.AddScoped<IDatabaseInitializer<CatalogDBContext>, CatalogDBInitializer>();
            services.AddSingleton<CatalogItemHiLoGenerator>();
            services.AddScoped(_ => new CatalogDBContext(Configuration.GetConnectionString("CatalogDBContext")));
        }
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddLog4Net("log4Net.xml");

        if (Env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
        }

        // Middleware for Application_BeginRequest
        app.Use((ctx, next) =>
        {
            LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper(ctx);
            LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo(ctx);
            return next();
        });

        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapBlazorHub();
            endpoints.MapFallbackToPage("/_Host");
        });

        ConfigDataBase(app);
    }

    private void ConfigDataBase(IApplicationBuilder app)
    {
        using (var scope = app.ApplicationServices.CreateScope())
        {
            var initializer = scope.ServiceProvider.GetService<IDatabaseInitializer<CatalogDBContext>>();

            if (initializer != null)
            {
                Database.SetInitializer(initializer);
            }
        }
    }
}
```

你可能会注意到，Web Forms 的一个重要变化是 DI 的重要性。 DI 一直是 ASP.NET Core 设计的指导原则。 它支持自定义 ASP.NET Core 框架的几乎所有方面。 它甚至还有一个内置的服务提供程序，可用于许多场景。 如果需要更多自定义，许多社区项目可以提供支持。 例如，你可以结转第三方 DI 库投资。

在原始 eShop 应用中，有一些用于会话管理的配置。 由于服务器端 Blazor 使用 ASP.NET Core SignalR 进行通信，因此不支持会话状态，因为连接可能独立于 HTTP 上下文。 使用会话状态的应用需要重新架构，才能作为 Blazor 应用运行。

有关应用启动的详细信息，请参阅[应用启动](app-startup.md)。

## <a name="migrate-http-modules-and-handlers-to-middleware"></a>将 HTTP 模块和处理程序迁移到中间件

HTTP 模块和处理程序是 Web Forms 中的常见模式，用于控制 HTTP 请求管道。 可以注册实现 `IHttpModule` 或 `IHttpHandler` 的类，由它们来处理传入请求。 Web Forms 在 web.config 文件中配置模块和处理程序。 而且，Web Forms 在很大程度上取决于应用生命周期事件处理。 ASP.NET Core 则使用中间件。 中间件在 `Startup` 类的 `Configure` 方法中注册。 中间件的执行顺序由注册顺序决定。

在[启用启动过程](#enable-startup-process)部分中，Web Forms 引发了一个生命周期事件作为 `Application_BeginRequest` 方法。 此事件在 ASP.NET Core 中不可用。 实现此行为的一种方法是实现中间件，如 Startup.cs 文件示例中所示。 此中间件执行相同的逻辑，然后将控制权转移到中间件管道中的下一个处理程序。

有关迁移模块和处理程序的详细信息，请参阅[将 HTTP 处理程序和模块迁移到 ASP.NET Core 中间件](/aspnet/core/migration/http-modules)。

## <a name="migrate-static-files"></a>迁移静态文件

若要提供静态文件（例如 HTML、CSS、图像和 JavaScript），必须由中间件公开这些文件。 通过调用 `UseStaticFiles` 方法，可以从 Web 根路径中提供静态文件。 默认的 Web 根目录是 wwwroot，但可以对其进行自定义。 如 eShop 的 `Startup` 类的 `Configure` 方法中包含的 Web 根目录：

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    ...
}
```

eShop 项目可以实现基本的静态文件访问。 有许多自定义项可用于静态文件访问。 有关启用默认文件或文件浏览器的信息，请参阅 [ASP.NET Core 中的静态文件](/aspnet/core/fundamentals/static-files)。

## <a name="migrate-runtime-bundling-and-minification-setup"></a>迁移运行时捆绑和缩小设置

捆绑和缩小是两种性能优化技术，用于减少服务器检索某些文件类型的请求的数量和大小。 JavaScript 和 CSS 在被发送到客户端之前通常会经过某种形式的捆绑或缩小。 在 ASP.NET Web Forms 中，这些优化在运行时进行处理。 优化约定定义为 App_Start/BundleConfig.cs 文件。 ASP.NET Core 采用更具声明性的方法。 一个文件将列出要缩小的文件以及特定的缩小设置。

有关捆绑和缩小的详细信息，请参阅[在 ASP.NET Core 中捆绑和缩小静态资产](/aspnet/core/client-side/bundling-and-minification)。

## <a name="migrate-aspx-pages"></a>迁移 ASPX 页面

Web Forms 应用中的页面是扩展名为 .aspx 的文件。 Web Forms 页面通常可以映射到 Blazor 中的组件。 Blazor 组件是在扩展名为 .razor 的文件中创作的。 在 eShop 项目中，需要将五个页面转换为 Razor 页面。

例如，详细信息视图包括 Web Forms 项目中的三个文件：Details.aspx、Details.aspx.cs 和 Details.aspx.designer.cs。 转换为 Blazor 时，代码隐藏和标记会合并到 Details.razor 中。 Razor 编译（等效于 .designer.cs 文件中的内容）存储在 obj 目录中，默认情况下在解决方案资源管理器中不可见。 Web Forms 页面包含以下标记：

```aspx-csharp
<%@ Page Title="Details" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Details.aspx.cs" Inherits="eShopLegacyWebForms.Catalog.Details" %>

<asp:Content ID="Details" ContentPlaceHolderID="MainContent" runat="server">
    <h2 class="esh-body-title">Details</h2>

    <div class="container">
        <div class="row">
            <asp:Image runat="server" CssClass="col-md-6 esh-picture" ImageUrl='<%#"/Pics/" + product.PictureFileName%>' />
            <dl class="col-md-6 dl-horizontal">
                <dt>Name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Name%>' />
                </dd>

                <dt>Description
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Description%>' />
                </dd>

                <dt>Brand
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogBrand.Brand%>' />
                </dd>

                <dt>Type
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogType.Type%>' />
                </dd>
                <dt>Price
                </dt>

                <dd>
                    <asp:Label CssClass="esh-price" runat="server" Text='<%#product.Price%>' />
                </dd>

                <dt>Picture name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.PictureFileName%>' />
                </dd>

                <dt>Stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.AvailableStock%>' />
                </dd>

                <dt>Restock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.RestockThreshold%>' />
                </dd>

                <dt>Max stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.MaxStockThreshold%>' />
                </dd>

            </dl>
        </div>

        <div class="form-actions no-color esh-link-list">
            <a runat="server" href='<%# GetRouteUrl("EditProductRoute", new {id =product.Id}) %>' class="esh-link-item">Edit
            </a>
            |
            <a runat="server" href="~" class="esh-link-item">Back to list
            </a>
        </div>

    </div>
</asp:Content>
```

以上标记的代码隐藏包括以下代码：

```csharp
using eShopLegacyWebForms.Models;
using eShopLegacyWebForms.Services;
using log4net;
using System;
using System.Web.UI;

namespace eShopLegacyWebForms.Catalog
{
    public partial class Details : System.Web.UI.Page
    {
        private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        protected CatalogItem product;

        public ICatalogService CatalogService { get; set; }

        protected void Page_Load(object sender, EventArgs e)
        {
            var productId = Convert.ToInt32(Page.RouteData.Values["id"]);
            _log.Info($"Now loading... /Catalog/Details.aspx?id={productId}");
            product = CatalogService.FindCatalogItem(productId);

            this.DataBind();
        }
    }
}
```

转换为 Blazor 时，Web Forms 页面会转换为以下代码：

```razor
@page "/Catalog/Details/{id:int}"
@inject ICatalogService CatalogService
@inject ILogger<Details> Logger

<h2 class="esh-body-title">Details</h2>

<div class="container">
    <div class="row">
        <img class="col-md-6 esh-picture" src="@($"/Pics/{_item.PictureFileName}")">

        <dl class="col-md-6 dl-horizontal">
            <dt>
                Name
            </dt>

            <dd>
                @_item.Name
            </dd>

            <dt>
                Description
            </dt>

            <dd>
                @_item.Description
            </dd>

            <dt>
                Brand
            </dt>

            <dd>
                @_item.CatalogBrand.Brand
            </dd>

            <dt>
                Type
            </dt>

            <dd>
                @_item.CatalogType.Type
            </dd>
            <dt>
                Price
            </dt>

            <dd>
                @_item.Price
            </dd>

            <dt>
                Picture name
            </dt>

            <dd>
                @_item.PictureFileName
            </dd>

            <dt>
                Stock
            </dt>

            <dd>
                @_item.AvailableStock
            </dd>

            <dt>
                Restock
            </dt>

            <dd>
                @_item.RestockThreshold
            </dd>

            <dt>
                Max stock
            </dt>

            <dd>
                @_item.MaxStockThreshold
            </dd>

        </dl>
    </div>

    <div class="form-actions no-color esh-link-list">
        <a href="@($"/Catalog/Edit/{_item.Id}")" class="esh-link-item">
            Edit
        </a>
        |
        <a href="/" class="esh-link-item">
            Back to list
        </a>
    </div>

</div>

@code {
    private CatalogItem _item;

    [Parameter]
    public int Id { get; set; }

    protected override void OnInitialized()
    {
        Logger.LogInformation("Now loading... /Catalog/Details/{Id}", Id);

        _item = CatalogService.FindCatalogItem(Id);
    }
}
```

请注意，代码和标记在同一文件中。 使用 `@inject` 属性可以访问任何必需的服务。 根据 `@page` 指令，可以在 `Catalog/Details/{id}` 路由中访问此页面。 该路由的 `{id}` 占位符的值已限制为整数。 如[路由](pages-routing-layouts.md)部分中所述，Razor 组件与 Web Forms 不同，它会显式声明其路由以及包含的所有参数。 许多 Web Forms 控件在 Blazor 中可能没有完全对应的控件。 但通常会有一个等效的 HTML 代码片段可以达到相同目的。 例如，可以将 `<asp:Label />` 控件替换为 HTML `<label>` 元素。

### <a name="model-validation-in-blazor"></a>Blazor 中的模型验证

如果你的 Web Forms 代码包含验证，那么几乎不需要更改就可以传输大部分内容。 在 Blazor 中运行的好处是可以运行相同的验证逻辑，而无需自定义 JavaScript。 数据注释使模型验证变得简单易行。

例如，Create.aspx 页面包含一个带验证的数据输入窗体。 代码片段示例如下所示：

```aspx
<div class="form-group">
    <label class="control-label col-md-2">Name</label>
    <div class="col-md-3">
        <asp:TextBox ID="Name" runat="server" CssClass="form-control"></asp:TextBox>
        <asp:RequiredFieldValidator runat="server" ControlToValidate="Name" Display="Dynamic"
            CssClass="field-validation-valid text-danger" ErrorMessage="The Name field is required." />
    </div>
</div>
```

在 Blazor 中，Create.razor 文件中提供了等效标记：

```razor
<EditForm Model="_item" OnValidSubmit="@...">
    <DataAnnotationsValidator />

    <div class="form-group">
        <label class="control-label col-md-2">Name</label>
        <div class="col-md-3">
            <InputText class="form-control" @bind-Value="_item.Name" />
            <ValidationMessage For="(() => _item.Name)" />
        </div>
    </div>

    ...
</EditForm>
```

`EditForm` 上下文包括验证支持，可以包装在输入周围。 数据注释是添加验证的常用方法。 可以通过 `DataAnnotationsValidator` 组件添加此类验证支持。 有关此机制的详细信息，请参阅 [ASP.NET Core Blazor 窗体和验证](/aspnet/core/blazor/forms-validation)。

## <a name="migrate-configuration"></a>迁移配置

在 Web Forms 项目中，配置数据通常存储在 web.config 文件中。 可使用 `ConfigurationManager` 访问配置数据。 通常需要使用服务来分析对象。 在 .NET Framework 4.7.2 中，已通过 `ConfigurationBuilders` 向配置添加可组合性。 这些生成器允许开发人员为配置添加各种源，然后在运行时组合这些源，以检索必要的值。

ASP.NET Core 引入了一种灵活的配置系统，该系统允许你定义应用和部署所使用的配置源。 你在 Web Forms 应用中可能使用的 `ConfigurationBuilder` 基础结构是按照 ASP.NET Core 配置系统中使用的概念建模的。

以下代码片段演示了 Web Forms eShop 项目如何使用 web.config 存储配置值：

```xml
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <connectionStrings>
    <add name="CatalogDBContext" connectionString="Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
  </connectionStrings>
  <appSettings>
    <add key="UseMockData" value="true" />
    <add key="UseCustomizationData" value="false" />
  </appSettings>
</configuration>
```

诸如数据库连接字符串之类的机密通常存储在 web.config 中。这些机密不可避免地保存在不安全的位置，例如源代码管理。 使用 ASP.NET Core 上的 Blazor 时，上面基于 XML 的配置将替换为以下 JSON：

```json
{
  "ConnectionStrings": {
    "CatalogDBContext": "Data Source=(localdb)\\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;"
  },
  "UseMockData": true,
  "UseCustomizationData": false
}
```

JSON 是默认配置格式；但是，ASP.NET Core 也支持许多其他格式，包括 XML。 另外还有几种社​​区支持的格式。

Blazor 项目的 `Startup` 类中的构造函数通过一种称为构造函数注入的 DI 技术接受 `IConfiguration` 实例：

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    ...
}
```

默认情况下，环境变量、JSON 文件（appsettings.json 和 appsettings.{Environment}.json）和命令行选项在配置对象中注册为有效的配置源。 可以通过 `Configuration[key]` 访问配置源。 一种更高级的技术是使用选项模式将配置数据绑定到对象。 有关配置和选项模式的详细信息，请分别参阅 [ASP.NET Core 中的配置](/aspnet/core/fundamentals/configuration/)和 [ASP.NET Core 中的选项模式](/aspnet/core/fundamentals/configuration/options)。

## <a name="migrate-data-access"></a>迁移数据访问

数据访问是所有应用很重要的一个方面。 eShop 项目将目录信息存储在数据库中，并使用实体框架 (EF) 6 检索数据。 由于 .NET 5.0 支持 EF 6，因此该项目可以继续使用它。

以下与 EF 相关的更改对于 eShop 是必需的：

- 在 .NET Framework 中，`DbContext` 对象接受格式为 name=ConnectionString 的字符串，并使用 `ConfigurationManager.AppSettings[ConnectionString]` 中的连接字符串进行连接。 .NET Core 不支持这样做， 你必须提供连接字符串。
- 以同步方式访问数据库。 虽然这种方式可行，但可伸缩性可能会受到影响。 此逻辑应迁移到异步模式。

虽然无法对数据集绑定提供本机支持，但是，Blazor 在 Razor 页面中提供 C# 支持，从而提供出色的灵活性和强大的功能。 例如，你可以执行计算并显示结果。 有关 Blazor 中数据模式的详细信息，请参阅[数据访问](data.md)一章。

## <a name="architectural-changes"></a>体系结构更改

最后，在迁移到 Blazor 时，需要考虑一些重要的体系结构差异。 其中许多更改适用于基于 .NET Core 或 ASP.NET Core 的任何产品。

由于 Blazor 是基于 .NET Core 构建的，因此请注意确保支持在 .NET Core 上运行。 一些重大更改包括删除以下功能：

- 多个 AppDomain
- 远程处理
- 代码访问安全性 (CAS)
- 安全透明度

可通过多种技术来确定支持在 .NET Core 上运行所需进行的更改；有关这些技术的详细信息，请参阅[将代码从 .NET Framework 移植到 .NET Core](../../core/porting/index.md)。

ASP.NET Core 是 ASP.NET 的重构版本，它有一些最初看起来并不明显的更改。 其主要更改包括：

- 没有同步上下文，这意味着没有 `HttpContext.Current`、`Thread.CurrentPrincipal` 或其他静态访问器
- 无卷影复制
- 无请求队列

ASP.NET Core 中的许多操作都是异步的，这使得 I/O 绑定的任务更容易卸载。 切勿使用 `Task.Wait()` 或 `Task.GetResult()` 进行阻塞，这样会迅速耗尽线程池资源。

## <a name="migration-conclusion"></a>迁移结束语

至此，你已经看到了许多如何将 Web Forms 项目迁移到 Blazor 的示例。 有关完整示例，请参阅 [eShopOnBlazor](https://github.com/dotnet-architecture/eShopOnBlazor) 项目。

>[!div class="step-by-step"]
>[上一页](security-authentication-authorization.md)
