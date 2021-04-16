---
title: Blazor 应用的项目结构
description: 了解 ASP.NET Web Forms 与 Blazor 的项目结构比较。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: ba7113c88db728f30812821deaf7c06a80663d1f
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98189084"
---
# <a name="project-structure-for-blazor-apps"></a><span data-ttu-id="d6425-103">Blazor 应用的项目结构</span><span class="sxs-lookup"><span data-stu-id="d6425-103">Project structure for Blazor apps</span></span>

<span data-ttu-id="d6425-104">尽管 ASP.NET Web Forms 和 Blazor 的项目结构存在重大差异，但它们具有许多类似的概念。</span><span class="sxs-lookup"><span data-stu-id="d6425-104">Despite their significant project structure differences, ASP.NET Web Forms and Blazor share many similar concepts.</span></span> <span data-ttu-id="d6425-105">在这里，我们将了解 Blazor 项目的结构，并将它与 ASP.NET Web Forms 项目进行比较。</span><span class="sxs-lookup"><span data-stu-id="d6425-105">Here, we'll look at the structure of a Blazor project and compare it to an ASP.NET Web Forms project.</span></span>

<span data-ttu-id="d6425-106">若要创建第一个 Blazor 应用，请按照 [Blazor 入门步骤](/aspnet/core/blazor/get-started)中的说明进行操作。</span><span class="sxs-lookup"><span data-stu-id="d6425-106">To create your first Blazor app, follow the instructions in the [Blazor getting started steps](/aspnet/core/blazor/get-started).</span></span> <span data-ttu-id="d6425-107">可以按照说明创建 Blazor 服务器应用或是在 ASP.NET Core 中托管的 Blazor WebAssembly 应用。</span><span class="sxs-lookup"><span data-stu-id="d6425-107">You can follow the instructions to create either a Blazor Server app or a Blazor WebAssembly app hosted in ASP.NET Core.</span></span> <span data-ttu-id="d6425-108">除了特定于托管模型的逻辑外，这两个项目中的大多数代码是相同的。</span><span class="sxs-lookup"><span data-stu-id="d6425-108">Except for the hosting model-specific logic, most of the code in both projects is the same.</span></span>

## <a name="project-file"></a><span data-ttu-id="d6425-109">项目文件</span><span class="sxs-lookup"><span data-stu-id="d6425-109">Project file</span></span>

