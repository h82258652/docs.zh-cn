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
# <a name="migrate-from-aspnet-web-forms-to-blazor"></a><span data-ttu-id="3b2f2-103">从 ASP.NET Web Forms 迁移到 Blazor</span><span class="sxs-lookup"><span data-stu-id="3b2f2-103">Migrate from ASP.NET Web Forms to Blazor</span></span>

<span data-ttu-id="3b2f2-104">将基本代码从 ASP.NET Web Forms 迁移到 Blazor 是一项需要规划的耗时任务。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-104">Migrating a code base from ASP.NET Web Forms to Blazor is a time-consuming task that requires planning.</span></span> <span data-ttu-id="3b2f2-105">本章概述了此过程。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-105">This chapter outlines the process.</span></span> <span data-ttu-id="3b2f2-106">为了简化过渡，应确保应用遵循 N 层体系结构，将应用模型（本例中为 Web Forms）与业务逻辑分开。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-106">Something that can ease the transition is to ensure the app adheres to an *N-tier* architecture, wherein the app model (in this case, Web Forms) is separate from the business logic.</span></span> <span data-ttu-id="3b2f2-107">通过这种层之间的逻辑分离，你可以清楚地确定需要迁移到 .NET Core 和 Blazor 的内容。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-107">This logical separation of layers makes it clear what needs to move to .NET Core and Blazor.</span></span>

<span data-ttu-id="3b2f2-108">本示例使用 [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) 上提供的 eShop 应用。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-108">For this example, the eShop app available on [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) is used.</span></span> <span data-ttu-id="3b2f2-109">eShop 是一种目录服务，它通过窗体输入和验证提供 CRUD 功能。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-109">eShop is a catalog service that provides CRUD capabilities via form entry and validation.</span></span>

<span data-ttu-id="3b2f2-110">为什么要将可正常运行的应用迁移到 Blazor？</span><span class="sxs-lookup"><span data-stu-id="3b2f2-110">Why should a working app be migrated to Blazor?</span></span> <span data-ttu-id="3b2f2-111">很多时候，并无必要。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-111">Many times, there's no need.</span></span> <span data-ttu-id="3b2f2-112">即使许多年后，我们还是会支持 ASP.NET Web Forms。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-112">ASP.NET Web Forms will continue to be supported for many years.</span></span> <span data-ttu-id="3b2f2-113">但是，Blazor 提供的许多功能仅在已迁移的应用上受支持。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-113">However, many of the features that Blazor provides are only supported on a migrated app.</span></span> <span data-ttu-id="3b2f2-114">此类功能包括：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-114">Such features include:</span></span>

- <span data-ttu-id="3b2f2-115">`Span<T>` 等框架的性能改进</span><span class="sxs-lookup"><span data-stu-id="3b2f2-115">Performance improvements in the framework such as `Span<T>`</span></span>
- <span data-ttu-id="3b2f2-116">能够作为 WebAssembly 运行</span><span class="sxs-lookup"><span data-stu-id="3b2f2-116">Ability to run as WebAssembly</span></span>
- <span data-ttu-id="3b2f2-117">对 Linux 和 macOS 的跨平台支持</span><span class="sxs-lookup"><span data-stu-id="3b2f2-117">Cross-platform support for Linux and macOS</span></span>
- <span data-ttu-id="3b2f2-118">应用本地部署或共享框架部署，不影响其他应用</span><span class="sxs-lookup"><span data-stu-id="3b2f2-118">App-local deployment or shared framework deployment without impacting other apps</span></span>

<span data-ttu-id="3b2f2-119">如果这些功能或其他新功能足够吸引你，那么迁移应用就有价值。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-119">If these or other new features are compelling enough, there may be value in migrating the app.</span></span> <span data-ttu-id="3b2f2-120">迁移可以采取不同的形式；可以迁移整个应用，也可以只迁移需要更改的某些终结点。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-120">The migration can take different shapes; it can be the entire app, or only certain endpoints that require the changes.</span></span> <span data-ttu-id="3b2f2-121">迁移决策最终取决于开发人员要解决的业务问题。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-121">The decision to migrate is ultimately based on the business problems to be solved by the developer.</span></span>

## <a name="server-side-versus-client-side-hosting"></a><span data-ttu-id="3b2f2-122">服务器端与客户端托管</span><span class="sxs-lookup"><span data-stu-id="3b2f2-122">Server-side versus client-side hosting</span></span>

<span data-ttu-id="3b2f2-123">如[托管模型](hosting-models.md)一章中所述，可以通过两种不同的方式托管 Blazor 应用：服务器端和客户端。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-123">As described in the [hosting models](hosting-models.md) chapter, a Blazor app can be hosted in two different ways: server-side and client-side.</span></span> <span data-ttu-id="3b2f2-124">服务器端模型使用 ASP.NET Core SignalR 连接来管理 DOM 更新，同时在服务器上运行所有实际代码。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-124">The server-side model uses ASP.NET Core SignalR connections to manage the DOM updates while running any actual code on the server.</span></span> <span data-ttu-id="3b2f2-125">客户端模型在浏览器中作为 WebAssembly 运行，并且不需要服务器连接。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-125">The client-side model runs as WebAssembly within a browser and requires no server connections.</span></span> <span data-ttu-id="3b2f2-126">哪种模型最适合某个特定应用可能会受到很多差异的影响：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-126">There are a number of differences that may affect which is best for a specific app:</span></span>

