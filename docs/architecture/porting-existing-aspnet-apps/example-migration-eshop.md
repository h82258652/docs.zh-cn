---
title: EShop 到 ASP.NET Core 的迁移示例
description: 使用示例在线商店应用作为参考，将现有的 ASP.NET MVC 应用迁移到 ASP.NET Core 的演练。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 119ba64134813fa17848cf9f5fe02cb1a14f8a5d
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122971"
---
# <a name="example-migration-of-eshop-to-aspnet-core"></a>EShop 到 ASP.NET Core 的迁移示例

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

在本章中，你将了解如何将 .NET Framework 应用迁移到 .NET Core。 本章介绍为 ASP.NET 5.0 编写的在线应用商店应用的示例。 此应用将使用本书前面介绍的许多概念和工具。 你将在 [ *eShopModernizing* GitHub 存储库](https://github.com/dotnet-architecture/eShopModernizing)中找到开始点应用。 有几个不同的起点应用。 本章重点介绍 *eShopLegacyMVCSolution*。

项目的初始版本如图4-1 所示。 它是一个相当标准的 ASP.NET MVC 5 应用程序。

![图4-1](media/Figure4-1.png)

**图 4-1**。 *EShopModernizing* MVC 示例项目结构。

本章演示了如何手动执行许多升级步骤。 或者，你可以使用 [.Net 升级助手工具](https://aka.ms/dotnet-upgrade-assistant) 来执行许多初始步骤，例如，转换项目文件、更改目标框架和更新 NuGet 包。

## <a name="run-apiport-to-identify-problematic-apis"></a>运行 *ApiPort* 以识别有问题的 api

准备迁移的第一步是运行 *ApiPort* 工具。 该工具识别应用程序调用的 .NET Framework Api 的数量，其中有多少 .NET Standard 或 .NET Core 等效项。 主要关注自己的应用程序逻辑，而不是第三方依赖项，并注意 `System.Web` 需要移植的依赖项。 ApiPort 工具是在上一章 [了解和更新依赖项](understand-update-dependencies.md)中引入的。

[安装和配置 *ApiPort* 工具](../../standard/analyzers/portability-analyzer.md)后，从 Visual Studio 中运行分析，如图4-2 所示。

![图4-2](media/Figure4-2.png)

**图 4-2**。 分析 Visual Studio 中的程序集可移植性。

从项目的 *bin* 文件夹中选择 web 项目的程序集，如图4-3 所示。

![图4-3](media/Figure4-3.png)

**图 4-3.** 选择项目的 web 程序集。

如果你的解决方案包含多个项目，则可以选择所有项目。 *EShop* 示例只包含单个 MVC 项目。

生成报告后，打开文件并查看结果。 摘要提供对应用正在进行的 .NET Framework 调用的百分比具有兼容版本的高级视图。 图4-4 显示了 *eShop* MVC 项目的摘要。

![图4-4](media/Figure4-4.png)

**图 4-4.** ApiPort 摘要。

对于此应用程序，大约80% 的 .NET Framework 调用是兼容的。 在迁移过程中需要解决20% 的调用。 查看详细信息会发现所有不兼容的调用都是的一部分 `System.Web` ，这是预期的不兼容性。 `System.Web`当在稍后的步骤中迁移应用的控制器和相关类时，将解决调用上的依赖关系。 图4-5 列出了工具发现的一些特定类型：

![图4-5](media/Figure4-5.png)

**图4-5。** *ApiPort* 不兼容的类型详细信息。

大多数不兼容的类型 `Controller` 在 ASP.NET Core 中引用和具有等效项的各种相关属性。

## <a name="update-project-files-and-nuget-reference-syntax"></a>更新项目文件和 NuGet 引用语法

接下来，从较旧的 *.csproj* 文件结构迁移到 .net Core 引入的较新的更简单的结构。 在此过程中，还将从使用 *packages.config* 文件进行 NuGet 引用，以使用 `<PackageReference>` 项目文件中的元素。 旧样式项目文件也可以使用 `<PackageReference>` 元素，因此，在升级到新的项目文件格式之前，最好先将所有 NuGet 包引用迁移为此格式。

原始项目的 *eShopLegacyMVC* 文件长度为418行。 图4-6 显示了项目文件的示例。 为了提供总体大小和复杂性，映像的右边包含了整个文件的缩图视图。

![图4-6](media/Figure4-6.png)

**图 4-6.** *EShopLegacyMVC* 文件结构。

为现有 ASP.NET 项目创建新项目文件的常见方法是 `dotnet new`   >    >  在 Visual Studio 中使用或 "新建 **项目**" 创建新的 ASP.NET Core 应用。 然后，可以将文件从旧项目复制到新项目，以完成迁移。

除了 c # 项目文件，NuGet 依赖项存储在单独的45行 *packages.config* 文件中，如图4-7 中所示。

![图4-7](media/Figure4-7.png)

**图 4-7.** *packages.config* 的文件。

你可以使用 Visual Studio 将 *packages.config* 迁移到类库项目中。 不过，此功能不适用于 ASP.NET 项目。 [详细了解如何将 *packages.config* 迁移到 `<PackageReference>` Visual Studio 中](/nuget/consume-packages/migrate-packages-config-to-package-reference)。 如果要迁移大量项目， [此社区工具可能会有所帮助](https://github.com/MarkKharitonov/NuGetPCToPRMigrator)。 如果要使用工具将项目文件迁移到新格式，则在完成迁移所有 NuGet 引用后，应执行此操作 `<PackageReverence>` 。

## <a name="create-new-aspnet-core-project"></a>创建新 ASP.NET Core 项目

向现有应用程序的解决方案中添加一个新的 ASP.NET Core 项目，以便更轻松地移动文件，因为大部分工作可从 Visual Studio 的 **解决方案资源管理器** 中完成。 在 Visual Studio 中，右键单击应用的解决方案，然后选择 " **添加新项目**"。 选择 **ASP.NET Core web 应用程序**"，并为新项目指定一个名称，如图4-8 所示。

![图4-8](media/Figure4-8.png)

**图4-8。** 添加新的 ASP.NET Core web 应用程序。

下一个对话框将要求您选择要使用的模板。 选择“空”模板。 还要确保将下拉列表从 **.Net Core** 更改为 **.NET Framework**。 选择 **ASP.NET Core 2.2**，如图4-9 中所示。

![图4-9](media/Figure4-9.png)

**图 4-9.** 。 选择针对 .NET Framework ASP.NET Core 2.2 的空项目模板。

### <a name="migrating-nuget-packages"></a>迁移 NuGet 包

由于用于迁移 *packages.config* 的内置迁移工具 `<PackageReference>` 不适用于 ASP.NET 项目，因此可以改为使用社区工具或手动迁移。 [我使用的社区工具](https://gist.github.com/tomkuijsten/2d75074d9a3c19c04ee8ea19a6289ddf)使用 XSL 文件从一种格式转换为另一种格式。 若要使用它，请先将 *packages.config* 文件复制到新创建的 ASP.NET Core 项目文件夹中。 对文件进行备份，因为此脚本将从运行脚本的下的所有文件夹中删除 *packages.config* 文件。 然后，在项目文件夹中运行以下命令 (对于整个解决方案，请按需) ：

```powershell
iwr https://git.io/vdKaV -OutFile Convert-ToPackageReference.ps1
iwr https://git.io/vdKar -OutFile  Convert-ToPackageReference.xsl
./Convert-ToPackageReference.ps1 | Out-Null
```

前两个命令下载文件，使文件在本地存在。 最后一行运行脚本。 运行后，尝试生成新的项目。 您很可能会遇到一些错误。 若要解决这些问题，你需要消除某些引用 (如大多数 `Microsoft.AspNet` 和 `System` 包) ，你可能需要删除一些 `xmlns` 属性。

在大多数 ASP.NET MVC 应用程序中，许多客户端依赖项（如启动和 jQuery）都是使用 NuGet 包部署的。 在 ASP.NET Core 中，NuGet 包仅用于服务器端功能。 应通过其他方式管理客户端文件。 查看 `<PackageReference>` 添加和删除的元素列表，并记下客户端库的任何内容，包括：

- Bootstrap
- jQuery
- jQuery. 验证
- Modernizr
- popper.js
- 响应

NuGet 为这些包安装的静态客户端文件将被复制到新项目的 *wwwroot* 文件夹，并从该文件夹中承载。 值得考虑的是，应用程序是否仍需要这些文件，以及是否有必要继续承载它们或改用 (CDN) 的内容交付网络。 这些库版本可在生成时使用 [LibMan](/aspnet/core/client-side/libman/) 或 [npm](https://www.npmjs.com/)等工具进行管理。 图4-10 显示了使用转换工具（显示和删除不必要的包）迁移包引用后的完整 *eShopPorted* 文件。

![图4-10](media/Figure4-10.png)

**图 4-10.** *EShopPorted* 文件中的包引用。

通过使元素成为特性，可以进一步压缩包引用 `<Version>1.0.0.0</Version>` `Version=1.0.0.0` `<PackageReference>` 。

### <a name="migrate-static-files"></a>迁移静态文件

应用使用的任何静态文件（包括第三方脚本和框架，以及自定义图像和样式表）都必须从旧项目复制到新项目。 在 ASP.NET MVC 应用中，通常会根据文件在项目文件夹中的位置对其进行访问。 在 ASP.NET Core 应用中，将根据其在 *wwwroot* 文件夹中的位置访问这些静态文件。 对于 *eShop* 项目，下列文件夹中有静态文件：

* *内容*
* *字体*
* *映像*
* *Pics*
* *脚本*

在上一步中使用的 **空** 项目模板在默认情况下不包括此文件夹，或在运行时所需的中间件。 需要添加它们。

将 *wwwroot* 文件夹添加到项目的根目录。

添加 NuGet 包的版本 2.2.0 `Microsoft.AspNetCore.StaticFiles` 。

在 *Startup* 中，在方法中添加对的调用 `app.UseStaticFiles()` `Configure` ：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    // ...
}
```

将 *Content* 文件夹从 ASP.NET MVC 应用复制到新项目的 *wwwroot* 文件夹。

运行应用并导航到其 */Content/base.css* 文件夹，以验证是否已正确从其预期路径提供静态文件。 继续将包含静态文件的其余文件夹复制到新项目中。 还需要将 *favicon* 文件从项目的根复制到 *wwwroot* 文件夹。 图4-11 显示了这些文件及其文件夹全部复制后的结果。

![图4-11](media/Figure4-11.png)

**图 4-11.** 静态文件夹复制到 *wwwroot* 文件夹。

### <a name="migrate-c-files"></a>迁移 c # 文件

接下来，复制应用使用的 c # 文件，包括标准 MVC 文件夹及其内容，如 *控制器*、 *模型*、 *ViewModel* 和 *服务*。 在这些文件中很可能需要进行一些更改。 最好一次复制一个文件夹 (或子文件夹) 并进行编译，以查看需要解决的错误。

对于 *eShop* 示例，我选择迁移的第一个文件夹是 " *模型* " 文件夹，其中包括 c # 实体和实体框架类。 此文件夹的类由其他大多数类使用，因此，在复制这些类之前，这些类将不起作用。 复制文件夹和生成后，编译器会显示与缺少命名空间相关的错误、对的 `System.Web.Hosting` 访问 `HostingEnvironment` ，以及对的引用 `ConfigurationManager.AppSettings` 。 这些问题的解决方案将传入必要的路径数据;现在，间断线会被注释掉，并向 `TODO:` 每个线添加注释进行跟踪。 更改5行后， **任务列表** 显示5个项，项目将生成。

接下来，复制 *ViewModel* 文件夹及其一个类。 这是一个简单的过程，可立即生成。

将复制 *服务* 文件夹。 此文件夹的类依赖于 *模型* 文件夹中实体框架类，这就是需要在该文件夹后复制它的原因。 幸运的是，它也不会发生错误。

这会离开 *控制器* 文件夹及其两个 `Controller` 类。 将文件夹复制到新项目并生成后，出现7个生成错误。 其中四个与 access 相关 `ViewBag` ，并报告错误：

> `Missing compiler required member 'Microsoft.CSharp.RuntimeBinder.CSharpArgumentInfo.Create'`

若要解决此错误，请添加对 c # 的 NuGet 包引用：

```xml
<PackageReference Include="Microsoft.CSharp" Version="4.7.0" />
```

其余三个错误指定在未引用的程序集中定义的类型。 具体来说，这些类型：

- `HttpServerUtilityBase`
- `RouteValueDictionary`
- `HttpRequestBase`

让我们逐个查看每个错误。 当尝试引用不再存在的属性时，将发生第一个错误 `Server` `Controller` 。 操作的目标是获取应用中映像文件的路径：

```csharp
if (item != null)
{
    var webRoot = Server.MapPath("~/Pics"); // compiler error on this line
    var path = Path.Combine(webRoot, item.PictureFileName);

    string imageFileExtension = Path.GetExtension(item.PictureFileName);
    string mimetype = GetImageMimeTypeFromImageFileExtension(imageFileExtension);

    var buffer = System.IO.File.ReadAllBytes(path);

    return File(buffer, mimetype);
}
```

此问题有两种可能的解决方法。 第一种方法是使功能保持原样。 在这种情况下， `Server.MapPath` 应使用在 *wwwroot* 中引用图像文件位置的固定路径，而不是使用。 另外，由于此操作方法的唯一目的是返回一个静态图像文件，因此可以更新视图文件中对此操作的引用以直接引用静态文件，从而提高运行时性能。 由于此操作不会进行任何处理，因此没有理由直接为文件提供服务。 如果不 tenable 更新对此操作的所有引用，则可以重写此操作，以生成到静态文件位置的重定向。

下面两个错误在同一代码行的同一私有方法中发生：

```csharp
private void AddUriPlaceHolder(CatalogItem item)
{
    item.PictureUri = this.Url.RouteUrl(PicController.GetPicRouteName, new { catalogItemId = item.Id }, this.Request.Url.Scheme);
}
```

和都将 `this.Url` `this.Request` 导致编译器错误。 了解如何使用此代码，其目的是构建指向 `PicController` 呈现图像文件的操作的链接。 我们刚刚发现的相同的是，可能会被替换为 *wwwroot* 中静态文件的直接链接。 目前，有必要注释掉此代码，并添加 `TODO:` 注释以另一种方式引用图片。

值得注意的 `Controller` 是，此代码出现的类所使用的基类 `CatalogController` 仍引用 `System.Web.Mvc.Controller` 。 在我们更新此更新以使用 ASP.NET Core 后，可能会有更多的修复错误。 首先，删除 `using System.Web.Mvc;` 中的语句列表中的行 `using` `CatalogController` 。 接下来，添加 NuGet 包 `Microsoft.AspNetCore.Mvc` 。 最后，添加一个 `using Microsoft.AspNetCore.Mvc;` 语句，然后重新生成该应用程序。

这次，有16个错误：

- `Include` 不是有效的命名特性参数 (2) 
- `HttpStatusCodeResult` 找不到 (3) 
- `HttpNotFound` 不存在 (3) 
- `SelectList` 找不到 (8) 

接下来，让我们逐个查看这些错误。 首先， `SelectList` 可以通过添加来修复 `using Microsoft.AspNetCore.Mvc.Rendering;` ，这会消除一半错误。

所有对的引用 `return HttpNotFound();` 应替换为 `return NotFound();` 。

所有对的引用 `return new HttpStatusCodeResult(HttpStatusCode.BadRequest);` 应替换为 `return BadRequest();` 。

这只是在 `Include` `[Bind]` 一些操作方法上使用属性，这些方法如下所示：

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create([Bind(Include = "Id,Name,Description,Price,PictureFileName,CatalogTypeId,CatalogBrandId,AvailableStock,RestockThreshold,MaxStockThreshold,OnReorder")] CatalogItem catalogItem)
{
```

前面的代码将模型绑定限制为字符串中列出的属性 `Include` 。 在 ASP.NET Core MVC 中， `[Bind]` 特性仍然存在，但不再需要该 `Include =` 参数。 直接将属性列表传递给 `[Bind]` 特性：

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create([Bind("Id,Name,Description,Price,PictureFileName,CatalogTypeId,CatalogBrandId,AvailableStock,RestockThreshold,MaxStockThreshold,OnReorder")] CatalogItem catalogItem)
{
```

进行这些更改后，项目将再编译一次。 通常，更好的做法是对控制器输入使用单独的模型类型，而不是直接使用模型绑定到域模型或数据模型类型。

## <a name="migrate-views"></a>迁移视图

与视图相关的两个最重要的 ASP.NET Core MVC 功能都是 [Razor Pages](/aspnet/core/razor-pages/) 和 [标记帮助](/aspnet/core/mvc/views/tag-helpers/built-in/)程序。 对于初始迁移，不使用这两个功能。 但是，如果在迁移后继续支持应用，则应记住这些功能。 下一步是将 *Views* 文件夹从原始项目复制到新项目中。 构建后，有九个错误：

- HttpContext 不存在 (2) 
- 脚本不存在 (5) 
- 样式不存在 (1) 
-  (1) 找不到 HtmlString

调查这些错误会发现其中的大多数都位于主 *_Layout cshtml* 中，其中有多个与呈现脚本和样式标记相关，或在上一次重新启动了承载应用程序的服务器时显示。 以下代码列表显示 *_Layout 的 cshtml* 文件中的问题区域：

```razor
// other lines omitted; only errors shown
@Styles.Render("~/Content/css")
@Scripts.Render("~/bundles/modernizr")

@{ var sessionInfo = new HtmlString($"{HttpContext.Current.Session["MachineName"]}, {HttpContext.Current.Session["SessionStartTime"]}");}

@Scripts.Render("~/bundles/jquery")
@Scripts.Render("~/bundles/bootstrap")
```

对 Modernizr 的引用可以删除。 可以将 CDN 链接替换为适当版本的启动和 jQuery 引用。

`@Styles.Render`行替换为：

```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
```

将最后两 `Scripts.Render` 行替换为：

```html
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
```

最后，在引导后 `<link>` ， `<link>` 为应用使用的本地样式添加其他元素。 对于 *eShop*，结果如下所示：

```html
<link rel="stylesheet" href="~/Content/custom.css" />
<link rel="stylesheet" href="~/Content/base.css" />
<link rel="stylesheet" href="~/Content/Site.css" />
```

若要确定元素的显示顺序 `<link>` ，请查看原始应用程序的呈现的 HTML。 另外，请查看 *BundleConfig*，其中 *eShop* 示例包含以下代码来指示适当的顺序：

```csharp
bundles.Add(new StyleBundle("~/Content/css").Include(
          "~/Content/bootstrap.css",
          "~/Content/custom.css",
          "~/Content/base.css",
          "~/Content/site.css"));
```

再次生成会发现在 " *创建* 和 *编辑* " 视图上加载 jQuery 验证会出现一个错误。 将其替换为以下脚本：

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.17.0/jquery.validate.min.js" integrity="sha512-O/nUTF5mdFkhEoQHFn9N5wmgYyW323JO6v8kr6ltSRKriZyTr/8417taVWeabVS4iONGk2V444QD0P2cwhuTkg==" crossorigin="anonymous"></script>
```

视图中要修复的最后一件事是对进行引用以 `Session` 显示应用已运行的时间，以及在哪个计算机上运行的时间。 通过使用和，我们可以直接在站点的 *_Layout* 中显示此数据 `System.Environment.MachineName` `System.Diagnostics.Process.GetCurrentProcess().StartTime` ：

```razor
<section class="col-sm-6">
    <img class="esh-app-footer-text hidden-xs" src="~/images/main_footer_text.png" width="335" height="26" alt="footer text image" />
    <br />
<small>@Environment.MachineName - @System.Diagnostics.Process.GetCurrentProcess().StartTime.ToString() UTC</small>
</section>
```

此时，已成功生成应用程序。 不过，尝试运行它只会产生 *Hello World！* 因为 **空** 的 ASP.NET Core 模板仅配置为响应任何请求而显示。 在下一部分中，我通过将应用程序配置为使用 ASP.NET Core MVC 完成迁移，包括依赖关系注入和配置。 一旦准备就绪，应用就应该运行。 然后，可以修复 `TODO:` 之前创建的任务。

## <a name="migrate-app-startup-components"></a>迁移应用启动组件

最后一个迁移步骤是从 *global.asax* 和它调用的类中获取应用启动任务，并将这些任务迁移到 ASP.NET Core 等效项。 这些任务包括 MVC 本身的配置，设置依赖关系注入，并使用新的配置系统。 在 ASP.NET Core 中，这些任务在 *启动 .cs* 文件中进行处理。

### <a name="configure-mvc"></a>配置 MVC

原始 ASP.NET MVC 应用程序在 global.asax 中具有以下代码 `Application_Start` ， 该应用程序在启动时运行：

```csharp
protected void Application_Start()
{
    container = RegisterContainer();
    AreaRegistration.RegisterAllAreas();
    FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
    RouteConfig.RegisterRoutes(RouteTable.Routes);
    BundleConfig.RegisterBundles(BundleTable.Bundles);
    ConfigDataBase();
}
```

逐个查看这些行， `RegisterContainer` 方法会设置依赖关系注入，这将在下面进行移植。 接下来的三行配置 MVC 的不同部分：区域、筛选器和路由。 绑定由移植应用中的静态文件替换。 最后一行为应用设置数据访问，将在后面的部分中显示。

由于此应用程序实际上不使用区域，因此无需执行任何操作来迁移区域注册呼叫。 如果你的应用程序需要迁移区域，则 [文档将指定如何在 ASP.NET Core 中配置区域](/aspnet/core/mvc/controllers/areas)。

注册全局筛选器的调用会在 `FilterConfig` 应用的 *App_Start* 文件夹中调用类的帮助程序：

```csharp
public static void RegisterGlobalFilters(GlobalFilterCollection filters)
{
    filters.Add(new HandleErrorAttribute());
}
```

添加到应用的唯一属性是 ASP.NET MVC 筛选器 `HandleErrorAttribute` 。 此筛选器可确保当发生异常作为请求的一部分时，将显示默认操作和视图，而不是显示异常详细信息。 在 ASP.NET Core 中，中间件将执行相同的功能 `UseExceptionHandler` 。 默认情况下不启用详细的错误消息。 必须使用中间件来配置这些设置 `UseDeveloperExceptionPage` 。 若要配置此行为以匹配原始应用程序，必须将以下代码添加到 `Configure` *启动* 中方法的开头：

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
    }
    // ...
}
```

这将负责 eShop 应用使用的唯一筛选器，在这种情况下，可以使用内置中间件来完成此操作。 如果你有必须在应用程序中配置的全局筛选器，则当在方法中添加 MVC 时，将会执行此操作 `ConfigureServices` ，本章稍后将会显示。

需要迁移的与 MVC 相关的最后一部分是应用的默认路由。 对的调用 `RouteConfig.RegisterRoutes(RouteTable.Routes)` 会将 MVC 路由表传递给 `RegisterRoutes` helper 方法，在此方法中，应用启动时将执行以下代码：

```csharp
public static void RegisterRoutes(RouteCollection routes)
{
    routes.MapMvcAttributeRoutes();
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Catalog", action = "Index", id = UrlParameter.Optional }
    );
}
```

如果逐行执行此代码，则第一行会设置对属性路由的支持。 这内置于 ASP.NET Core 中，因此不需要单独配置。 同样， *{resource} axd* 文件不与 ASP.NET Core 一起使用，因此无需忽略此类路由。 `MapRoute`方法为使用典型路由模板的 MVC 配置默认值 `{controller}/{action}/{id}` 。 它还指定此模板的默认值，以使 `CatalogController` 成为使用的默认控制器并且 `Index` 方法是默认操作。 较大的应用通常会包含多个对的调用，以 `MapRoute` 设置其他路由。

ASP.NET Core MVC 支持 [传统路由和属性路由](/aspnet/core/mvc/controllers/routing?preserve-view=true&view=aspnetcore-2.2)。 传统路由类似于前面列出的方法中的路由表的配置方式 `RegisterRoutes` 。 若要使用默认路由（如 *eShop* 应用程序中使用的路由）设置传统路由，请将以下代码添加到 `Configure` *Startup* 中方法的底部：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Catalog}/{action=Index}/{id?}");
});
```

> [!NOTE]
> 对于 ASP.NET Core 3.0 及更高版本，此更改为使用终结点。 对于要 ASP.NET Core 2.2 的初始端口，这是用于映射传统路由的正确语法。

进行这些更改 `Configure` 后，就几乎完成了方法。 打印 Hello World 的原始模板 `app.Run` 方法 *！* 应删除。 此时，该方法如下所示：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Catalog}/{action=Index}/{id?}");
    });
}
```

现在是时候配置 MVC 服务，接下来是应用程序对依赖关系注入的支持 (DI) 。 到目前为止， *eShopPorted* 项目的 `ConfigureServices` 方法保持为空。 现在是时候开始填充它了。

首先，若要使 ASP.NET Core MVC 正常工作，需要添加：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
}
```