<span data-ttu-id="d6425-110">Blazor 服务器应用是 .NET 项目。</span><span class="sxs-lookup"><span data-stu-id="d6425-110">Blazor Server apps are .NET projects.</span></span> <span data-ttu-id="d6425-111">Blazor 服务器应用的项目文件可如同下面一样简单：</span><span class="sxs-lookup"><span data-stu-id="d6425-111">The project file for the Blazor Server app is about as simple as it can get:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```

<span data-ttu-id="d6425-112">Blazor WebAssembly 应用的项目文件看起来稍微复杂一些（确切的版本号可能有所不同）：</span><span class="sxs-lookup"><span data-stu-id="d6425-112">The project file for a Blazor WebAssembly app looks slightly more involved (exact version numbers may vary):</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.BlazorWebAssembly">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Components.WebAssembly" Version="5.0.0" />
    <PackageReference Include="Microsoft.AspNetCore.Components.WebAssembly.DevServer" Version="5.0.0" PrivateAssets="all" />
    <PackageReference Include="System.Net.Http.Json" Version="5.0.0" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="d6425-113">Blazor WebAssembly 项目面向 `Microsoft.NET.Sdk.BlazorWebAssembly` 而不是 `Microsoft.NET.Sdk.Web` sdk，因为它们在基于 WebAssembly 的 .NET 运行时上的浏览器中运行。</span><span class="sxs-lookup"><span data-stu-id="d6425-113">Blazor WebAssembly project targets `Microsoft.NET.Sdk.BlazorWebAssembly` instead of `Microsoft.NET.Sdk.Web` sdk because they run in the browser on a WebAssembly-based .NET runtime.</span></span> <span data-ttu-id="d6425-114">不能如同在服务器或开发人员计算机上那样将 .NET 安装到 Web 浏览器中。</span><span class="sxs-lookup"><span data-stu-id="d6425-114">You can't install .NET into a web browser like you can on a server or developer machine.</span></span> <span data-ttu-id="d6425-115">因此，项目会使用单个包引用来引用 Blazor 框架。</span><span class="sxs-lookup"><span data-stu-id="d6425-115">Consequently, the project references the Blazor framework using individual package references.</span></span>

<span data-ttu-id="d6425-116">相比之下，默认 ASP.NET Web Forms 项目在其 .csproj 文件中包含将近 300 行 XML，大多数这种文件会显式列出项目中的各种代码和内容文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-116">By comparison, a default ASP.NET Web Forms project includes almost 300 lines of XML in its *.csproj* file, most of which is explicitly listing the various code and content files in the project.</span></span> <span data-ttu-id="d6425-117">随着 `.NET 5` 的发布，`Blazor Server` 和 `Blazor WebAssembly` 应用可以轻松共享一个统一的运行时。</span><span class="sxs-lookup"><span data-stu-id="d6425-117">With the release of `.NET 5` both `Blazor Server` and `Blazor WebAssembly` app can easily share one unified runtime.</span></span>

<span data-ttu-id="d6425-118">虽然单个程序集引用受支持，但是它们在 .NET 项目中不太常见。</span><span class="sxs-lookup"><span data-stu-id="d6425-118">Although they're supported, individual assembly references are less common in .NET projects.</span></span> <span data-ttu-id="d6425-119">大多数项目依赖项都作为 NuGet 包引用进行处理。</span><span class="sxs-lookup"><span data-stu-id="d6425-119">Most project dependencies are handled as NuGet package references.</span></span> <span data-ttu-id="d6425-120">只需在 .NET 项目中引用顶级包依赖项。</span><span class="sxs-lookup"><span data-stu-id="d6425-120">You only need to reference top-level package dependencies in .NET projects.</span></span> <span data-ttu-id="d6425-121">可传递的依赖项会自动包含在内。</span><span class="sxs-lookup"><span data-stu-id="d6425-121">Transitive dependencies are included automatically.</span></span> <span data-ttu-id="d6425-122">会使用 `<PackageReference>` 元素将包引用添加到项目文件，而不是使用 ASP.NET Web Forms 项目中常见的 packages.config 文件来引用包。</span><span class="sxs-lookup"><span data-stu-id="d6425-122">Instead of using the *packages.config* file commonly found in ASP.NET Web Forms projects to reference packages, package references are added to the project file using the `<PackageReference>` element.</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
</ItemGroup>
```

## <a name="entry-point"></a><span data-ttu-id="d6425-123">入口点</span><span class="sxs-lookup"><span data-stu-id="d6425-123">Entry point</span></span>