- <span data-ttu-id="3b2f2-127">目前，作为 WebAssembly 运行尚不支持所有功能（如线程）</span><span class="sxs-lookup"><span data-stu-id="3b2f2-127">Running as WebAssembly doesn't support all features (such as threading) at the current time</span></span>
- <span data-ttu-id="3b2f2-128">客户端和服务器之间的聊天通信可能会导致服务器端模式出现延迟问题</span><span class="sxs-lookup"><span data-stu-id="3b2f2-128">Chatty communication between the client and server may cause latency issues in server-side mode</span></span>
- <span data-ttu-id="3b2f2-129">访问数据库和内部服务或受保护的服务需要带有客户端托管的单独服务</span><span class="sxs-lookup"><span data-stu-id="3b2f2-129">Access to databases and internal or protected services require a separate service with client-side hosting</span></span>

<span data-ttu-id="3b2f2-130">在撰写本文时，服务器端模型更接近于 Web Forms。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-130">At the time of writing, the server-side model more closely resembles Web Forms.</span></span> <span data-ttu-id="3b2f2-131">本章的大部分内容重点介绍服务器端托管模型，因为它已经可以投入生产。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-131">Most of this chapter focuses on the server-side hosting model, as it's production-ready.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="3b2f2-132">创建新项目</span><span class="sxs-lookup"><span data-stu-id="3b2f2-132">Create a new project</span></span>

<span data-ttu-id="3b2f2-133">最初的迁移步骤是创建一个新项目。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-133">This initial migration step is to create a new project.</span></span> <span data-ttu-id="3b2f2-134">此项目类型基于 .NET 的 SDK 样式项目，并简化了以前项目格式中使用的许多样本。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-134">This project type is based on the SDK style projects of .NET and simplifies much of the boilerplate that was used in previous project formats.</span></span> <span data-ttu-id="3b2f2-135">有关更多详细信息，请参阅[项目结构](project-structure.md)一章。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-135">For more detail, please see the chapter on [Project Structure](project-structure.md).</span></span>

<span data-ttu-id="3b2f2-136">创建项目后，安装上一个项目中使用的库。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-136">Once the project has been created, install the libraries that were used in the previous project.</span></span> <span data-ttu-id="3b2f2-137">在较早的 Web Forms 项目中，你可能已经使用 packages.config 文件列出了所需的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-137">In older Web Forms projects, you may have used the *packages.config* file to list the required NuGet packages.</span></span> <span data-ttu-id="3b2f2-138">在新的 SDK 样式项目中，项目文件中的 packages.config 已替换为 `<PackageReference>` 元素。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-138">In the new SDK-style project, *packages.config* has been replaced with `<PackageReference>` elements in the project file.</span></span> <span data-ttu-id="3b2f2-139">这种方法的好处是，所有依赖项都以可传递的方式安装。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-139">A benefit to this approach is that all dependencies are installed transitively.</span></span> <span data-ttu-id="3b2f2-140">你只需列出你关心的顶级依赖项。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-140">You only list the top-level dependencies you care about.</span></span>

<span data-ttu-id="3b2f2-141">你要使用的许多依赖项都适用于 .NET，包括实体框架 6 和 log4net。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-141">Many of the dependencies you're using are available for .NET, including Entity Framework 6 and log4net.</span></span> <span data-ttu-id="3b2f2-142">如果没有可用的 .NET 或 .NET Standard 版本，通常可以使用 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-142">If there's no .NET or .NET Standard version available, the .NET Framework version can often be used.</span></span> <span data-ttu-id="3b2f2-143">你的里程可能会有所不同。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-143">Your mileage may vary.</span></span> <span data-ttu-id="3b2f2-144">使用任何在 .NET 中不可用的 API 都会导致运行时错误。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-144">Any API used that isn't available in .NET causes a runtime error.</span></span> <span data-ttu-id="3b2f2-145">如果有这种包，Visual Studio 会通知你。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-145">Visual Studio notifies you of such packages.</span></span> <span data-ttu-id="3b2f2-146">在解决方案资源管理器中，项目的“引用”节点上会出现一个黄色图标。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-146">A yellow icon appears on the project's **References** node in **Solution Explorer**.</span></span>

<span data-ttu-id="3b2f2-147">在基于 Blazor 的 eShop 项目中，你可以看到已安装的包。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-147">In the Blazor-based eShop project, you can see the packages that are installed.</span></span> <span data-ttu-id="3b2f2-148">以前，packages.config 文件会列出项目中使用的每一个包，导致文件长达近 50 行。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-148">Previously, the *packages.config* file listed every package used in the project, resulting in a file almost 50 lines long.</span></span> <span data-ttu-id="3b2f2-149">下面是 packages.config 的一个代码片段：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-149">A snippet of *packages.config* is:</span></span>

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

<span data-ttu-id="3b2f2-150">`<packages>` 元素包含所有必要的依赖项。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-150">The `<packages>` element includes all necessary dependencies.</span></span> <span data-ttu-id="3b2f2-151">你很难确定要添加其中哪些包，因为这些包都是必需的。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-151">It's difficult to identify which of these packages are included because you require them.</span></span> <span data-ttu-id="3b2f2-152">某些 `<package>` 元素只是为了满足所需的依赖项需求而列出。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-152">Some `<package>` elements are listed simply to satisfy the needs of dependencies you require.</span></span>