前面的代码是使 MVC 功能正常工作所需的最低配置。 此调用可以配置许多其他功能 (其中一些功能在本章后面的部分进行了详细介绍) ，但是现在这就足以构建应用了。 现在运行它会正确路由默认请求，但由于尚未配置 DI，因此在激活时出现错误， `CatalogController` 因为尚未提供类型的实现 `ICatalogService` 。 稍后我们将再次配置 MVC。 现在，让我们迁移应用程序的依赖关系注入。

#### <a name="migrate-dependency-injection-configuration"></a>迁移依赖关系注入配置

原始应用程序的 *global.asax* 文件定义以下方法，在应用启动时调用该方法：

```csharp
protected IContainer RegisterContainer()
{
  var builder = new ContainerBuilder();

  builder.RegisterControllers(typeof(MvcApplication).Assembly);

  var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
  builder.RegisterModule(new ApplicationModule(mockData));

  var container = builder.Build();
  DependencyResolver.SetResolver(new AutofacDependencyResolver(container));

  return container;
}
```

此代码配置一个 [Autofac](https://autofac.org/) 容器，读取一个配置设置以确定是否应使用真实数据或模拟数据，并将此设置传递到 Autofac 模块 (在应用的 */Modules* 目录) 中找到。 幸运的是，Autofac 支持 .NET Core，因此模块可以直接迁移。 将文件夹复制到新项目并更新类的命名空间，并进行编译。

ASP.NET Core 提供对依赖关系注入的内置支持，但是，如果需要，可以轻松地连接到 Autofac 等第三方容器。 在这种情况下，因为已将应用程序配置为使用 Autofac，所以最简单的解决方案是保持其使用。 为此， `ConfigureServices` 必须修改方法签名以返回 `IServiceProvider` ，并从方法配置和返回 Autofac 容器实例。

**注意：** 在 .NET Core 3.0 及更高版本中，集成第三方 DI 容器的过程已更改。

配置 Autofac 的一部分需要调用 `builder.Populate(services)` 。 此扩展在 `Autofac.Extensions.DependencyInjection` NuGet 包中找到，在编译代码之前，必须先安装此扩展。

在修改 `ConfigureServices` 以配置 Autofac 容器后，新的方法如下所示：

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    // Create Autofac container builder
    var builder = new ContainerBuilder();
    builder.Populate(services);
    bool useMockData = true; // TODO: read from config
    builder.RegisterModule(new ApplicationModule(useMockData));

    ILifetimeScope container = builder.Build();

    return new AutofacServiceProvider(container);
}
```

目前，的设置 `useMockData` 设置为 `true` 。 此设置将在一段时间内从配置中读取。 此时，应用会在运行时编译并成功加载，如图4-12 所示。

![图4-12](media/Figure4-12.png)

**图 4-12.** 在本地运行的、带有模拟数据的 *eShop* 应用。

#### <a name="migrate-app-settings"></a>迁移应用设置

ASP.NET Core 使用一个新的 [配置系统](/aspnet/core/fundamentals/configuration/?preserve-view=true&view=aspnetcore-2.2)，默认情况下，该系统利用文件 *上的appsettings.js* 。 通过 `CreateDefaultBuilder` 在 *.cs* 中使用，已在应用程序中设置了默认配置。 若要访问配置，类只需在其构造函数中请求它即可。 `Startup`类不是异常。 若要开始访问中的配置 `Startup` 和应用程序的其余部分，请 `IConfiguration` 从其构造函数中请求的实例：

```csharp
public Startup(IConfiguration configuration)
{
    Configuration = configuration;
}