<span data-ttu-id="d6425-124">Blazor 服务器应用的入口点在 Program.cs 文件中进行定义，与控制台应用一样。</span><span class="sxs-lookup"><span data-stu-id="d6425-124">The Blazor Server app's entry point is defined in the *Program.cs* file, as you would see in a Console app.</span></span> <span data-ttu-id="d6425-125">当应用执行时，它会使用特定于 Web 应用的默认值创建并运行 Web 主机实例。</span><span class="sxs-lookup"><span data-stu-id="d6425-125">When the app executes, it creates and runs a web host instance using defaults specific to web apps.</span></span> <span data-ttu-id="d6425-126">Web 主机会管理 Blazor 服务器应用的生命周期，并设置主机级别服务。</span><span class="sxs-lookup"><span data-stu-id="d6425-126">The web host manages the Blazor Server app's lifecycle and sets up host-level services.</span></span> <span data-ttu-id="d6425-127">此类服务的示例包括配置、日志记录、依赖项注入和 HTTP 服务器。</span><span class="sxs-lookup"><span data-stu-id="d6425-127">Examples of such services are configuration, logging, dependency injection, and the HTTP server.</span></span> <span data-ttu-id="d6425-128">此代码主要作为样板，通常保持不变。</span><span class="sxs-lookup"><span data-stu-id="d6425-128">This code is mostly boilerplate and is often left unchanged.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="d6425-129">Blazor WebAssembly 应用也在 Program.cs 中定义入口点。</span><span class="sxs-lookup"><span data-stu-id="d6425-129">Blazor WebAssembly apps also define an entry point in *Program.cs*.</span></span> <span data-ttu-id="d6425-130">代码看起来略有不同。</span><span class="sxs-lookup"><span data-stu-id="d6425-130">The code looks slightly different.</span></span> <span data-ttu-id="d6425-131">该代码的相似之处在于，它会设置应用主机以便向应用提供相同的主机级别服务。</span><span class="sxs-lookup"><span data-stu-id="d6425-131">The code is similar in that it's setting up the app host to provide the same host-level services to the app.</span></span> <span data-ttu-id="d6425-132">不过，WebAssembly 应用主机不会设置 HTTP 服务器，因为它直接在浏览器中执行。</span><span class="sxs-lookup"><span data-stu-id="d6425-132">The WebAssembly app host doesn't, however, set up an HTTP server because it executes directly in the browser.</span></span>

<span data-ttu-id="d6425-133">Blazor 应用具有 `Startup` 类（而不是 Global.asax 文件），用于定义应用的启动逻辑。</span><span class="sxs-lookup"><span data-stu-id="d6425-133">Blazor apps have a `Startup` class instead of a *Global.asax* file to define the startup logic for the app.</span></span> <span data-ttu-id="d6425-134">`Startup` 类用于配置应用和任何特定于应用的服务。</span><span class="sxs-lookup"><span data-stu-id="d6425-134">The `Startup` class is used to configure the app and any app-specific services.</span></span> <span data-ttu-id="d6425-135">在 Blazor 服务器应用中，`Startup` 类用于为 Blazor 在客户端浏览器与服务器之间使用的实时连接设置终结点。</span><span class="sxs-lookup"><span data-stu-id="d6425-135">In the Blazor Server app, the `Startup` class is used to set up the endpoint for the real-time connection used by Blazor between the client browsers and the server.</span></span> <span data-ttu-id="d6425-136">在 Blazor WebAssembly 应用中，`Startup` 类定义应用的根组件及其呈现位置。</span><span class="sxs-lookup"><span data-stu-id="d6425-136">In the Blazor WebAssembly app, the `Startup` class defines the root components for the app and where they should be rendered.</span></span> <span data-ttu-id="d6425-137">我们将在[应用启动](./app-startup.md)部分中更深入地了解 `Startup` 类。</span><span class="sxs-lookup"><span data-stu-id="d6425-137">We'll take a deeper look at the `Startup` class in the [App startup](./app-startup.md) section.</span></span>

## <a name="static-files"></a><span data-ttu-id="d6425-138">静态文件</span><span class="sxs-lookup"><span data-stu-id="d6425-138">Static files</span></span>

<span data-ttu-id="d6425-139">与 ASP.NET Web Forms 项目不同，并非 Blazor 项目中的所有文件都可以作为静态文件进行请求。</span><span class="sxs-lookup"><span data-stu-id="d6425-139">Unlike ASP.NET Web Forms projects, not all files in a Blazor project can be requested as static files.</span></span> <span data-ttu-id="d6425-140">只有 wwwroot 文件夹中的文件可进行 Web 寻址。</span><span class="sxs-lookup"><span data-stu-id="d6425-140">Only the files in the *wwwroot* folder are web-addressable.</span></span> <span data-ttu-id="d6425-141">此文件夹被称为应用的“Web 根目录”。</span><span class="sxs-lookup"><span data-stu-id="d6425-141">This folder is referred to the app's "web root".</span></span> <span data-ttu-id="d6425-142">应用的 Web 根目录之外的任何内容都不可进行 Web 寻址。</span><span class="sxs-lookup"><span data-stu-id="d6425-142">Anything outside of the app's web root *isn't* web-addressable.</span></span> <span data-ttu-id="d6425-143">此设置可提供额外的安全级别，防止在 Web 上意外公开项目文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-143">This setup provides an additional level of security that prevents accidental exposing of project files over the web.</span></span>