<span data-ttu-id="3b2f2-153">Blazor 项目在项目文件的 `<ItemGroup>` 元素中列出了所需的依赖项：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-153">The Blazor project lists the dependencies you require within an `<ItemGroup>` element in the project file:</span></span>

```xml
<ItemGroup>
    <PackageReference Include="Autofac" Version="4.9.3" />
    <PackageReference Include="EntityFramework" Version="6.4.4" />
    <PackageReference Include="log4net" Version="2.0.12" />
    <PackageReference Include="Microsoft.Extensions.Logging.Log4Net.AspNetCore" Version="2.2.12" />
</ItemGroup>
```

<span data-ttu-id="3b2f2-154">有一个 NuGet 包简化了 Web Forms 开发人员的生活，那就是 [Windows 兼容包](../../core/porting/windows-compat-pack.md)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-154">One NuGet package that simplifies the life of Web Forms developers is the [Windows Compatibility Pack](../../core/porting/windows-compat-pack.md).</span></span> <span data-ttu-id="3b2f2-155">虽然 .NET 是跨平台的，但有些功能只能在 Windows 上使用。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-155">Although .NET is cross-platform, some features are only available on Windows.</span></span> <span data-ttu-id="3b2f2-156">通过安装兼容包，可以使用 Windows 特定功能。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-156">Windows-specific features are made available by installing the compatibility pack.</span></span> <span data-ttu-id="3b2f2-157">此类功能的示例包括注册表、WMI 和目录服务。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-157">Examples of such features include the Registry, WMI, and Directory Services.</span></span> <span data-ttu-id="3b2f2-158">该包添加了大约 20,000 个 API，并激活了许多你可能已经熟悉的服务。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-158">The package adds around 20,000 APIs and activates many services with which you may already be familiar.</span></span> <span data-ttu-id="3b2f2-159">eShop 项目不需要兼容包；但如果你的项目使用 Windows 特定功能，该包可以简化迁移工作。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-159">The eShop project doesn't require the compatibility pack; but if your projects use Windows-specific features, the package eases the migration efforts.</span></span>

## <a name="enable-startup-process"></a><span data-ttu-id="3b2f2-160">启用启动过程</span><span class="sxs-lookup"><span data-stu-id="3b2f2-160">Enable startup process</span></span>

<span data-ttu-id="3b2f2-161">从 Web Forms 开始，Blazor 的启动过程发生了变化，它遵循其他 ASP.NET Core 服务的类似设置。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-161">The startup process for Blazor has changed from Web Forms and follows a similar setup for other ASP.NET Core services.</span></span> <span data-ttu-id="3b2f2-162">当在服务器端托管时，Blazor 组件作为正常 ASP.NET Core 应用的一部分运行。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-162">When hosted server-side, Blazor components are run as part of a normal ASP.NET Core app.</span></span> <span data-ttu-id="3b2f2-163">通过 WebAssembly 在浏览器中托管时，Blazor 组件使用类似的托管模型。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-163">When hosted in the browser with WebAssembly, Blazor components use a similar hosting model.</span></span> <span data-ttu-id="3b2f2-164">不同之处在于，这些组件作为独立于任何后端进程的服务运行。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-164">The difference is the components are run as a separate service from any of the backend processes.</span></span> <span data-ttu-id="3b2f2-165">无论采用哪种方式，启动过程都差不多。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-165">Either way, the startup is similar.</span></span>

<span data-ttu-id="3b2f2-166">Global.asax.cs 文件是 Web Forms 项目的默认启动页面。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-166">The *Global.asax.cs* file is the default startup page for Web Forms projects.</span></span> <span data-ttu-id="3b2f2-167">在 eShop 项目中，此文件将配置控制反转 (IoC) 容器，并处理应用或请求的各种生命周期事件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-167">In the eShop project, this file configures the Inversion of Control (IoC) container and handles the various lifecycle events of the app or request.</span></span> <span data-ttu-id="3b2f2-168">其中一些事件通过中间件处理（例如 `Application_BeginRequest`）。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-168">Some of these events are handled with middleware (such as `Application_BeginRequest`).</span></span> <span data-ttu-id="3b2f2-169">其他事件则需要通过依赖关系注入 (DI) 来覆盖特定服务。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-169">Other events require the overriding of specific services via dependency injection (DI).</span></span>

<span data-ttu-id="3b2f2-170">举个例子，eShop 的 Global.asax.cs 文件包含以下代码：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-170">By way of example, the *Global.asax.cs* file for eShop, contains the following code:</span></span>

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

<span data-ttu-id="3b2f2-171">前面的文件变成服务器端 Blazor 中的 `Startup` 类：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-171">The preceding file becomes the `Startup` class in server-side Blazor:</span></span>

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

<span data-ttu-id="3b2f2-172">你可能会注意到，Web Forms 的一个重要变化是 DI 的重要性。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-172">One significant change you may notice from Web Forms is the prominence of DI.</span></span> <span data-ttu-id="3b2f2-173">DI 一直是 ASP.NET Core 设计的指导原则。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-173">DI has been a guiding principle in the ASP.NET Core design.</span></span> <span data-ttu-id="3b2f2-174">它支持自定义 ASP.NET Core 框架的几乎所有方面。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-174">It supports customization of almost all aspects of the ASP.NET Core framework.</span></span> <span data-ttu-id="3b2f2-175">它甚至还有一个内置的服务提供程序，可用于许多场景。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-175">There's even a built-in service provider that can be used for many scenarios.</span></span> <span data-ttu-id="3b2f2-176">如果需要更多自定义，许多社区项目可以提供支持。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-176">If more customization is required, it can be supported by many community projects.</span></span> <span data-ttu-id="3b2f2-177">例如，你可以结转第三方 DI 库投资。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-177">For example, you can carry forward your third-party DI library investment.</span></span>

