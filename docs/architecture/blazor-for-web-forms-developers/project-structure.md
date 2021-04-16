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
# <a name="project-structure-for-blazor-apps"></a>Blazor 应用的项目结构

尽管 ASP.NET Web Forms 和 Blazor 的项目结构存在重大差异，但它们具有许多类似的概念。 在这里，我们将了解 Blazor 项目的结构，并将它与 ASP.NET Web Forms 项目进行比较。

若要创建第一个 Blazor 应用，请按照 [Blazor 入门步骤](/aspnet/core/blazor/get-started)中的说明进行操作。 可以按照说明创建 Blazor 服务器应用或是在 ASP.NET Core 中托管的 Blazor WebAssembly 应用。 除了特定于托管模型的逻辑外，这两个项目中的大多数代码是相同的。

## <a name="project-file"></a>项目文件

Blazor 服务器应用是 .NET 项目。 Blazor 服务器应用的项目文件可如同下面一样简单：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```

Blazor WebAssembly 应用的项目文件看起来稍微复杂一些（确切的版本号可能有所不同）：

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

Blazor WebAssembly 项目面向 `Microsoft.NET.Sdk.BlazorWebAssembly` 而不是 `Microsoft.NET.Sdk.Web` sdk，因为它们在基于 WebAssembly 的 .NET 运行时上的浏览器中运行。 不能如同在服务器或开发人员计算机上那样将 .NET 安装到 Web 浏览器中。 因此，项目会使用单个包引用来引用 Blazor 框架。

相比之下，默认 ASP.NET Web Forms 项目在其 .csproj 文件中包含将近 300 行 XML，大多数这种文件会显式列出项目中的各种代码和内容文件。 随着 `.NET 5` 的发布，`Blazor Server` 和 `Blazor WebAssembly` 应用可以轻松共享一个统一的运行时。

虽然单个程序集引用受支持，但是它们在 .NET 项目中不太常见。 大多数项目依赖项都作为 NuGet 包引用进行处理。 只需在 .NET 项目中引用顶级包依赖项。 可传递的依赖项会自动包含在内。 会使用 `<PackageReference>` 元素将包引用添加到项目文件，而不是使用 ASP.NET Web Forms 项目中常见的 packages.config 文件来引用包。

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
</ItemGroup>
```

## <a name="entry-point"></a>入口点

Blazor 服务器应用的入口点在 Program.cs 文件中进行定义，与控制台应用一样。 当应用执行时，它会使用特定于 Web 应用的默认值创建并运行 Web 主机实例。 Web 主机会管理 Blazor 服务器应用的生命周期，并设置主机级别服务。 此类服务的示例包括配置、日志记录、依赖项注入和 HTTP 服务器。 此代码主要作为样板，通常保持不变。

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

Blazor WebAssembly 应用也在 Program.cs 中定义入口点。 代码看起来略有不同。 该代码的相似之处在于，它会设置应用主机以便向应用提供相同的主机级别服务。 不过，WebAssembly 应用主机不会设置 HTTP 服务器，因为它直接在浏览器中执行。

Blazor 应用具有 `Startup` 类（而不是 Global.asax 文件），用于定义应用的启动逻辑。 `Startup` 类用于配置应用和任何特定于应用的服务。 在 Blazor 服务器应用中，`Startup` 类用于为 Blazor 在客户端浏览器与服务器之间使用的实时连接设置终结点。 在 Blazor WebAssembly 应用中，`Startup` 类定义应用的根组件及其呈现位置。 我们将在[应用启动](./app-startup.md)部分中更深入地了解 `Startup` 类。

## <a name="static-files"></a>静态文件

与 ASP.NET Web Forms 项目不同，并非 Blazor 项目中的所有文件都可以作为静态文件进行请求。 只有 wwwroot 文件夹中的文件可进行 Web 寻址。 此文件夹被称为应用的“Web 根目录”。 应用的 Web 根目录之外的任何内容都不可进行 Web 寻址。 此设置可提供额外的安全级别，防止在 Web 上意外公开项目文件。

## <a name="configuration"></a>配置

ASP.NET Web Forms 应用中的配置通常使用一个或多个 web.config 文件进行处理。 Blazor 应用通常没有 web.config 文件。 如果它们有这类文件，则该文件仅用于在 IIS 上进行托管时配置特定于 IIS 的设置。 相反，Blazor 服务器应用使用 ASP.NET Core 配置抽象（Blazor WebAssembly 应用当前不支持相同的配置抽象，但这可能是会在将来添加的一种功能）。 例如，默认 Blazor 服务器应用会将某些设置存储在 appsettings.json 中。

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

我们将在[配置](./config.md)部分中详细了解 ASP.NET Core 项目中的配置。

## <a name="razor-components"></a>Razor 组件

Blazor 项目中的大多数文件是 .razor 文件。 Razor 是一种基于 HTML 和 C# 的模板化语言，用于动态生成 Web UI。 .razor 文件定义组成应用 UI 的组件。 大多数情况下，组件对于 Blazor 服务器和 Blazor WebAssembly 应用是相同的。 Blazor 中的组件类似于 ASP.NET Web Forms 中的用户控件。