## <a name="configuration"></a><span data-ttu-id="d6425-144">配置</span><span class="sxs-lookup"><span data-stu-id="d6425-144">Configuration</span></span>

<span data-ttu-id="d6425-145">ASP.NET Web Forms 应用中的配置通常使用一个或多个 web.config 文件进行处理。</span><span class="sxs-lookup"><span data-stu-id="d6425-145">Configuration in ASP.NET Web Forms apps is typically handled using one or more *web.config* files.</span></span> <span data-ttu-id="d6425-146">Blazor 应用通常没有 web.config 文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-146">Blazor apps don't typically have *web.config* files.</span></span> <span data-ttu-id="d6425-147">如果它们有这类文件，则该文件仅用于在 IIS 上进行托管时配置特定于 IIS 的设置。</span><span class="sxs-lookup"><span data-stu-id="d6425-147">If they do, the file is only used to configure IIS-specific settings when hosted on IIS.</span></span> <span data-ttu-id="d6425-148">相反，Blazor 服务器应用使用 ASP.NET Core 配置抽象（Blazor WebAssembly 应用当前不支持相同的配置抽象，但这可能是会在将来添加的一种功能）。</span><span class="sxs-lookup"><span data-stu-id="d6425-148">Instead, Blazor Server apps use the ASP.NET Core configuration abstractions (Blazor WebAssembly apps don't currently support the same configuration abstractions, but that may be a feature added in the future).</span></span> <span data-ttu-id="d6425-149">例如，默认 Blazor 服务器应用会将某些设置存储在 appsettings.json 中。</span><span class="sxs-lookup"><span data-stu-id="d6425-149">For example, the default Blazor Server app stores some settings in *appsettings.json*.</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

<span data-ttu-id="d6425-150">我们将在[配置](./config.md)部分中详细了解 ASP.NET Core 项目中的配置。</span><span class="sxs-lookup"><span data-stu-id="d6425-150">We'll learn more about configuration in ASP.NET Core projects in the [Configuration](./config.md) section.</span></span>

## <a name="razor-components"></a><span data-ttu-id="d6425-151">Razor 组件</span><span class="sxs-lookup"><span data-stu-id="d6425-151">Razor components</span></span>

<span data-ttu-id="d6425-152">Blazor 项目中的大多数文件是 .razor 文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-152">Most files in Blazor projects are *.razor* files.</span></span> <span data-ttu-id="d6425-153">Razor 是一种基于 HTML 和 C# 的模板化语言，用于动态生成 Web UI。</span><span class="sxs-lookup"><span data-stu-id="d6425-153">Razor is a templating language based on HTML and C# that is used to dynamically generate web UI.</span></span> <span data-ttu-id="d6425-154">.razor 文件定义组成应用 UI 的组件。</span><span class="sxs-lookup"><span data-stu-id="d6425-154">The *.razor* files define components that make up the UI of the app.</span></span> <span data-ttu-id="d6425-155">大多数情况下，组件对于 Blazor 服务器和 Blazor WebAssembly 应用是相同的。</span><span class="sxs-lookup"><span data-stu-id="d6425-155">For the most part, the components are identical for both the Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="d6425-156">Blazor 中的组件类似于 ASP.NET Web Forms 中的用户控件。</span><span class="sxs-lookup"><span data-stu-id="d6425-156">Components in Blazor are analogous to user controls in ASP.NET Web Forms.</span></span>