<span data-ttu-id="3b2f2-178">在原始 eShop 应用中，有一些用于会话管理的配置。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-178">In the original eShop app, there's some configuration for session management.</span></span> <span data-ttu-id="3b2f2-179">由于服务器端 Blazor 使用 ASP.NET Core SignalR 进行通信，因此不支持会话状态，因为连接可能独立于 HTTP 上下文。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-179">Since server-side Blazor uses ASP.NET Core SignalR for communication, the session state isn't supported as the connections may occur independent of an HTTP context.</span></span> <span data-ttu-id="3b2f2-180">使用会话状态的应用需要重新架构，才能作为 Blazor 应用运行。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-180">An app that uses the session state requires rearchitecting before running as a Blazor app.</span></span>

<span data-ttu-id="3b2f2-181">有关应用启动的详细信息，请参阅[应用启动](app-startup.md)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-181">For more information about app startup, see [App startup](app-startup.md).</span></span>

## <a name="migrate-http-modules-and-handlers-to-middleware"></a><span data-ttu-id="3b2f2-182">将 HTTP 模块和处理程序迁移到中间件</span><span class="sxs-lookup"><span data-stu-id="3b2f2-182">Migrate HTTP modules and handlers to middleware</span></span>

<span data-ttu-id="3b2f2-183">HTTP 模块和处理程序是 Web Forms 中的常见模式，用于控制 HTTP 请求管道。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-183">HTTP modules and handlers are common patterns in Web Forms to control the HTTP request pipeline.</span></span> <span data-ttu-id="3b2f2-184">可以注册实现 `IHttpModule` 或 `IHttpHandler` 的类，由它们来处理传入请求。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-184">Classes that implement `IHttpModule` or `IHttpHandler` could be registered and process incoming requests.</span></span> <span data-ttu-id="3b2f2-185">Web Forms 在 web.config 文件中配置模块和处理程序。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-185">Web Forms configure modules and handlers in the *web.config* file.</span></span> <span data-ttu-id="3b2f2-186">而且，Web Forms 在很大程度上取决于应用生命周期事件处理。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-186">Web Forms is also heavily based on app lifecycle event handling.</span></span> <span data-ttu-id="3b2f2-187">ASP.NET Core 则使用中间件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-187">ASP.NET Core uses middleware instead.</span></span> <span data-ttu-id="3b2f2-188">中间件在 `Startup` 类的 `Configure` 方法中注册。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-188">Middleware is registered in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="3b2f2-189">中间件的执行顺序由注册顺序决定。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-189">Middleware execution order is determined by the registration order.</span></span>