public IConfiguration Configuration { get; }
```

原始应用使用引用其设置 `ConfigurationManager.AppSettings` 。 此术语的所有引用的快速搜索将生成新应用所需的一组设置。 只有两个：

- `UseMockData`
- `UseCustomizationData`

如果应用具有更复杂的配置（尤其是在使用自定义配置节时），则可能需要创建对象并将其绑定到应用配置的不同部分。 然后，可以使用 [options 模式](../../core/extensions/options.md)访问这些类型。 但是，如引用的文档中所述，此模式不应在中使用 `ConfigureServices` 。 相反，移植应用会直接引用 `UseMockData` 配置值。

首先，修改端口上的应用 `appsettings.json` 文件，并在根中添加这两个设置：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "UseMockData": "true",
  "UseCustomizationData" :  "true"
}
```

现在，请修改 `ConfigureServices` 以 `UseMockData` 从属性 (访问设置， `Configuration` 先前我们将值设置为 `true`) ：

```csharp
  bool useMockData = Configuration.GetValue<bool>("UseMockData");
```

此时，该设置是从配置中提取的。 其他设置 `UseCustomizationData` 由 `CatalogDBInitializer` 类使用。 首次移植此类时，已注释掉对的访问 `ConfigurationManager.AppSettings["UseCustomizationData"]` 。 现在可以将其修改为使用 ASP.NET Core 配置。 修改的构造函数 `CatalogDBInitializer` ，如下所示：