<span data-ttu-id="d6425-157">生成项目时，每个 Razor 组件文件都会编译为 .NET 类。</span><span class="sxs-lookup"><span data-stu-id="d6425-157">Each Razor component file is compiled into a .NET class when the project is built.</span></span> <span data-ttu-id="d6425-158">生成的类会捕获组件的状态、呈现逻辑、生命周期方法、事件处理程序和其他逻辑。</span><span class="sxs-lookup"><span data-stu-id="d6425-158">The generated class captures the component's state, rendering logic, lifecycle methods, event handlers, and other logic.</span></span> <span data-ttu-id="d6425-159">我们将在[使用 Blazor 生成可重用 UI 组件](./components.md)部分中了解如何创作组件。</span><span class="sxs-lookup"><span data-stu-id="d6425-159">We'll look at authoring components in the [Building reusable UI components with Blazor](./components.md) section.</span></span>

<span data-ttu-id="d6425-160">_Imports.razor 文件不是 Razor 组件文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-160">The *_Imports.razor* files aren't Razor component files.</span></span> <span data-ttu-id="d6425-161">相反，它们定义一组 Razor 指令以导入到相同文件夹及其子文件夹中的其他 .razor 文件中。</span><span class="sxs-lookup"><span data-stu-id="d6425-161">Instead, they define a set of Razor directives to import into other *.razor* files within the same folder and in its subfolders.</span></span> <span data-ttu-id="d6425-162">例如，_Imports.razor 文件是为常用命名空间添加 `using` 指令的常规方法：</span><span class="sxs-lookup"><span data-stu-id="d6425-162">For example, a *_Imports.razor* file is a conventional way to add `using` directives for commonly used namespaces:</span></span>

```razor
@using System.Net.Http
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.JSInterop
@using BlazorApp1
@using BlazorApp1.Shared
```

## <a name="pages"></a><span data-ttu-id="d6425-163">页</span><span class="sxs-lookup"><span data-stu-id="d6425-163">Pages</span></span>

<span data-ttu-id="d6425-164">Blazor 应用中的页面位于何处？</span><span class="sxs-lookup"><span data-stu-id="d6425-164">Where are the pages in the Blazor apps?</span></span> <span data-ttu-id="d6425-165">Blazor 不会为可寻址页面定义单独的文件扩展名，例如 ASP.NET Web Forms 应用中的 .aspx 文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-165">Blazor doesn't define a separate file extension for addressable pages, like the *.aspx* files in ASP.NET Web Forms apps.</span></span> <span data-ttu-id="d6425-166">而是通过将路由分配给组件来定义页面。</span><span class="sxs-lookup"><span data-stu-id="d6425-166">Instead, pages are defined by assigning routes to components.</span></span> <span data-ttu-id="d6425-167">通常使用 `@page` Razor 指令分配路由。</span><span class="sxs-lookup"><span data-stu-id="d6425-167">A route is typically assigned using the `@page` Razor directive.</span></span> <span data-ttu-id="d6425-168">例如，在 Pages/Counter.razor 文件中创作的 `Counter` 组件定义以下路由：</span><span class="sxs-lookup"><span data-stu-id="d6425-168">For example, the `Counter` component authored in the *Pages/Counter.razor* file defines the following route:</span></span>

```razor
@page "/counter"
```

<span data-ttu-id="d6425-169">Blazor 中的路由在客户端（而不是服务器上）进行处理。</span><span class="sxs-lookup"><span data-stu-id="d6425-169">Routing in Blazor is handled client-side, not on the server.</span></span> <span data-ttu-id="d6425-170">当用户在浏览器中导航时，Blazor 会截获导航，然后呈现具有匹配路由的组件。</span><span class="sxs-lookup"><span data-stu-id="d6425-170">As the user navigates in the browser, Blazor intercepts the navigation and then renders the component with the matching route.</span></span>