生成项目时，每个 Razor 组件文件都会编译为 .NET 类。 生成的类会捕获组件的状态、呈现逻辑、生命周期方法、事件处理程序和其他逻辑。 我们将在[使用 Blazor 生成可重用 UI 组件](./components.md)部分中了解如何创作组件。

_Imports.razor 文件不是 Razor 组件文件。 相反，它们定义一组 Razor 指令以导入到相同文件夹及其子文件夹中的其他 .razor 文件中。 例如，_Imports.razor 文件是为常用命名空间添加 `using` 指令的常规方法：

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

## <a name="pages"></a>页

Blazor 应用中的页面位于何处？ Blazor 不会为可寻址页面定义单独的文件扩展名，例如 ASP.NET Web Forms 应用中的 .aspx 文件。 而是通过将路由分配给组件来定义页面。 通常使用 `@page` Razor 指令分配路由。 例如，在 Pages/Counter.razor 文件中创作的 `Counter` 组件定义以下路由：

```razor
@page "/counter"
```

Blazor 中的路由在客户端（而不是服务器上）进行处理。 当用户在浏览器中导航时，Blazor 会截获导航，然后呈现具有匹配路由的组件。

组件路由当前不像在 .aspx 页面中那样通过组件的文件位置进行推断。 将来可能会添加此功能。 必须在组件上显式指定每个路由。 将可路由组件存储在 Pages 文件夹中没有特殊含义，纯粹是一种约定。

我们将在[页面、路由和布局](./pages-routing-layouts.md)部分中更详细地了解 Blazor 中的路由。

## <a name="layout"></a>Layout

在 ASP.NET Web Forms 应用中，使用母版页 (Site.Master) 处理常见页面布局。 在 Blazor 应用中，使用布局组件 (Shared/MainLayout.razor) 处理页面布局。 布局组件将在[页面、路由和布局](./pages-routing-layouts.md)部分中更详细地进行讨论。

## <a name="bootstrap-blazor"></a>启动 Blazor

若要启动 Blazor，应用必须：

- 指定根组件 (App.Razor) 应在页面上的何处进行呈现。
- 添加相应的 Blazor 框架脚本。

在 Blazor 服务器应用中，根组件的主机页面在 _Host.cshtml 文件中进行定义。 此文件定义一个 Razor 页面，而不是一个组件。 Razor Pages 使用 Razor 语法定义服务器可寻址页面（与 .aspx 页面非常类似）。 `Html.RenderComponentAsync<TComponent>(RenderMode)` 方法用于定义根级别组件的呈现位置。 `RenderMode` 选项指示组件的呈现方式。 下表概述了支持的 `RenderMode` 选项。

|选项                        |说明       |
|------------------------------|------------------|
|`RenderMode.Server`           |建立与浏览器的连接后以交互方式呈现|
|`RenderMode.ServerPrerendered`|首先预呈现，然后以交互方式呈现|
|`RenderMode.Static`           |呈现为静态内容|

对 _framework/blazor.server.js 的脚本引用会与服务器建立实时连接，然后处理所有用户交互和 UI 更新。

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

在 Blazor WebAssembly 应用中，主机页面是 wwwroot/index.html 下的简单静态 HTML 文件。 ID 名为 `app` 的 `<div>` 元素用于指示根组件的呈现位置。

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

要呈现的根组件在应用的 `Program.Main` 方法中指定，可通过依赖项注入灵活地注册服务。 有关详细信息，请参阅 [ASP.NET Core Blazor 依赖项注入](/aspnet/core/blazor/fundamentals/dependency-injection?pivots=webassembly)。

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

## <a name="build-output"></a>生成输出

生成 Blazor 项目时，所有 Razor 组件和代码文件都会编译为单个程序集。 与 ASP.NET Web Forms 项目不同，Blazor 不支持 UI 逻辑的运行时编译。

## <a name="run-the-app"></a>运行应用

若要运行 Blazor 服务器应用，请在 Visual Studio 中按 `F5`。 Blazor 应用不支持运行时编译。 若要查看代码和组件标记更改的结果，请在附加了调试器的情况下重新生成并重启应用。 如果在未附加调试器的情况下运行 (`Ctrl+F5`)，则 Visual Studio 会监视文件更改，并在进行更改后重启应用。 进行更改后，需手动刷新浏览器。

若要运行 Blazor WebAssembly 应用，请选择以下方法之一：

- 使用开发服务器直接运行客户端项目。
- 在使用 ASP.NET Core 托管应用时运行服务器项目。

Blazor WebAssembly 应用可以在浏览器和 Visual Studio 中进行调试。有关详细信息，请参阅[调试 ASP.NET Core Blazor WebAssembly](/aspnet/core/blazor/debug)。

>[!div class="step-by-step"]
>[上一页](hosting-models.md)
>[下一页](app-startup.md)