<span data-ttu-id="3b2f2-190">在[启用启动过程](#enable-startup-process)部分中，Web Forms 引发了一个生命周期事件作为 `Application_BeginRequest` 方法。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-190">In the [Enable startup process](#enable-startup-process) section, a lifecycle event was raised by Web Forms as the `Application_BeginRequest` method.</span></span> <span data-ttu-id="3b2f2-191">此事件在 ASP.NET Core 中不可用。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-191">This event isn't available in ASP.NET Core.</span></span> <span data-ttu-id="3b2f2-192">实现此行为的一种方法是实现中间件，如 Startup.cs 文件示例中所示。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-192">One way to achieve this behavior is to implement middleware as seen in the *Startup.cs* file example.</span></span> <span data-ttu-id="3b2f2-193">此中间件执行相同的逻辑，然后将控制权转移到中间件管道中的下一个处理程序。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-193">This middleware does the same logic and then transfers control to the next handler in the middleware pipeline.</span></span>

<span data-ttu-id="3b2f2-194">有关迁移模块和处理程序的详细信息，请参阅[将 HTTP 处理程序和模块迁移到 ASP.NET Core 中间件](/aspnet/core/migration/http-modules)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-194">For more information on migrating modules and handlers, see [Migrate HTTP handlers and modules to ASP.NET Core middleware](/aspnet/core/migration/http-modules).</span></span>

## <a name="migrate-static-files"></a><span data-ttu-id="3b2f2-195">迁移静态文件</span><span class="sxs-lookup"><span data-stu-id="3b2f2-195">Migrate static files</span></span>

<span data-ttu-id="3b2f2-196">若要提供静态文件（例如 HTML、CSS、图像和 JavaScript），必须由中间件公开这些文件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-196">To serve static files (for example, HTML, CSS, images, and JavaScript), the files must be exposed by middleware.</span></span> <span data-ttu-id="3b2f2-197">通过调用 `UseStaticFiles` 方法，可以从 Web 根路径中提供静态文件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-197">Calling the `UseStaticFiles` method enables the serving of static files from the web root path.</span></span> <span data-ttu-id="3b2f2-198">默认的 Web 根目录是 wwwroot，但可以对其进行自定义。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-198">The default web root directory is *wwwroot*, but it can be customized.</span></span> <span data-ttu-id="3b2f2-199">如 eShop 的 `Startup` 类的 `Configure` 方法中包含的 Web 根目录：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-199">As included in the `Configure` method of eShop's `Startup` class:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="3b2f2-200">eShop 项目可以实现基本的静态文件访问。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-200">The eShop project enables basic static file access.</span></span> <span data-ttu-id="3b2f2-201">有许多自定义项可用于静态文件访问。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-201">There are many customizations available for static file access.</span></span> <span data-ttu-id="3b2f2-202">有关启用默认文件或文件浏览器的信息，请参阅 [ASP.NET Core 中的静态文件](/aspnet/core/fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-202">For information on enabling default files or a file browser, see [Static files in ASP.NET Core](/aspnet/core/fundamentals/static-files).</span></span>

## <a name="migrate-runtime-bundling-and-minification-setup"></a><span data-ttu-id="3b2f2-203">迁移运行时捆绑和缩小设置</span><span class="sxs-lookup"><span data-stu-id="3b2f2-203">Migrate runtime bundling and minification setup</span></span>

<span data-ttu-id="3b2f2-204">捆绑和缩小是两种性能优化技术，用于减少服务器检索某些文件类型的请求的数量和大小。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-204">Bundling and minification are performance optimization techniques for reducing the number and size of server requests to retrieve certain file types.</span></span> <span data-ttu-id="3b2f2-205">JavaScript 和 CSS 在被发送到客户端之前通常会经过某种形式的捆绑或缩小。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-205">JavaScript and CSS often undergo some form of bundling or minification before being sent to the client.</span></span> <span data-ttu-id="3b2f2-206">在 ASP.NET Web Forms 中，这些优化在运行时进行处理。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-206">In ASP.NET Web Forms, these optimizations are handled at runtime.</span></span> <span data-ttu-id="3b2f2-207">优化约定定义为 App_Start/BundleConfig.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-207">The optimization conventions are defined an *App_Start/BundleConfig.cs* file.</span></span> <span data-ttu-id="3b2f2-208">ASP.NET Core 采用更具声明性的方法。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-208">In ASP.NET Core, a more declarative approach is adopted.</span></span> <span data-ttu-id="3b2f2-209">一个文件将列出要缩小的文件以及特定的缩小设置。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-209">A file lists the files to be minified, along with specific minification settings.</span></span>

<span data-ttu-id="3b2f2-210">有关捆绑和缩小的详细信息，请参阅[在 ASP.NET Core 中捆绑和缩小静态资产](/aspnet/core/client-side/bundling-and-minification)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-210">For more information on bundling and minification, see [Bundle and minify static assets in ASP.NET Core](/aspnet/core/client-side/bundling-and-minification).</span></span>

## <a name="migrate-aspx-pages"></a><span data-ttu-id="3b2f2-211">迁移 ASPX 页面</span><span class="sxs-lookup"><span data-stu-id="3b2f2-211">Migrate ASPX pages</span></span>

<span data-ttu-id="3b2f2-212">Web Forms 应用中的页面是扩展名为 .aspx 的文件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-212">A page in a Web Forms app is a file with the *.aspx* extension.</span></span> <span data-ttu-id="3b2f2-213">Web Forms 页面通常可以映射到 Blazor 中的组件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-213">A Web Forms page can often be mapped to a component in Blazor.</span></span> <span data-ttu-id="3b2f2-214">Blazor 组件是在扩展名为 .razor 的文件中创作的。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-214">A Blazor component is authored in a file with the *.razor* extension.</span></span> <span data-ttu-id="3b2f2-215">在 eShop 项目中，需要将五个页面转换为 Razor 页面。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-215">For the eShop project, five pages are converted to a Razor page.</span></span>

<span data-ttu-id="3b2f2-216">例如，详细信息视图包括 Web Forms 项目中的三个文件：Details.aspx、Details.aspx.cs 和 Details.aspx.designer.cs。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-216">For example, the details view comprises three files in the Web Forms project: *Details.aspx*, *Details.aspx.cs*, and *Details.aspx.designer.cs*.</span></span> <span data-ttu-id="3b2f2-217">转换为 Blazor 时，代码隐藏和标记会合并到 Details.razor 中。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-217">When converting to Blazor, the code-behind and markup are combined into *Details.razor*.</span></span> <span data-ttu-id="3b2f2-218">Razor 编译（等效于 .designer.cs 文件中的内容）存储在 obj 目录中，默认情况下在解决方案资源管理器中不可见。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-218">Razor compilation (equivalent to what's in *.designer.cs* files) is stored in the *obj* directory and isn't, by default, viewable in **Solution Explorer**.</span></span> <span data-ttu-id="3b2f2-219">Web Forms 页面包含以下标记：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-219">The Web Forms page consists of the following markup:</span></span>

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

<span data-ttu-id="3b2f2-220">以上标记的代码隐藏包括以下代码：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-220">The preceding markup's code-behind includes the following code:</span></span>

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

<span data-ttu-id="3b2f2-221">转换为 Blazor 时，Web Forms 页面会转换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-221">When converted to Blazor, the Web Forms page translates to the following code:</span></span>

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

<span data-ttu-id="3b2f2-222">请注意，代码和标记在同一文件中。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-222">Notice that the code and markup are in the same file.</span></span> <span data-ttu-id="3b2f2-223">使用 `@inject` 属性可以访问任何必需的服务。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-223">Any required services are made accessible with the `@inject` attribute.</span></span> <span data-ttu-id="3b2f2-224">根据 `@page` 指令，可以在 `Catalog/Details/{id}` 路由中访问此页面。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-224">Per the `@page` directive, this page can be accessed at the `Catalog/Details/{id}` route.</span></span> <span data-ttu-id="3b2f2-225">该路由的 `{id}` 占位符的值已限制为整数。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-225">The value of the route's `{id}` placeholder has been constrained to an integer.</span></span> <span data-ttu-id="3b2f2-226">如[路由](pages-routing-layouts.md)部分中所述，Razor 组件与 Web Forms 不同，它会显式声明其路由以及包含的所有参数。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-226">As described in the [routing](pages-routing-layouts.md) section, unlike Web Forms, a Razor component explicitly states its route and any parameters that are included.</span></span> <span data-ttu-id="3b2f2-227">许多 Web Forms 控件在 Blazor 中可能没有完全对应的控件。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-227">Many Web Forms controls may not have exact counterparts in Blazor.</span></span> <span data-ttu-id="3b2f2-228">但通常会有一个等效的 HTML 代码片段可以达到相同目的。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-228">There's often an equivalent HTML snippet that will serve the same purpose.</span></span> <span data-ttu-id="3b2f2-229">例如，可以将 `<asp:Label />` 控件替换为 HTML `<label>` 元素。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-229">For example, the `<asp:Label />` control can be replaced with an HTML `<label>` element.</span></span>

### <a name="model-validation-in-blazor"></a><span data-ttu-id="3b2f2-230">Blazor 中的模型验证</span><span class="sxs-lookup"><span data-stu-id="3b2f2-230">Model validation in Blazor</span></span>

<span data-ttu-id="3b2f2-231">如果你的 Web Forms 代码包含验证，那么几乎不需要更改就可以传输大部分内容。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-231">If your Web Forms code includes validation, you can transfer much of what you have with little-to-no changes.</span></span> <span data-ttu-id="3b2f2-232">在 Blazor 中运行的好处是可以运行相同的验证逻辑，而无需自定义 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-232">A benefit to running in Blazor is that the same validation logic can be run without needing custom JavaScript.</span></span> <span data-ttu-id="3b2f2-233">数据注释使模型验证变得简单易行。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-233">Data annotations enable easy model validation.</span></span>

<span data-ttu-id="3b2f2-234">例如，Create.aspx 页面包含一个带验证的数据输入窗体。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-234">For example, the *Create.aspx* page has a data entry form with validation.</span></span> <span data-ttu-id="3b2f2-235">代码片段示例如下所示：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-235">An example snippet would look like this:</span></span>

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

<span data-ttu-id="3b2f2-236">在 Blazor 中，Create.razor 文件中提供了等效标记：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-236">In Blazor, the equivalent markup is provided in a *Create.razor* file:</span></span>

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

<span data-ttu-id="3b2f2-237">`EditForm` 上下文包括验证支持，可以包装在输入周围。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-237">The `EditForm` context includes validation support and can be wrapped around an input.</span></span> <span data-ttu-id="3b2f2-238">数据注释是添加验证的常用方法。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-238">Data annotations are a common way to add validation.</span></span> <span data-ttu-id="3b2f2-239">可以通过 `DataAnnotationsValidator` 组件添加此类验证支持。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-239">Such validation support can be added via the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="3b2f2-240">有关此机制的详细信息，请参阅 [ASP.NET Core Blazor 窗体和验证](/aspnet/core/blazor/forms-validation)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-240">For more information on this mechanism, see [ASP.NET Core Blazor forms and validation](/aspnet/core/blazor/forms-validation).</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="3b2f2-241">迁移配置</span><span class="sxs-lookup"><span data-stu-id="3b2f2-241">Migrate configuration</span></span>

<span data-ttu-id="3b2f2-242">在 Web Forms 项目中，配置数据通常存储在 web.config 文件中。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-242">In a Web Forms project, configuration data is most commonly stored in the *web.config* file.</span></span> <span data-ttu-id="3b2f2-243">可使用 `ConfigurationManager` 访问配置数据。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-243">The configuration data is accessed with `ConfigurationManager`.</span></span> <span data-ttu-id="3b2f2-244">通常需要使用服务来分析对象。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-244">Services were often required to parse objects.</span></span> <span data-ttu-id="3b2f2-245">在 .NET Framework 4.7.2 中，已通过 `ConfigurationBuilders` 向配置添加可组合性。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-245">With .NET Framework 4.7.2, composability was added to the configuration via `ConfigurationBuilders`.</span></span> <span data-ttu-id="3b2f2-246">这些生成器允许开发人员为配置添加各种源，然后在运行时组合这些源，以检索必要的值。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-246">These builders allowed developers to add various sources for the configuration that was then composed at runtime to retrieve the necessary values.</span></span>

<span data-ttu-id="3b2f2-247">ASP.NET Core 引入了一种灵活的配置系统，该系统允许你定义应用和部署所使用的配置源。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-247">ASP.NET Core introduced a flexible configuration system that allows you to define the configuration source or sources used by your app and deployment.</span></span> <span data-ttu-id="3b2f2-248">你在 Web Forms 应用中可能使用的 `ConfigurationBuilder` 基础结构是按照 ASP.NET Core 配置系统中使用的概念建模的。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-248">The `ConfigurationBuilder` infrastructure that you may be using in your Web Forms app was modeled after the concepts used in the ASP.NET Core configuration system.</span></span>

<span data-ttu-id="3b2f2-249">以下代码片段演示了 Web Forms eShop 项目如何使用 web.config 存储配置值：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-249">The following snippet demonstrates how the Web Forms eShop project uses *web.config* to store configuration values:</span></span>

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

<span data-ttu-id="3b2f2-250">诸如数据库连接字符串之类的机密通常存储在 web.config 中。这些机密不可避免地保存在不安全的位置，例如源代码管理。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-250">It's common for secrets, such as database connection strings, to be stored within the *web.config*. The secrets are inevitably persisted in unsecure locations, such as source control.</span></span> <span data-ttu-id="3b2f2-251">使用 ASP.NET Core 上的 Blazor 时，上面基于 XML 的配置将替换为以下 JSON：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-251">With Blazor on ASP.NET Core, the preceding XML-based configuration is replaced with the following JSON:</span></span>

```json
{
  "ConnectionStrings": {
    "CatalogDBContext": "Data Source=(localdb)\\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;"
  },
  "UseMockData": true,
  "UseCustomizationData": false
}
```

<span data-ttu-id="3b2f2-252">JSON 是默认配置格式；但是，ASP.NET Core 也支持许多其他格式，包括 XML。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-252">JSON is the default configuration format; however, ASP.NET Core supports many other formats, including XML.</span></span> <span data-ttu-id="3b2f2-253">另外还有几种社​​区支持的格式。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-253">There are also several community-supported formats.</span></span>

<span data-ttu-id="3b2f2-254">Blazor 项目的 `Startup` 类中的构造函数通过一种称为构造函数注入的 DI 技术接受 `IConfiguration` 实例：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-254">The constructor in the Blazor project's `Startup` class accepts an `IConfiguration` instance through a DI technique known as constructor injection:</span></span>

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

<span data-ttu-id="3b2f2-255">默认情况下，环境变量、JSON 文件（appsettings.json 和 appsettings.{Environment}.json）和命令行选项在配置对象中注册为有效的配置源。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-255">By default, environment variables, JSON files (*appsettings.json* and *appsettings.{Environment}.json*), and command-line options are registered as valid configuration sources in the configuration object.</span></span> <span data-ttu-id="3b2f2-256">可以通过 `Configuration[key]` 访问配置源。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-256">The configuration sources can be accessed via `Configuration[key]`.</span></span> <span data-ttu-id="3b2f2-257">一种更高级的技术是使用选项模式将配置数据绑定到对象。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-257">A more advanced technique is to bind the configuration data to objects using the options pattern.</span></span> <span data-ttu-id="3b2f2-258">有关配置和选项模式的详细信息，请分别参阅 [ASP.NET Core 中的配置](/aspnet/core/fundamentals/configuration/)和 [ASP.NET Core 中的选项模式](/aspnet/core/fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-258">For more information on configuration and the options pattern, see [Configuration in ASP.NET Core](/aspnet/core/fundamentals/configuration/) and [Options pattern in ASP.NET Core](/aspnet/core/fundamentals/configuration/options), respectively.</span></span>

## <a name="migrate-data-access"></a><span data-ttu-id="3b2f2-259">迁移数据访问</span><span class="sxs-lookup"><span data-stu-id="3b2f2-259">Migrate data access</span></span>

<span data-ttu-id="3b2f2-260">数据访问是所有应用很重要的一个方面。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-260">Data access is an important aspect of any app.</span></span> <span data-ttu-id="3b2f2-261">eShop 项目将目录信息存储在数据库中，并使用实体框架 (EF) 6 检索数据。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-261">The eShop project stores catalog information in a database and retrieves the data with Entity Framework (EF) 6.</span></span> <span data-ttu-id="3b2f2-262">由于 .NET 5.0 支持 EF 6，因此该项目可以继续使用它。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-262">Since EF 6 is supported in .NET 5.0, the project can continue to use it.</span></span>

<span data-ttu-id="3b2f2-263">以下与 EF 相关的更改对于 eShop 是必需的：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-263">The following EF-related changes were necessary to eShop:</span></span>

- <span data-ttu-id="3b2f2-264">在 .NET Framework 中，`DbContext` 对象接受格式为 name=ConnectionString 的字符串，并使用 `ConfigurationManager.AppSettings[ConnectionString]` 中的连接字符串进行连接。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-264">In .NET Framework, the `DbContext` object accepts a string of the form *name=ConnectionString* and uses the connection string from `ConfigurationManager.AppSettings[ConnectionString]` to connect.</span></span> <span data-ttu-id="3b2f2-265">.NET Core 不支持这样做，</span><span class="sxs-lookup"><span data-stu-id="3b2f2-265">In .NET Core, this isn't supported.</span></span> <span data-ttu-id="3b2f2-266">你必须提供连接字符串。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-266">The connection string must be supplied.</span></span>
- <span data-ttu-id="3b2f2-267">以同步方式访问数据库。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-267">The database was accessed in a synchronous way.</span></span> <span data-ttu-id="3b2f2-268">虽然这种方式可行，但可伸缩性可能会受到影响。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-268">Though this works, scalability may suffer.</span></span> <span data-ttu-id="3b2f2-269">此逻辑应迁移到异步模式。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-269">This logic should be moved to an asynchronous pattern.</span></span>

<span data-ttu-id="3b2f2-270">虽然无法对数据集绑定提供本机支持，但是，Blazor 在 Razor 页面中提供 C# 支持，从而提供出色的灵活性和强大的功能。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-270">Although there isn't the same native support for dataset binding, Blazor provides flexibility and power with its C# support in a Razor page.</span></span> <span data-ttu-id="3b2f2-271">例如，你可以执行计算并显示结果。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-271">For example, you can perform calculations and display the result.</span></span> <span data-ttu-id="3b2f2-272">有关 Blazor 中数据模式的详细信息，请参阅[数据访问](data.md)一章。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-272">For more information on data patterns in Blazor, see the [Data access](data.md) chapter.</span></span>

## <a name="architectural-changes"></a><span data-ttu-id="3b2f2-273">体系结构更改</span><span class="sxs-lookup"><span data-stu-id="3b2f2-273">Architectural changes</span></span>

<span data-ttu-id="3b2f2-274">最后，在迁移到 Blazor 时，需要考虑一些重要的体系结构差异。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-274">Finally, there are some important architectural differences to consider when migrating to Blazor.</span></span> <span data-ttu-id="3b2f2-275">其中许多更改适用于基于 .NET Core 或 ASP.NET Core 的任何产品。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-275">Many of these changes are applicable to anything based on .NET Core or ASP.NET Core.</span></span>

<span data-ttu-id="3b2f2-276">由于 Blazor 是基于 .NET Core 构建的，因此请注意确保支持在 .NET Core 上运行。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-276">Because Blazor is built on .NET Core, there are considerations in ensuring support on .NET Core.</span></span> <span data-ttu-id="3b2f2-277">一些重大更改包括删除以下功能：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-277">Some of the major changes include the removal of the following features:</span></span>

- <span data-ttu-id="3b2f2-278">多个 AppDomain</span><span class="sxs-lookup"><span data-stu-id="3b2f2-278">Multiple AppDomains</span></span>
- <span data-ttu-id="3b2f2-279">远程处理</span><span class="sxs-lookup"><span data-stu-id="3b2f2-279">Remoting</span></span>
- <span data-ttu-id="3b2f2-280">代码访问安全性 (CAS)</span><span class="sxs-lookup"><span data-stu-id="3b2f2-280">Code Access Security (CAS)</span></span>
- <span data-ttu-id="3b2f2-281">安全透明度</span><span class="sxs-lookup"><span data-stu-id="3b2f2-281">Security Transparency</span></span>

<span data-ttu-id="3b2f2-282">可通过多种技术来确定支持在 .NET Core 上运行所需进行的更改；有关这些技术的详细信息，请参阅[将代码从 .NET Framework 移植到 .NET Core](../../core/porting/index.md)。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-282">For more information on techniques to identify necessary changes to support running on .NET Core, see [Port your code from .NET Framework to .NET Core](../../core/porting/index.md).</span></span>

<span data-ttu-id="3b2f2-283">ASP.NET Core 是 ASP.NET 的重构版本，它有一些最初看起来并不明显的更改。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-283">ASP.NET Core is a reimagined version of ASP.NET and has some changes that may not initially seem obvious.</span></span> <span data-ttu-id="3b2f2-284">其主要更改包括：</span><span class="sxs-lookup"><span data-stu-id="3b2f2-284">The main changes are:</span></span>

- <span data-ttu-id="3b2f2-285">没有同步上下文，这意味着没有 `HttpContext.Current`、`Thread.CurrentPrincipal` 或其他静态访问器</span><span class="sxs-lookup"><span data-stu-id="3b2f2-285">No synchronization context, which means there's no `HttpContext.Current`, `Thread.CurrentPrincipal`, or other static accessors</span></span>
- <span data-ttu-id="3b2f2-286">无卷影复制</span><span class="sxs-lookup"><span data-stu-id="3b2f2-286">No shadow copying</span></span>
- <span data-ttu-id="3b2f2-287">无请求队列</span><span class="sxs-lookup"><span data-stu-id="3b2f2-287">No request queue</span></span>

<span data-ttu-id="3b2f2-288">ASP.NET Core 中的许多操作都是异步的，这使得 I/O 绑定的任务更容易卸载。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-288">Many operations in ASP.NET Core are asynchronous, which allows easier off-loading of I/O-bound tasks.</span></span> <span data-ttu-id="3b2f2-289">切勿使用 `Task.Wait()` 或 `Task.GetResult()` 进行阻塞，这样会迅速耗尽线程池资源。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-289">It's important to never block by using `Task.Wait()` or `Task.GetResult()`, which can quickly exhaust thread pool resources.</span></span>

## <a name="migration-conclusion"></a><span data-ttu-id="3b2f2-290">迁移结束语</span><span class="sxs-lookup"><span data-stu-id="3b2f2-290">Migration conclusion</span></span>

<span data-ttu-id="3b2f2-291">至此，你已经看到了许多如何将 Web Forms 项目迁移到 Blazor 的示例。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-291">At this point, you've seen many examples of what it takes to move a Web Forms project to Blazor.</span></span> <span data-ttu-id="3b2f2-292">有关完整示例，请参阅 [eShopOnBlazor](https://github.com/dotnet-architecture/eShopOnBlazor) 项目。</span><span class="sxs-lookup"><span data-stu-id="3b2f2-292">For a full example, see the [eShopOnBlazor](https://github.com/dotnet-architecture/eShopOnBlazor) project.</span></span>

>[!div class="step-by-step"]
>[<span data-ttu-id="3b2f2-293">上一页</span><span class="sxs-lookup"><span data-stu-id="3b2f2-293">Previous</span></span>](security-authentication-authorization.md)