<span data-ttu-id="d6425-171">组件路由当前不像在 .aspx 页面中那样通过组件的文件位置进行推断。</span><span class="sxs-lookup"><span data-stu-id="d6425-171">The component routes aren't currently inferred by the component's file location like they are with *.aspx* pages.</span></span> <span data-ttu-id="d6425-172">将来可能会添加此功能。</span><span class="sxs-lookup"><span data-stu-id="d6425-172">This feature may be added in the future.</span></span> <span data-ttu-id="d6425-173">必须在组件上显式指定每个路由。</span><span class="sxs-lookup"><span data-stu-id="d6425-173">Each route must be specified explicitly on the component.</span></span> <span data-ttu-id="d6425-174">将可路由组件存储在 Pages 文件夹中没有特殊含义，纯粹是一种约定。</span><span class="sxs-lookup"><span data-stu-id="d6425-174">Storing routable components in a *Pages* folder has no special meaning and is purely a convention.</span></span>

<span data-ttu-id="d6425-175">我们将在[页面、路由和布局](./pages-routing-layouts.md)部分中更详细地了解 Blazor 中的路由。</span><span class="sxs-lookup"><span data-stu-id="d6425-175">We'll look in greater detail at routing in Blazor in the [Pages, routing, and layouts](./pages-routing-layouts.md) section.</span></span>

## <a name="layout"></a><span data-ttu-id="d6425-176">Layout</span><span class="sxs-lookup"><span data-stu-id="d6425-176">Layout</span></span>

<span data-ttu-id="d6425-177">在 ASP.NET Web Forms 应用中，使用母版页 (Site.Master) 处理常见页面布局。</span><span class="sxs-lookup"><span data-stu-id="d6425-177">In ASP.NET Web Forms apps, a common page layout is handled using master pages (*Site.Master*).</span></span> <span data-ttu-id="d6425-178">在 Blazor 应用中，使用布局组件 (Shared/MainLayout.razor) 处理页面布局。</span><span class="sxs-lookup"><span data-stu-id="d6425-178">In Blazor apps, the page layout is handled using layout components (*Shared/MainLayout.razor*).</span></span> <span data-ttu-id="d6425-179">布局组件将在[页面、路由和布局](./pages-routing-layouts.md)部分中更详细地进行讨论。</span><span class="sxs-lookup"><span data-stu-id="d6425-179">Layout components will be discussed in more detail in [Page, routing, and layouts](./pages-routing-layouts.md) section.</span></span>

## <a name="bootstrap-blazor"></a><span data-ttu-id="d6425-180">启动 Blazor</span><span class="sxs-lookup"><span data-stu-id="d6425-180">Bootstrap Blazor</span></span>

<span data-ttu-id="d6425-181">若要启动 Blazor，应用必须：</span><span class="sxs-lookup"><span data-stu-id="d6425-181">To bootstrap Blazor, the app must:</span></span>

- <span data-ttu-id="d6425-182">指定根组件 (App.Razor) 应在页面上的何处进行呈现。</span><span class="sxs-lookup"><span data-stu-id="d6425-182">Specify where on the page the root component (*App.Razor*) should be rendered.</span></span>
- <span data-ttu-id="d6425-183">添加相应的 Blazor 框架脚本。</span><span class="sxs-lookup"><span data-stu-id="d6425-183">Add the corresponding Blazor framework script.</span></span>