```csharp
  // add using Microsoft.Extensions.Configuration
  public CatalogDBInitializer(CatalogItemHiLoGenerator indexGenerator,
      IConfiguration configuration)
  {
      this.indexGenerator = indexGenerator;
      useCustomizationData = configuration.GetValue<bool>("UseCustomizationData");
  }
```

应以这种方式修改对 web 应用中的配置的所有访问权限，以使用新 `IConfiguration` 类型。 需要访问 .NET Framework 配置的依赖项可能会在添加到 web 项目的 *app.config* 文件中包含此类设置。 依赖项目可用于 `ConfigurationManager` 访问设置，如果已使用此方法，则不应要求进行任何更改。 不过，由于 ASP.NET Core 应用作为其自己的可执行文件运行，因此它们不会引用 *web.config* ，而是 *app.config*。通过将旧应用程序 *web.config* 文件中的设置迁移到 ASP.NET Core 应用程序中的新 *app.config* 文件，用于 `ConfigurationManager` 访问其设置的组件将继续正常工作。

应用的迁移即将完成。 唯一剩余的任务是数据访问配置。
  
## <a name="data-access-considerations"></a>数据访问注意事项

.NET Framework 上运行的 ASP.NET Core 应用可以继续利用 (EF) 的实体框架。 如果执行增量迁移，则在尝试将应用程序的数据访问移植到使用 EF Core 时，应用程序将使用 EF 6。 通过这种方式，可以在另一个迁移工作块开始之前确定和解决应用的任何迁移问题。

在这种情况下，在 eShop 示例迁移中配置 EF 6 并不需要任何特殊工作，因为此操作是在 Autofac 中执行的 `ApplicationModule` 。 唯一的问题是，类当前 `CatalogDBContext` 尝试从 *web.config* 读取其连接字符串。若要解决此情况，需要将连接详细信息添加到 *appsettings.js*。 然后，在创建连接字符串时必须将其传递到 `CatalogDBContext` 。

更新 *appsettings.js* 以包含连接字符串。 下面列出了完整的文件：

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=eShopPorted;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "UseMockData": "false",
  "UseCustomizationData": "true"
}
```

创建时，必须将连接字符串传递到构造函数中 `DbContext` 。 由于实例是由 Autofac 创建的，因此需要在中进行更改 `ApplicationModule` 。 修改模块以在 `connectionString` 其构造函数中采用，并将其分配给字段。 然后修改的注册 `CatalogDBContext` ，将连接字符串作为参数添加：

```csharp
builder.RegisterType<CatalogDBContext>()
  .WithParameter("connectionString", _connectionString)
  .InstancePerLifetimeScope();
```

还必须将参数添加到新的构造函数重载 `CatalogDBContext` 本身中：

```csharp
public CatalogDBContext(string connectionString) : base(connectionString)
{
}
```

最后， `ConfigureServices` 必须从中读取连接字符串 `Config` ，并在 `ApplicationModule` 其实例化时将其传递到：

```csharp
bool useMockData = Configuration.GetValue<bool>("UseMockData");
string connectionString = Configuration.GetConnectionString("DefaultConnection");
builder.RegisterModule(new ApplicationModule(useMockData, connectionString));
```

使用此代码后，应用程序将像以前一样运行，在为时连接到 SQL Server 的 `UseMockData` 数据库 `false` 。

此时，可以在生产环境中部署和运行该应用，转换为 ASP.NET Core 但仍在 .NET Framework 和 EF 6 上运行。 如果需要，可以将应用迁移到 .NET Core 和 Entity Framework Core 上运行，这会带来前面几章所述的其他优势。 此文档特定于实体框架， [它将 EF Core 和 EF 6 进行比较](/ef/efcore-and-ef6/) ，并包含一个网格，其中显示了哪些库支持每个单个功能。

### <a name="migrate-to-entity-framework-core"></a>迁移到 Entity Framework Core

假设决定迁移到 EF Core，则步骤可能相当简单，特别是在原始应用使用基于代码的模型方法时。 [准备从 EF 6 到 EF Core 的端口](/ef/efcore-and-ef6/porting/)时，请查看要使用 EF Core 的目标版本中的功能可用性。 查看有关[从和基于 EDMX 的模型的迁移以及](/ef/efcore-and-ef6/porting/port-edmx)[从基于代码的模型移植](/ef/efcore-and-ef6/porting/port-code)的文档。

若要升级到 EF Core 2.2，所涉及的基本步骤是添加适当的 NuGet 包 (s) 和更新命名空间。 然后调整如何将连接字符串传递到 `DbContext` 类型，以及如何将其连接到依赖关系注入。

EF Core 作为包引用添加到项目中：

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="2.2.6" />
```