<span data-ttu-id="d6425-184">在 Blazor 服务器应用中，根组件的主机页面在 _Host.cshtml 文件中进行定义。</span><span class="sxs-lookup"><span data-stu-id="d6425-184">In the Blazor Server app, the root component's host page is defined in the *_Host.cshtml* file.</span></span> <span data-ttu-id="d6425-185">此文件定义一个 Razor 页面，而不是一个组件。</span><span class="sxs-lookup"><span data-stu-id="d6425-185">This file defines a Razor Page, not a component.</span></span> <span data-ttu-id="d6425-186">Razor Pages 使用 Razor 语法定义服务器可寻址页面（与 .aspx 页面非常类似）。</span><span class="sxs-lookup"><span data-stu-id="d6425-186">Razor Pages use Razor syntax to define a server-addressable page, very much like an *.aspx* page.</span></span> <span data-ttu-id="d6425-187">`Html.RenderComponentAsync<TComponent>(RenderMode)` 方法用于定义根级别组件的呈现位置。</span><span class="sxs-lookup"><span data-stu-id="d6425-187">The `Html.RenderComponentAsync<TComponent>(RenderMode)` method is used to define where a root-level component should be rendered.</span></span> <span data-ttu-id="d6425-188">`RenderMode` 选项指示组件的呈现方式。</span><span class="sxs-lookup"><span data-stu-id="d6425-188">The `RenderMode` option indicates the manner in which the component should be rendered.</span></span> <span data-ttu-id="d6425-189">下表概述了支持的 `RenderMode` 选项。</span><span class="sxs-lookup"><span data-stu-id="d6425-189">The following table outlines the supported `RenderMode` options.</span></span>

|<span data-ttu-id="d6425-190">选项</span><span class="sxs-lookup"><span data-stu-id="d6425-190">Option</span></span>                        |<span data-ttu-id="d6425-191">说明</span><span class="sxs-lookup"><span data-stu-id="d6425-191">Description</span></span>       |
|------------------------------|------------------|
|`RenderMode.Server`           |<span data-ttu-id="d6425-192">建立与浏览器的连接后以交互方式呈现</span><span class="sxs-lookup"><span data-stu-id="d6425-192">Rendered interactively once a connection with the browser is established</span></span>|
|`RenderMode.ServerPrerendered`|<span data-ttu-id="d6425-193">首先预呈现，然后以交互方式呈现</span><span class="sxs-lookup"><span data-stu-id="d6425-193">First prerendered and then rendered interactively</span></span>|
|`RenderMode.Static`           |<span data-ttu-id="d6425-194">呈现为静态内容</span><span class="sxs-lookup"><span data-stu-id="d6425-194">Rendered as static content</span></span>|

<span data-ttu-id="d6425-195">对 _framework/blazor.server.js 的脚本引用会与服务器建立实时连接，然后处理所有用户交互和 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="d6425-195">The script reference to *_framework/blazor.server.js* establishes the real-time connection with the server and then deals with all user interactions and UI updates.</span></span>

```razor
@page "/"
@namespace BlazorApp1.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorApp1</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>
        @(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
</html>
```

<span data-ttu-id="d6425-196">在 Blazor WebAssembly 应用中，主机页面是 wwwroot/index.html 下的简单静态 HTML 文件。</span><span class="sxs-lookup"><span data-stu-id="d6425-196">In the Blazor WebAssembly app, the host page is a simple static HTML file under *wwwroot/index.html*.</span></span> <span data-ttu-id="d6425-197">ID 名为 `app` 的 `<div>` 元素用于指示根组件的呈现位置。</span><span class="sxs-lookup"><span data-stu-id="d6425-197">The `<div>` element with id named `app` is used to indicate where the root component should be rendered.</span></span>

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>BlazorApp2</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/app.css" rel="stylesheet" />
    <link href="blazor-web.styles.css" rel="stylesheet" />
</head>

<body>
    <div id="app">Loading...</div>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    <script src="_framework/blazor.webassembly.js"></script>
</body>

</html>

```

<span data-ttu-id="d6425-198">要呈现的根组件在应用的 `Program.Main` 方法中指定，可通过依赖项注入灵活地注册服务。</span><span class="sxs-lookup"><span data-stu-id="d6425-198">The root component to render is specified in the app's `Program.Main` method with the flexibility to register services through dependency injection.</span></span> <span data-ttu-id="d6425-199">有关详细信息，请参阅 [ASP.NET Core Blazor 依赖项注入](/aspnet/core/blazor/fundamentals/dependency-injection?pivots=webassembly)。</span><span class="sxs-lookup"><span data-stu-id="d6425-199">For more information, see [ASP.NET Core Blazor dependency injection](/aspnet/core/blazor/fundamentals/dependency-injection?pivots=webassembly).</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.RootComponents.Add<App>("#app");

        ....
        ....
    }
}
```