移除了对 EF 6 的引用：

```xml
<PackageReference Include="EntityFramework" Version="6.2.0" />
```

编译器将在和中报告 `CatalogDBContext` 错误 `CatalogDBInitializer` 。 `CatalogDbContext` 需要删除旧的命名空间，并将其替换为 `Microsoft.EntityFrameworkCore` 。 可删除其构造函数。 `DbModelBuilder` 应该替换为 `ModelBuilder` 。 用于配置类型的帮助器方法将移动到实现的单独类中 `IEntityTypeConfiguration<T>` 。 然后， `CatalogDBContext` 类的 `OnModelCreating` 方法只是：

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

    base.OnModelCreating(builder);
}
```

涉及的其他更改包括：

- `HasDatabaseGeneratedOption(DatabaseGeneratedOption.None)` 替换为 `ValueGeneratedNever()`
- `HasRequired<T>` 替换为 `HasOne<T>`
- 已安装 `Microsoft.EntityFrameworkCore.Relational` 包
- 添加构造函数以 `CatalogDBContext` 采用 `DbContextOptions` 并将其传递到基构造函数

的示例配置类如下 `CatalogType` 所示：

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata.Builders;

namespace eShopPorted.Models.Config
{
    public class CatalogTypeConfig : IEntityTypeConfiguration<CatalogType>
    {
        public void Configure(EntityTypeBuilder<CatalogType> builder)
        {
            builder.ToTable(nameof(CatalogType));

            builder.HasKey(ci => ci.Id);

            builder.Property(ci => ci.Id)
               .IsRequired();

            builder.Property(cb => cb.Type)
                .IsRequired()
                .HasMaxLength(100);
        }
    }
}
```

`CatalogDBInitializer`及其基类 `CreateDatabaseIfNotExists<T>` 与 EF Core 不兼容。 此类的目的是创建数据库并为数据库创建种子。 使用 EF Core 将使用以下方法[创建并删除关联的 `DbContext` 数据库](/ef/core/managing-schemas/ensure-created)：

```csharp
dbContext.Database.EnsureDeleted();
dbContext.Database.EnsureCreated();
```

可以通过手动脚本或作为类型配置的一部分来实现 EF Core 中的数据种子设定。 除了其他实体属性，还可以使用在类中配置种子数据 `IEntityTypeConfiguration` `builder.HasData()` 。 原始应用已从 *安装* 目录中的 CSV 文件加载种子数据。 假设只有少量项，则可以将这些数据记录作为实体配置的一部分添加。 此方法非常适用于不经常更改的表中的查找数据。 将以下方法添加到 `CatalogTypeConfig` 的 `Configure` 方法可确保在创建数据库时存在相关行：

```csharp
builder.HasData(
    new CatalogType { Id = 1, Type = "Mug" },
    new CatalogType { Id = 2, Type = "T-Shirt" },
    new CatalogType { Id = 3, Type = "Sheet" },
    new CatalogType { Id = 4, Type = "USB Memory Stick" }
);
```

初始应用包含一个 `PreconfiguredData` 类，该类包含和的数据 `CatalogBrand` `CatalogType` ，因此使用此方法时， `HasData` 调用将减少到：

```csharp
builder.HasData(
    PreconfiguredData.GetPreconfiguredCatalogBrands()
);
```

`CatalogItem`还可以从中提取数据 `PreconfiguredData` ，假定关联的图像保留在源代码管理中，这是应用程序正常运行所需的最后一个表。 `CatalogDBInitializer`类可以与任何对它的引用一起被移除。 `CatalogItemHiLoGenerator`还将删除目录中的类和 SQL 文件，并在 `Infrastructure`) 中删除对这些文件的任何引用 (`CatalogService` `ApplicationModule` 。

由于消除了的特殊密钥生成器类 `CatalogItem` ，此代码现在将从 `CatalogItemConfig` 以下内容中删除：

```csharp
builder.Property(ci => ci.Id)
    .ValueGeneratedNever()
    .IsRequired();
```

进行这些修改后，ASP.NET Core 应用程序将生成，但它尚不能用于 EF Core，但仍必须配置为依赖项注入。 在 EF Core 中，对其进行配置的最简单方法是在中 `ConfigureServices` ：

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    bool useMockData = Configuration.GetValue<bool>("UseMockData");
    if (!useMockData)
    {
        string connectionString = Configuration.GetConnectionString("DefaultConnection");

        services.AddDbContext<CatalogDBContext>(options =>
            options.UseSqlServer(connectionString)
        );
    }

    // Create Autofac container builder
    var builder = new ContainerBuilder();
    builder.Populate(services);
    builder.RegisterModule(new ApplicationModule(useMockData));

    ILifetimeScope container = builder.Build();

    return new AutofacServiceProvider(container);
}
```

Autofac 的最终版本 `ApplicationModule` 仅配置一种类型，具体取决于应用是否已配置为使用模拟数据：

```csharp
public class ApplicationModule : Module
{
    private bool _useMockData;