## <a name="build-output"></a><span data-ttu-id="d6425-200">生成输出</span><span class="sxs-lookup"><span data-stu-id="d6425-200">Build output</span></span>

<span data-ttu-id="d6425-201">生成 Blazor 项目时，所有 Razor 组件和代码文件都会编译为单个程序集。</span><span class="sxs-lookup"><span data-stu-id="d6425-201">When a Blazor project is built, all Razor component and code files are compiled into a single assembly.</span></span> <span data-ttu-id="d6425-202">与 ASP.NET Web Forms 项目不同，Blazor 不支持 UI 逻辑的运行时编译。</span><span class="sxs-lookup"><span data-stu-id="d6425-202">Unlike ASP.NET Web Forms projects, Blazor doesn't support runtime compilation of the UI logic.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d6425-203">运行应用</span><span class="sxs-lookup"><span data-stu-id="d6425-203">Run the app</span></span>

<span data-ttu-id="d6425-204">若要运行 Blazor 服务器应用，请在 Visual Studio 中按 `F5`。</span><span class="sxs-lookup"><span data-stu-id="d6425-204">To run the Blazor Server app, press `F5` in Visual Studio.</span></span> <span data-ttu-id="d6425-205">Blazor 应用不支持运行时编译。</span><span class="sxs-lookup"><span data-stu-id="d6425-205">Blazor apps don't support runtime compilation.</span></span> <span data-ttu-id="d6425-206">若要查看代码和组件标记更改的结果，请在附加了调试器的情况下重新生成并重启应用。</span><span class="sxs-lookup"><span data-stu-id="d6425-206">To see the results of code and component markup changes, rebuild and restart the app with the debugger attached.</span></span> <span data-ttu-id="d6425-207">如果在未附加调试器的情况下运行 (`Ctrl+F5`)，则 Visual Studio 会监视文件更改，并在进行更改后重启应用。</span><span class="sxs-lookup"><span data-stu-id="d6425-207">If you run without the debugger attached (`Ctrl+F5`), Visual Studio watches for file changes and restarts the app as changes are made.</span></span> <span data-ttu-id="d6425-208">进行更改后，需手动刷新浏览器。</span><span class="sxs-lookup"><span data-stu-id="d6425-208">You manually refresh the browser as changes are made.</span></span>

<span data-ttu-id="d6425-209">若要运行 Blazor WebAssembly 应用，请选择以下方法之一：</span><span class="sxs-lookup"><span data-stu-id="d6425-209">To run the Blazor WebAssembly app, choose one of the following approaches:</span></span>

- <span data-ttu-id="d6425-210">使用开发服务器直接运行客户端项目。</span><span class="sxs-lookup"><span data-stu-id="d6425-210">Run the client project directly using the development server.</span></span>
- <span data-ttu-id="d6425-211">在使用 ASP.NET Core 托管应用时运行服务器项目。</span><span class="sxs-lookup"><span data-stu-id="d6425-211">Run the server project when hosting the app with ASP.NET Core.</span></span>

<span data-ttu-id="d6425-212">Blazor WebAssembly 应用可以在浏览器和 Visual Studio 中进行调试。有关详细信息，请参阅[调试 ASP.NET Core Blazor WebAssembly](/aspnet/core/blazor/debug)。</span><span class="sxs-lookup"><span data-stu-id="d6425-212">Blazor WebAssembly apps can be debugged in both browser and Visual Studio.See [Debug ASP.NET Core Blazor WebAssembly](/aspnet/core/blazor/debug) for details.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="d6425-213">[上一页](hosting-models.md)
>[下一页](app-startup.md)</span><span class="sxs-lookup"><span data-stu-id="d6425-213">[Previous](hosting-models.md)
[Next](app-startup.md)</span></span>