    public ApplicationModule(bool useMockData)
    {
        _useMockData = useMockData;
    }

    protected override void Load(ContainerBuilder builder)
    {
        if (_useMockData)
        {
            builder.RegisterType<CatalogServiceMock>()
                .As<ICatalogService>()
                .SingleInstance();
        }
        else
        {
            builder.RegisterType<CatalogService>()
                .As<ICatalogService>()
                .InstancePerLifetimeScope();
        }
    }
}
```

端口应用将运行，但如果配置为使用非模拟数据，则不会显示任何数据。 `HasData`仅在应用迁移时插入通过添加的种子数据。 源应用未使用迁移，如果存在，则不会按原样迁移。 最好的方法是从新的迁移脚本开始。 为此，请添加的包引用， `Microsoft.EntityFrameworkCore.Design` 并在项目根目录中打开一个终端窗口。 然后运行：

```dotnetcli
dotnet ef migrations add Initial
```

删除现有的 *eShopPorted* 数据库（如果存在），然后运行：

```dotnetcli
dotnet ef database update
```

这会创建数据库并将其作为种子。 现已准备好运行，其中包含几个较小的小更新可解决。

## <a name="fix-all-todo-tasks"></a>修复所有 TODO 任务

此时运行移植应用会显示页面上不显示图片。 这是因为 `PictureUri` `CatalogItem` 从不设置的属性。 查看 `TODO` 我们使用 Visual Studio **任务列表** 创建的项的列表，只保留处于中的项 `CatalogController` ，并提供 "从 wwwroot 引用 pic" 的注释。 相关代码是：

```csharp
private void AddUriPlaceHolder(CatalogItem item)
{
    //TODO: Reference pic from wwwroot
    //item.PictureUri = this.Url.RouteUrl(PicController.GetPicRouteName, new { catalogItemId = item.Id }, this.Request.Url.Scheme);
}
```

最简单的解决方法是在站点的公共 *wwwroot/图片* 目录中引用公共映像文件。 可以通过将方法替换为以下代码来完成此任务：

```csharp
private void AddUriPlaceHolder(CatalogItem item)
{
    item.PictureUri = $"/Pics/{item.Id}.png";
}
```

进行此更改后，运行应用程序将使图像像以前一样工作。

## <a name="additional-mvc-customizations"></a>其他 MVC 自定义

*EShopLegacyMVC* 应用程序非常简单，因此在默认 MVC 行为方面没有太多的配置。 但是，如果确实需要配置其他 MVC 组件（如 CORS、筛选器和路由约束），则通常会在中提供此信息 `Startup.ConfigureServices` ，其中 `UseMvc` 调用。 例如，下面的代码列表配置 [CORS](/aspnet/core/security/cors?preserve-view=true&view=aspnetcore-2.2) 并设置全局操作筛选器：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy(MyAllowSpecificOrigins,
            builder =>
                builder.WithOrigins("http://example.com", "http://www.contoso.com")
                    .AllowAnyHeader()
                    .AllowAnyMethod());
    });

    services.AddMvc(options =>
    {
      options.Filters.Add(new SampleGlobalActionFilter());
    }).SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

> [!Note]
> 若要完成 CORS 配置，还必须 `app.UseCors()` 在中调用 `Configure` 。

详细的 ASP.NET Core 文档中介绍了其他高级方案，如添加 [自定义模型联编](/aspnet/core/mvc/advanced/custom-model-binding?preserve-view=true&view=aspnetcore-2.2)程序、格式化程序等。通常，这些应用程序可以应用于单个控制器或操作基础上，也可以使用上述代码列表中所示的相同选项方法全局应用。

## <a name="other-dependencies"></a>其他依赖项

使用依赖于旧配置模型的 .NET Framework 功能的依赖项（例如 WCF 客户端类型和跟踪代码）必须在移植时修改。 它们不是直接请求配置信息，而是应在代码中进行配置。 例如，与在 ASP.NET 应用程序的 *web.config* 中配置的 WCF 服务的连接，可以使用 `basicHttpBinding` 以下代码以编程方式进行配置：

```csharp
var binding = new BasicHttpBinding();
binding.MaxReceivedMessageSize = 2_000_000;

var endpointAddress = new EndpointAddress("http://localhost:9200/ExampleService");

var myClient = new MyServiceClient(binding, endpointAddress);
```

WCF 客户端和其他 .NET Framework 类型应在代码中指定其设置，而不是依赖于配置文件的设置。 通过这种方式进行配置，这些类型可在 ASP.NET Core 2.2 应用中继续工作。

## <a name="references"></a>参考

- [eShopModernizing GitHub 存储库](https://github.com/dotnet-architecture/eShopModernizing)
- [.NET 升级助手工具](https://aka.ms/dotnet-upgrade-assistant)
- [API 和 Viewmodel 不应引用域模型](https://ardalis.com/your-api-and-view-models-should-not-reference-domain-models/)
- [开发人员异常页中间件](/aspnet/core/fundamentals/error-handling#developer-exception-page)
- [深入了解 EF Core HasData](/archive/msdn-magazine/2018/august/data-points-deep-dive-into-ef-core-hasdata-seeding)

>[!div class="step-by-step"]
>[上一页](more-migration-scenarios.md)
>[下一页](deployment-scenarios.md)
