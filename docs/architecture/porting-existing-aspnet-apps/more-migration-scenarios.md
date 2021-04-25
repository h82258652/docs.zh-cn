---
title: 更多迁移方案
description: 本部分介绍将 .NET Framework 应用升级到 .NET Core/.NET 5 的其他迁移方案和技术。
author: ardalis
ms.date: 02/11/2021
ms.openlocfilehash: da710644c5c17c3d701cc8d492481a076a691eeb
ms.sourcegitcommit: 5cad087ed0cfc4cf632b6d8270ed311e4d8b5217
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2021
ms.locfileid: "107928587"
---
# <a name="more-migration-scenarios"></a><span data-ttu-id="3d09c-103">更多迁移方案</span><span class="sxs-lookup"><span data-stu-id="3d09c-103">More migration scenarios</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="3d09c-104">本部分介绍了几种不同的 ASP.NET 应用方案，并提供了解决每个应用方案的特定方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-104">This section describes several different ASP.NET app scenarios, and offers specific techniques for solving each of them.</span></span> <span data-ttu-id="3d09c-105">你可以使用此部分来确定适用于你的应用程序的方案，并评估适用于你的应用程序及其宿主环境的哪些方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-105">You can use this section to identify scenarios applicable to your app, and evaluate which techniques will work for your app and its hosting environment.</span></span>

## <a name="migrate-aspnet-mvc-5-and-webapi-2-to-aspnet-core-mvc"></a><span data-ttu-id="3d09c-106">将 ASP.NET MVC 5 和 WebApi 2 迁移到 ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="3d09c-106">Migrate ASP.NET MVC 5 and WebApi 2 to ASP.NET Core MVC</span></span>

<span data-ttu-id="3d09c-107">ASP.NET MVC 5 和 Web API 2 应用的常见方案是，这两个产品都安装在同一应用程序中。</span><span class="sxs-lookup"><span data-stu-id="3d09c-107">A common scenario in ASP.NET MVC 5 and Web API 2 apps was for both products to be installed in the same application.</span></span> <span data-ttu-id="3d09c-108">这是一种支持的、对许多团队使用的相对常见方法，但由于这两种产品使用不同的抽象，因此需要执行一些冗余工作。</span><span class="sxs-lookup"><span data-stu-id="3d09c-108">This is a supported and relatively common approach used by many teams, but because the two products use different abstractions, there is some redundant effort needed.</span></span> <span data-ttu-id="3d09c-109">例如，设置 ASP.NET MVC 的路由是使用上的方法（ `RouteCollection` 如和）完成 `MapMvcAttributeRoutes()` 的 `MapRoute()` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-109">For example, setting up routes for ASP.NET MVC is done using methods on `RouteCollection`, such as `MapMvcAttributeRoutes()` and `MapRoute()`.</span></span> <span data-ttu-id="3d09c-110">但 ASP.NET Web API 2 路由由和等 `HttpConfiguration` 方法管理 `MapHttpAttributeRoutes()` `MapHttpRoute()` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-110">But ASP.NET Web API 2 routing is managed with `HttpConfiguration` and methods like `MapHttpAttributeRoutes()` and `MapHttpRoute()`.</span></span>

<span data-ttu-id="3d09c-111">此 `eShopLegacyMVC` 应用包括 ASP.NET MVC 和 WEB API，并在其文件夹中包含 `App_Start` 用于为两者设置路由的方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-111">The `eShopLegacyMVC` app includes both ASP.NET MVC and Web API, and includes methods in its `App_Start` folder for setting up routes for both.</span></span> <span data-ttu-id="3d09c-112">它还支持使用 Autofac 进行依赖关系注入，这还需要配置两组相似的工作：</span><span class="sxs-lookup"><span data-stu-id="3d09c-112">It also supports dependency injection using Autofac, which also requires two sets of similar work to configure:</span></span>

```csharp
protected IContainer RegisterContainer()
{
    var builder = new ContainerBuilder();

    var thisAssembly = Assembly.GetExecutingAssembly();
    builder.RegisterControllers(thisAssembly);      // MVC controllers
    builder.RegisterApiControllers(thisAssembly);   // Web API controllers

    var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
    builder.RegisterModule(new ApplicationModule(mockData));

    var container = builder.Build();

    // set mvc resolver
    DependencyResolver.SetResolver(new AutofacDependencyResolver(container));

    // set webapi resolver
    var resolver = new AutofacWebApiDependencyResolver(container);
    GlobalConfiguration.Configuration.DependencyResolver = resolver;

    return container;
}
```

<span data-ttu-id="3d09c-113">当升级这些应用程序以使用 ASP.NET Core 时，这种重复的工作以及有时会造成混淆。</span><span class="sxs-lookup"><span data-stu-id="3d09c-113">When upgrading these apps to use ASP.NET Core, this duplicate effort and the confusion that sometimes accompanies it is eliminated.</span></span> <span data-ttu-id="3d09c-114">ASP.NET Core MVC 是一个统一框架，其中包含一组用于路由、筛选器和其他规则的规则。</span><span class="sxs-lookup"><span data-stu-id="3d09c-114">ASP.NET Core MVC is a unified framework with one set of rules for routing, filters, and more.</span></span> <span data-ttu-id="3d09c-115">依赖关系注入内置于 .NET Core 本身。</span><span class="sxs-lookup"><span data-stu-id="3d09c-115">Dependency injection is built into .NET Core itself.</span></span> <span data-ttu-id="3d09c-116">所有这些都可以在中进行配置 `Startup.cs` ，如示例中的应用程序中所示 `eShopPorted` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-116">All of this can can be configured in `Startup.cs`, as is shown in the `eShopPorted` app in the sample.</span></span>

## <a name="migrate-httpresponsemessage-to-aspnet-core"></a><span data-ttu-id="3d09c-117">将 HttpResponseMessage 迁移到 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d09c-117">Migrate HttpResponseMessage to ASP.NET Core</span></span>

<span data-ttu-id="3d09c-118">某些 ASP.NET Web API 应用可能具有返回的操作方法 `HttpResponseMessage` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-118">Some ASP.NET Web API apps may have action methods that return `HttpResponseMessage`.</span></span> <span data-ttu-id="3d09c-119">ASP.NET Core 中不存在此类型。</span><span class="sxs-lookup"><span data-stu-id="3d09c-119">This type does not exist in ASP.NET Core.</span></span> <span data-ttu-id="3d09c-120">下面是在 `Delete` 操作方法中使用的 `ResponseMessage` 帮助器方法的示例 `ApiController` ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-120">Below is an example of its usage in a `Delete` action method, using the `ResponseMessage` helper method on the base `ApiController`:</span></span>

```csharp
// DELETE api/<controller>/5
[HttpDelete]
public IHttpActionResult Delete(int id)
{
    var brandToDelete = _service.GetCatalogBrands().FirstOrDefault(x => x.Id == id);
    if (brandToDelete == null)
    {
        return ResponseMessage(new HttpResponseMessage(HttpStatusCode.NotFound));
    }

    // demo only - don't actually delete
    return ResponseMessage(new HttpResponseMessage(HttpStatusCode.OK));
}
```

<span data-ttu-id="3d09c-121">在 ASP.NET Core MVC 中，有可用于所有常见 HTTP 响应状态代码的帮助器方法，因此以上方法将移植到以下代码：</span><span class="sxs-lookup"><span data-stu-id="3d09c-121">In ASP.NET Core MVC, there are helper methods available for all of the common HTTP response status codes, so the above method would be ported to the following code:</span></span>

```csharp
[HttpDelete("{id}")]
public IActionResult Delete(int id)
{
    var brandToDelete = _service.GetCatalogBrands().FirstOrDefault(x => x.Id == id);
    if (brandToDelete == null)
    {
        return NotFound();
    }

    // demo only - don't actually delete
    return Ok();
}
```

<span data-ttu-id="3d09c-122">如果你发现需要返回不存在任何帮助程序的自定义状态代码，你始终可以使用 `return StatusCode(int statusCode)` 返回你喜欢的任何数值代码。</span><span class="sxs-lookup"><span data-stu-id="3d09c-122">If you do find that you need to return a custom status code for which no helper exists, you can always use `return StatusCode(int statusCode)` to return any numeric code you like.</span></span>

## <a name="migrate-content-negotiation-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="3d09c-123">将内容协商从 ASP.NET Web API 迁移到 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d09c-123">Migrate content negotiation from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="3d09c-124">ASP.NET Web API 2 以本机方式支持 [内容协商](/aspnet/web-api/overview/formats-and-model-binding/content-negotiation) 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-124">ASP.NET Web API 2 supports [content negotiation](/aspnet/web-api/overview/formats-and-model-binding/content-negotiation) natively.</span></span> <span data-ttu-id="3d09c-125">该示例应用包含一个 `BrandsController` ，它通过在 XML 或 JSON 中列出其结果来演示此支持。</span><span class="sxs-lookup"><span data-stu-id="3d09c-125">The sample app includes a `BrandsController` that demonstrates this support by listing its results in either XML or JSON.</span></span> <span data-ttu-id="3d09c-126">这取决于请求的 `Accept` 标头，并在包含或时进行 `application/xml` 更改 `application/json` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-126">This is based on the request's `Accept` header, and changes when it includes `application/xml` or `application/json`.</span></span>

<span data-ttu-id="3d09c-127">ASP.NET MVC 5 应用未内置内容协商支持。</span><span class="sxs-lookup"><span data-stu-id="3d09c-127">ASP.NET MVC 5 apps do not have content negotiation support built in.</span></span>

<span data-ttu-id="3d09c-128">内容协商比返回特定的编码类型更好，因为它更灵活，并使 API 可用于更多的客户端。</span><span class="sxs-lookup"><span data-stu-id="3d09c-128">Content negotiation is preferable to returning a specific encoding type, as it is more flexible and makes the API available to a larger number of clients.</span></span> <span data-ttu-id="3d09c-129">如果当前具有返回特定格式的操作方法，应考虑在将代码移植到 ASP.NET Core 时修改它们以返回支持内容协商的结果类型。</span><span class="sxs-lookup"><span data-stu-id="3d09c-129">If you currently have action methods that return a specific format, you should consider modifying them to return a result type that supports content negotiation when you port the code to ASP.NET Core.</span></span>

<span data-ttu-id="3d09c-130">以下代码将返回 JSON 格式的数据，而不考虑客户端 `Accept` 标头内容：</span><span class="sxs-lookup"><span data-stu-id="3d09c-130">The following code returns data in JSON format regardless of client `Accept` header content:</span></span>

```csharp
[HttpGet]
public ActionResult Index()
{
    return Json(new { Message = "Hello World!" });
}
```

<span data-ttu-id="3d09c-131">如果使用了适当的[返回类型](/aspnet/core/web-api/action-return-types)， [ASP.NET Core MVC 将以本机方式支持内容协商](/aspnet/core/web-api/advanced/formatting)。</span><span class="sxs-lookup"><span data-stu-id="3d09c-131">[ASP.NET Core MVC supports content negotiation natively](/aspnet/core/web-api/advanced/formatting), provided an appropriate [return type](/aspnet/core/web-api/action-return-types) is used.</span></span> <span data-ttu-id="3d09c-132">内容协商由 [ObjectResult] 实现，由控制器 helper 方法返回的状态代码特定操作结果返回。</span><span class="sxs-lookup"><span data-stu-id="3d09c-132">Content negotiation is implemented by [ObjectResult] which is returned by the status code-specific action results returned by the controller helper methods.</span></span> <span data-ttu-id="3d09c-133">在 ASP.NET Core MVC 和使用内容协商中实现的上一操作方法为：</span><span class="sxs-lookup"><span data-stu-id="3d09c-133">The previous action method, implemented in ASP.NET Core MVC and using content negotiation, would be:</span></span>

```csharp
public IActionResult Index()
{
    return Ok(new { Message = "Hello World!"} );
}
```

<span data-ttu-id="3d09c-134">这将默认返回 JSON 格式的数据。</span><span class="sxs-lookup"><span data-stu-id="3d09c-134">This will default to returning the data in JSON format.</span></span> <span data-ttu-id="3d09c-135">[如果已使用适当的格式化程序配置了应用程序](/aspnet/core/web-api/advanced/formatting)，则将使用 XML 和其他格式。</span><span class="sxs-lookup"><span data-stu-id="3d09c-135">XML and other formats will be used [if the app has been configured with the appropriate formatter](/aspnet/core/web-api/advanced/formatting).</span></span>

## <a name="custom-model-binding"></a><span data-ttu-id="3d09c-136">自定义模型绑定</span><span class="sxs-lookup"><span data-stu-id="3d09c-136">Custom model binding</span></span>

<span data-ttu-id="3d09c-137">大多数 ASP.NET MVC 和 Web API 应用使用模型绑定。</span><span class="sxs-lookup"><span data-stu-id="3d09c-137">Most ASP.NET MVC and Web API apps make use of model binding.</span></span> <span data-ttu-id="3d09c-138">默认模型绑定语法在这些应用和 ASP.NET Core MVC 之间完全迁移。</span><span class="sxs-lookup"><span data-stu-id="3d09c-138">The default model binding syntax migrates fairly seamlessly between these apps and ASP.NET Core MVC.</span></span> <span data-ttu-id="3d09c-139">但是，在某些情况下，客户已编写 [自定义模型联编](/aspnet/web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api#model-binders) 以支持特定的模型类型或使用方案。</span><span class="sxs-lookup"><span data-stu-id="3d09c-139">However, in some cases customers have written [custom model binders](/aspnet/web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api#model-binders) to support specific model types or usage scenarios.</span></span> <span data-ttu-id="3d09c-140">ASP.NET MVC 和 Web API 项目中的自定义模型联编器 `IModelBinder` 分别使用 `System.Web.Mvc` 和命名空间中定义的单独接口 `System.Web.Http` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-140">Custom model binders in ASP.NET MVC and Web API projects use separate `IModelBinder` interfaces defined in `System.Web.Mvc` and `System.Web.Http` namespaces, respectively.</span></span> <span data-ttu-id="3d09c-141">在这两种情况下，自定义联编程序都公开了一 `Bind` 种方法，该方法接受控制器或操作上下文以及作为参数的模型绑定上下文。</span><span class="sxs-lookup"><span data-stu-id="3d09c-141">In both cases, the custom binder exposes a `Bind` method that accepts a controller or action context and a model binding context as arguments.</span></span>

<span data-ttu-id="3d09c-142">创建自定义联编程序后，必须将其注册到应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-142">Once the custom binder is created, it must be registered with the app.</span></span> <span data-ttu-id="3d09c-143">此步骤需要创建另一个类型， `ModelBinderProvider` 它充当工厂并在请求期间创建模型联编程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-143">This step requires creating another type, a `ModelBinderProvider`, which acts as a factory and creates the model binder during a request.</span></span> <span data-ttu-id="3d09c-144">可以 `ApplicationStart` 在 MVC 应用中添加联编程序，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3d09c-144">Binders can be added during `ApplicationStart` in MVC apps as shown:</span></span>

```csharp
ModelBinderProviders.BinderProviders.Insert(0, new MyCustomBinderProvider()); // MVC
```

<span data-ttu-id="3d09c-145">在 Web API 应用程序中，可以使用属性引用自定义联编程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-145">In Web API apps, custom binders can be referenced using attributes.</span></span> <span data-ttu-id="3d09c-146">`ModelBinder`可以将特性添加到操作方法参数或参数的类型定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3d09c-146">The `ModelBinder` attribute can be added to action method parameters or to the parameter's type definition, as shown:</span></span>

```csharp
// attribute on action method parameter
public HttpResponseMessage([ModelBinder(typeof(MyCustomBinder))] CustomDTO custom)
{
}

// attribute on type
[ModelBinder(typeof(MyCustomBinder))]
public class CustomDTO
{
}
```

<span data-ttu-id="3d09c-147">若要在 ASP.NET Web API 中全局注册模型联编程序，必须在应用程序启动期间添加其提供程序：</span><span class="sxs-lookup"><span data-stu-id="3d09c-147">To register a model binder globally in ASP.NET Web API, its provider must be added during app startup:</span></span>

```csharp
public static class WebApiConfig
{
    public static void Register(HttpConfiguration config)
    {
        var provider = new CustomModelBinderProvider(
            typeof(CustomDTO), new CustomModelBinder());
        config.Services.Insert(typeof(ModelBinderProvider), 0, provider);

        // ...
    }
}
```

<span data-ttu-id="3d09c-148">将 [自定义模型提供程序迁移到 ASP.NET Core](/aspnet/core/mvc/advanced/custom-model-binding#custom-model-binder-sample)时，Web API 模式比 ASP.NET MVC 5 ASP.NET Core 方法更接近。</span><span class="sxs-lookup"><span data-stu-id="3d09c-148">When migrating [custom model providers to ASP.NET Core](/aspnet/core/mvc/advanced/custom-model-binding#custom-model-binder-sample), the Web API pattern is closer to the ASP.NET Core approach than the ASP.NET MVC 5.</span></span> <span data-ttu-id="3d09c-149">ASP.NET Core 的 `IModelBinder` 接口和 WEB API 之间的主要区别在于 ASP.NET Core 方法 (异步 `BindModelAsync`) ，它只需要一个 `BindingModelContext` 参数，而不需要使用 Web API 的版本所需的两个参数。</span><span class="sxs-lookup"><span data-stu-id="3d09c-149">The main differences between ASP.NET Core's `IModelBinder` interface and Web API's is that the ASP.NET Core method is async (`BindModelAsync`) and it only requires a single `BindingModelContext` parameter instead of two parameters like Web API's version required.</span></span> <span data-ttu-id="3d09c-150">在 ASP.NET Core 中，可以 `[ModelBinder]` 对各个操作方法参数或其关联的类型使用特性。</span><span class="sxs-lookup"><span data-stu-id="3d09c-150">In ASP.NET Core, you can use a `[ModelBinder]` attribute on individual action method parameters or their associated types.</span></span> <span data-ttu-id="3d09c-151">你还可以创建一个 `ModelBinderProvider` ，它将在适当的情况下在应用中全局使用。</span><span class="sxs-lookup"><span data-stu-id="3d09c-151">You can also create a `ModelBinderProvider` that will be used globally within the app where appropriate.</span></span> <span data-ttu-id="3d09c-152">若要配置此类提供程序，请将代码添加到 `Startup` 中的 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-152">To configure such a provider, you would add code to `Startup` in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers(options =>
    {
        options.ModelBinderProviders.Insert(0, new CustomModelBinderProvider());
    });
}
```

## <a name="media-formatters"></a><span data-ttu-id="3d09c-153">媒体格式化程序</span><span class="sxs-lookup"><span data-stu-id="3d09c-153">Media formatters</span></span>

<span data-ttu-id="3d09c-154">ASP.NET Web API 支持多种媒体格式，可以使用自定义媒体格式化程序进行扩展。</span><span class="sxs-lookup"><span data-stu-id="3d09c-154">ASP.NET Web API supports multiple media formats and can be extended by using custom media formatters.</span></span> <span data-ttu-id="3d09c-155">文档介绍可用于以逗号分隔值格式发送数据的 [示例 CSV 媒体格式化程序](/aspnet/web-api/overview/formats-and-model-binding/media-formatters#example-creating-a-csv-media-formatter) 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-155">The docs describe an [example CSV Media Formatter](/aspnet/web-api/overview/formats-and-model-binding/media-formatters#example-creating-a-csv-media-formatter) that can be used to send data in a comma-separated value format.</span></span> <span data-ttu-id="3d09c-156">如果 Web API 应用使用自定义媒体格式化程序，则需要将其转换为 [ASP.NET Core 自定义格式化](/aspnet/core/web-api/advanced/custom-formatters)程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-156">If your Web API app uses custom media formatters, you'll need to convert them to [ASP.NET Core custom formatters](/aspnet/core/web-api/advanced/custom-formatters).</span></span>

<span data-ttu-id="3d09c-157">若要在 Web API 2 中创建自定义格式化程序，请从相应的基类继承，然后使用对象将格式化程序添加到 Web API 管道 `HttpConfiguration` ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-157">To create a custom formatter in Web API 2, you inherited from an appropriate base class and then added the formatter to the Web API pipeline using the `HttpConfiguration` object:</span></span>

```csharp
public static void ConfigureApis(HttpConfiguration config)
{
    config.Formatters.Add(new ProductCsvFormatter());
}
```

<span data-ttu-id="3d09c-158">在 ASP.NET Core 中，此过程类似。</span><span class="sxs-lookup"><span data-stu-id="3d09c-158">In ASP.NET Core, the process is similar.</span></span> <span data-ttu-id="3d09c-159">ASP.NET Core 支持) 的和输出格式化程序 (使用的输入格式化程序， (用于对) 响应进行格式设置。</span><span class="sxs-lookup"><span data-stu-id="3d09c-159">ASP.NET Core supports both input formatters (used by model binding) and output formatters (used to format responses).</span></span> <span data-ttu-id="3d09c-160">以特定方式向输出响应添加自定义格式化程序涉及从适当的基类继承，并将格式化程序添加到中的 MVC `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-160">Adding a custom formatter to output responses in a specific way involves inheriting from an appropriate base class and adding the formatter to MVC in `Startup`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers(options =>
    {
        options.InputFormatters.Insert(0, new CustomInputFormatter());
        options.OutputFormatters.Insert(0, new CustomOutputFormatter());
    });
}
```

<span data-ttu-id="3d09c-161">你将在 [AspNetCore](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.formatters) 命名空间中找到基类的完整列表。</span><span class="sxs-lookup"><span data-stu-id="3d09c-161">You'll find a complete list of base classes in the [Microsoft.AspNetCore.Mvc.Formatters](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.formatters) namespace.</span></span>

<span data-ttu-id="3d09c-162">从 Web API 格式化程序迁移到 ASP.NET Core MVC 格式化程序的步骤如下：</span><span class="sxs-lookup"><span data-stu-id="3d09c-162">The steps to migrate from a Web API formatter to an ASP.NET Core MVC formatter are:</span></span>

1. <span data-ttu-id="3d09c-163">为新的格式化程序标识相应的基类。</span><span class="sxs-lookup"><span data-stu-id="3d09c-163">Identify an appropriate base class for the new formatter.</span></span>
1. <span data-ttu-id="3d09c-164">创建基类的新实例，并实现其所需的方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-164">Create a new instance of the base class and implement its required methods.</span></span>
1. <span data-ttu-id="3d09c-165">将 Web API 格式化程序中的功能复制到新的实现。</span><span class="sxs-lookup"><span data-stu-id="3d09c-165">Copy over the functionality from the Web API formatter to the new implementation.</span></span>
1. <span data-ttu-id="3d09c-166">将 ASP.NET Core 应用的方法中的 MVC 配置 `ConfigureServices` 为使用新的格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-166">Configure MVC in the ASP.NET Core App's `ConfigureServices` method to use the new formatter.</span></span>

## <a name="custom-filters"></a><span data-ttu-id="3d09c-167">自定义筛选器</span><span class="sxs-lookup"><span data-stu-id="3d09c-167">Custom filters</span></span>

<span data-ttu-id="3d09c-168">筛选器在 ASP.NET Core 应用中用于在请求处理管道中的特定阶段之前和/或之后执行代码。</span><span class="sxs-lookup"><span data-stu-id="3d09c-168">Filters are used in ASP.NET Core apps to execute code before and/or after certain stages in the request processing pipeline.</span></span> <span data-ttu-id="3d09c-169">ASP.NET MVC 和 Web API 还使用筛选器的方式几乎相同，但详细信息会有所不同。</span><span class="sxs-lookup"><span data-stu-id="3d09c-169">ASP.NET MVC and Web API also use filters in much the same way, but the details vary.</span></span> <span data-ttu-id="3d09c-170">例如， [ASP.NET MVC 支持四种类型的筛选器](/aspnet/mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs#the-different-types-of-filters)。</span><span class="sxs-lookup"><span data-stu-id="3d09c-170">For instance, [ASP.NET MVC supports four kinds of filters](/aspnet/mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs#the-different-types-of-filters).</span></span> <span data-ttu-id="3d09c-171">ASP.NET Web API 2 支持类似的筛选器，并且 MVC 和 Web API 都包含用于 [重写筛选器](/dotnet/api/system.web.mvc.filters.ioverridefilter)的属性。</span><span class="sxs-lookup"><span data-stu-id="3d09c-171">ASP.NET Web API 2 supports similar filters, and both MVC and Web API included attributes to [override filters](/dotnet/api/system.web.mvc.filters.ioverridefilter).</span></span>

<span data-ttu-id="3d09c-172">ASP.NET MVC 和 Web API 应用中使用的最常见筛选器是由 [IActionFilter 接口](/dotnet/api/system.web.mvc.iactionfilter)定义的操作筛选器。</span><span class="sxs-lookup"><span data-stu-id="3d09c-172">The most common filter used in ASP.NET MVC and Web API apps is the action filter, which is defined by an [IActionFilter interface](/dotnet/api/system.web.mvc.iactionfilter).</span></span> <span data-ttu-id="3d09c-173">此接口在)  (之前提供方法，在 `OnActionExecuting` `OnActionExecuted` 执行操作之前和/或之后 () 可用于执行代码，如每个方法所述。</span><span class="sxs-lookup"><span data-stu-id="3d09c-173">This interface provides methods for before (`OnActionExecuting`) and after (`OnActionExecuted`) which can be used to execute code before and/or after an action executes, as noted for each method.</span></span>

<span data-ttu-id="3d09c-174">ASP.NET Core 继续支持筛选器，并且其 MVC 和 Web API 的统一是指其实现仅有一种方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-174">ASP.NET Core continues to support filters, and its unification of MVC and Web API means there is only one approach to their implementation.</span></span> <span data-ttu-id="3d09c-175">[文档包括 ASP.NET CORE MVC 中内置的5个 () 类筛选器的详细信息](/aspnet/core/mvc/controllers/filters#filter-types)。</span><span class="sxs-lookup"><span data-stu-id="3d09c-175">The [docs include detailed coverage of the five (5) kinds of filters built into ASP.NET Core MVC](/aspnet/core/mvc/controllers/filters#filter-types).</span></span> <span data-ttu-id="3d09c-176">ASP.NET MVC 和 ASP.NET Web API 中支持的所有筛选器变体在 ASP.NET Core 中都具有关联的版本，因此，通常只需标识适当的接口和/或基类，并将代码迁移到。</span><span class="sxs-lookup"><span data-stu-id="3d09c-176">All of the filter variants supported in ASP.NET MVC and ASP.NET Web API have associated versions in ASP.NET Core, so migration is generally just a matter of identifying the appropriate interface and/or base class and migrating the code over.</span></span>

<span data-ttu-id="3d09c-177">除同步接口外，ASP.NET Core 还提供了异步接口，如 `IAsyncActionFilter` 提供可用于合并代码以在操作之前和之后运行的异步方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3d09c-177">In addition to the synchronous interfaces, ASP.NET Core also provides async interfaces like `IAsyncActionFilter` which provide a single async method that can be used to incorporate code to run both before and after the action, as shown:</span></span>

```csharp
public class SampleAsyncActionFilter : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(
        ActionExecutingContext context,
        ActionExecutionDelegate next)
    {
        // Do something before the action executes.

        // next() calls the action method.
        var resultContext = await next();
        // resultContext.Result is set.
        // Do something after the action executes.
    }
}
```

<span data-ttu-id="3d09c-178">迁移异步代码 (或应异步) 的代码时，团队应考虑利用为此目的提供的内置异步类型。</span><span class="sxs-lookup"><span data-stu-id="3d09c-178">When migrating async code (or code that should be async), teams should consider leveraging the built in async types that are provided for this purpose.</span></span>

<span data-ttu-id="3d09c-179">大多数 ASP.NET MVC 和 Web API 应用不使用大量的自定义筛选器。</span><span class="sxs-lookup"><span data-stu-id="3d09c-179">Most ASP.NET MVC and Web API apps do not use a large number of custom filters.</span></span> <span data-ttu-id="3d09c-180">由于 ASP.NET Core MVC 中筛选器的方法与 ASP.NET MVC 和 Web API 中的筛选器密切一致，因此，迁移自定义筛选器通常非常简单。</span><span class="sxs-lookup"><span data-stu-id="3d09c-180">Since the approach to filters in ASP.NET Core MVC is closely aligned with filters in ASP.NET MVC and Web API, the migration of custom filters is generally fairly straightforward.</span></span> <span data-ttu-id="3d09c-181">请务必阅读 ASP.NET Core 文档中有关筛选器的详细文档，一旦确信已充分了解这些文档，请将旧系统中的逻辑移植到新系统的筛选器。</span><span class="sxs-lookup"><span data-stu-id="3d09c-181">Be sure to read the detailed documentation on filters in ASP.NET Core's docs, and once you're sure you have a good understanding of them, port the logic from the old system to the new system's filters.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="3d09c-182">路由约束</span><span class="sxs-lookup"><span data-stu-id="3d09c-182">Route constraints</span></span>

<span data-ttu-id="3d09c-183">ASP.NET Core 使用路由约束来帮助确保正确路由请求以便路由请求。</span><span class="sxs-lookup"><span data-stu-id="3d09c-183">ASP.NET Core uses route constraints to help ensure requests are routed properly to route a request.</span></span> <span data-ttu-id="3d09c-184">[ASP.NET Core 支持大量不同的路由约束用于此目的]/aspnet/core/fundamentals/routing # 路由约束-引用) 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-184">[ASP.NET Core supports a large number of different route constraints for this purpose]/aspnet/core/fundamentals/routing#route-constraint-reference).</span></span> <span data-ttu-id="3d09c-185">路由约束可以应用于路由表中，但使用 ASP.NET MVC 5 和/或 [ASP.NET Web API 2](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints) 生成的大多数应用都使用应用于属性路由的内联路由约束。</span><span class="sxs-lookup"><span data-stu-id="3d09c-185">Route constraints can be applied in the route table, but most apps built with ASP.NET MVC 5 and/or [ASP.NET Web API 2](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints) use inline route constraints applied to attribute routes.</span></span> <span data-ttu-id="3d09c-186">内联路由约束使用如下格式：</span><span class="sxs-lookup"><span data-stu-id="3d09c-186">Inline route constraints use a format like this one:</span></span>

```csharp
[Route("/customer/{id:int}")]
```

<span data-ttu-id="3d09c-187">`:int` `id` 路由参数后的值会限制值，以匹配 `int` 类型。</span><span class="sxs-lookup"><span data-stu-id="3d09c-187">The `:int` after the `id` route parameter constrains the value to match the the `int` type.</span></span> <span data-ttu-id="3d09c-188">使用路由约束的一个优点是，它们允许两个完全相同的路由存在，其中的参数仅有其类型不同。</span><span class="sxs-lookup"><span data-stu-id="3d09c-188">One benefit of using route constraints is that they allow for two otherwise-identical routes to exist where the parameters differ only by their type.</span></span> <span data-ttu-id="3d09c-189">这使得仅基于参数类型的路由的 [方法重载](../../standard/design-guidelines/member-overloading.md) 是等效的。</span><span class="sxs-lookup"><span data-stu-id="3d09c-189">This allows for the equivalent of [method overloading](../../standard/design-guidelines/member-overloading.md) of routes based solely on parameter type.</span></span>

<span data-ttu-id="3d09c-190">路由约束、其语法和用法的集合在所有这三种方法之间非常类似。</span><span class="sxs-lookup"><span data-stu-id="3d09c-190">The set of route constraints, their syntax, and usage is very similar between all three approaches.</span></span> <span data-ttu-id="3d09c-191">自定义路由约束在客户应用程序中非常罕见。</span><span class="sxs-lookup"><span data-stu-id="3d09c-191">Custom route constraints are fairly rare in customer applications.</span></span> <span data-ttu-id="3d09c-192">如果你的应用程序使用自定义路由约束，并且需要 ASP.NET Core 端口，则这些文档将包含演示 [如何在 ASP.NET Core 中创建自定义路由约束](/aspnet/core/fundamentals/routing#custom-route-constraints)的示例。</span><span class="sxs-lookup"><span data-stu-id="3d09c-192">If your app uses a custom route constraint and needs to port to ASP.NET Core, the docs include examples showing [how to create custom route constraints in ASP.NET Core](/aspnet/core/fundamentals/routing#custom-route-constraints).</span></span> <span data-ttu-id="3d09c-193">实质上，只需实现 `IRouteConstraint` 和 `Match` 方法，然后在配置应用的路由时添加自定义约束：</span><span class="sxs-lookup"><span data-stu-id="3d09c-193">Essentially all that's required is to implement `IRouteConstraint` and its `Match` method, and then add the custom constraint when configuring routing for the app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();

    services.AddRouting(options =>
    {
        options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
    });
}
```

<span data-ttu-id="3d09c-194">这与在 ASP.NET Web API 中使用自定义约束的方式非常类似，后者使用 `IHttpRouteConstraint` 解析程序并对其进行配置 `HttpConfiguration.MapHttpAttributeRoutes` ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-194">This is very similar to how custom constraints are used in ASP.NET Web API, which uses `IHttpRouteConstraint` and configures it using a resolver and a call to `HttpConfiguration.MapHttpAttributeRoutes`:</span></span>

```csharp
public static class WebApiConfig
{
    public static void Register(HttpConfiguration config)
    {
        var constraintResolver = new DefaultInlineConstraintResolver();
        constraintResolver.ConstraintMap.Add("nonzero", typeof(CustomConstraint));

        config.MapHttpAttributeRoutes(constraintResolver);
    }
}
```

<span data-ttu-id="3d09c-195">ASP.NET MVC 5 遵循非常类似的方法， `IRouteConstraint` 它使用作为其接口名称并将约束配置为路由配置的一部分：</span><span class="sxs-lookup"><span data-stu-id="3d09c-195">ASP.NET MVC 5 follows a very similar approach, using `IRouteConstraint` for its interface name and configuring the constraint as part of route configuration:</span></span>

```csharp
public class RouteConfig
{
    public static void RegisterRoutes(RouteCollection routes)
    {
        routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

        var constraintsResolver = new DefaultInlineConstraintResolver();
        constraintsResolver.ConstraintMap.Add("values", typeof(ValuesConstraint));
        routes.MapMvcAttributeRoutes(constraintsResolver);
    }
}
```

<span data-ttu-id="3d09c-196">将路由约束使用情况以及自定义路由约束迁移到 ASP.NET Core 通常非常简单。</span><span class="sxs-lookup"><span data-stu-id="3d09c-196">Migrating route constraint usage as well as custom route constraints to ASP.NET Core is typically very straightforward.</span></span>

## <a name="custom-route-handlers"></a><span data-ttu-id="3d09c-197">自定义路由处理程序</span><span class="sxs-lookup"><span data-stu-id="3d09c-197">Custom route handlers</span></span>

<span data-ttu-id="3d09c-198">ASP.NET MVC 5 的另一个相当高级的功能是路由处理程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-198">Another fairly advanced feature of ASP.NET MVC 5 is route handlers.</span></span> <span data-ttu-id="3d09c-199">自定义路由处理程序实现 `IRouteHandler` ，其中包括一个方法，该方法返回 `IHttpHandler` 给定请求的。</span><span class="sxs-lookup"><span data-stu-id="3d09c-199">Custom route handlers implement `IRouteHandler`, which includes a single method that returns an `IHttpHandler` for a give request.</span></span> <span data-ttu-id="3d09c-200">反过来，会 `IHttpHandler` 公开一个 `IsReusable` 属性和一个 `ProcessRequest` 方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-200">The `IHttpHandler`, in turn, exposes an `IsReusable` property and a single `ProcessRequest` method.</span></span> <span data-ttu-id="3d09c-201">在 ASP.NET MVC 5 中，您可以将路由表中的特定路由配置为使用您的自定义处理程序：</span><span class="sxs-lookup"><span data-stu-id="3d09c-201">In ASP.NET MVC 5, you can configure a particular route in the route table to use your custom handler:</span></span>

```csharp
public static void RegisterRoutes(RouteCollection routes)
{
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

    routes.Add(new Route("custom", new CustomRouteHandler()));
}
```

<span data-ttu-id="3d09c-202">若要将自定义路由处理程序从 ASP.NET MVC 5 迁移到 ASP.NET Core，可以使用筛选器 (例如操作筛选器) 或自定义 [`IRouter`](/dotnet/api/microsoft.aspnetcore.routing.irouter) 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-202">To migrate custom route handlers from ASP.NET MVC 5 to ASP.NET Core, you can either use a filter (such as an action filter) or a custom [`IRouter`](/dotnet/api/microsoft.aspnetcore.routing.irouter).</span></span> <span data-ttu-id="3d09c-203">筛选器方法相对简单，并且在 MVC 添加到中时，可以作为全局筛选器添加 `ConfigureServices` 。 </span><span class="sxs-lookup"><span data-stu-id="3d09c-203">The filter approach is relatively straightforward, and can be added as a global filter when MVC is added to `ConfigureServices` in *Startup.cs*.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Filters.Add(typeof(CustomActionFilter));
    });
}
```

<span data-ttu-id="3d09c-204">`IRouter`选项需要实现接口的 `RouteAsync` 和 `GetVirtualPath` 方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-204">The `IRouter` option requires implementing the interface's `RouteAsync` and `GetVirtualPath` methods.</span></span> <span data-ttu-id="3d09c-205">自定义路由器将添加到 `Configure` *启动 .cs* 的方法中的请求管道。</span><span class="sxs-lookup"><span data-stu-id="3d09c-205">The custom router is added to the request pipeline in the `Configure` method in *Startup.cs*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ...
    app.UseMvc(routes =>
    {
        routes.Routes.Add(new CustomRouter(routes.DefaultHandler));
    });
}
```

<span data-ttu-id="3d09c-206">在 ASP.NET Web API 中，这些处理程序称为 [自定义消息处理程序](/aspnet/web-api/overview/advanced/http-message-handlers#custom-message-handlers)，而不是 *路由处理程序*。</span><span class="sxs-lookup"><span data-stu-id="3d09c-206">In ASP.NET Web API, these handlers are referred to as [custom message handlers](/aspnet/web-api/overview/advanced/http-message-handlers#custom-message-handlers), rather than *route handlers*.</span></span> <span data-ttu-id="3d09c-207">消息处理程序必须派生自 `DelegatingHandler` 并重写其 `SendAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-207">Message handlers must derive from `DelegatingHandler` and override its `SendAsync` method.</span></span> <span data-ttu-id="3d09c-208">消息处理程序可以链接在一起形成管道，其方式与 ASP.NET Core 中间件及其请求管道非常类似。</span><span class="sxs-lookup"><span data-stu-id="3d09c-208">Message handlers can be chained together to form a pipeline in a fashion that is very similar to ASP.NET Core middleware and its request pipeline.</span></span>

<span data-ttu-id="3d09c-209">ASP.NET Core 没有 `DelegatingHandler` 类型或单独的消息处理程序管道。</span><span class="sxs-lookup"><span data-stu-id="3d09c-209">ASP.NET Core has no `DelegatingHandler` type or separate message handler pipeline.</span></span> <span data-ttu-id="3d09c-210">相反，此类处理程序应使用全局筛选器、自定义 `IRouter` 实例 (参见以上) 或自定义中间件。</span><span class="sxs-lookup"><span data-stu-id="3d09c-210">Instead, such handlers should be migrated using global filters, custom `IRouter` instances (see above), or custom middleware.</span></span> <span data-ttu-id="3d09c-211">ASP.NET Core MVC 筛选器和 `IRouter` 类型具有对 mvc 构造（如控制器和操作）的内置访问的优势，而中间件则是与 mvc 无任何依赖项的较低级别方法。</span><span class="sxs-lookup"><span data-stu-id="3d09c-211">ASP.NET Core MVC filters and `IRouter` types have the advantage of having built-in access to MVC constructs like controllers and actions, while middleware is a lower level approach that has no ties to MVC.</span></span> <span data-ttu-id="3d09c-212">这使得它更灵活，但如果需要访问 MVC 组件，还需要更多的精力。</span><span class="sxs-lookup"><span data-stu-id="3d09c-212">This makes it more flexible but also requires more effort if you need to access MVC components.</span></span>

## <a name="cors-support"></a><span data-ttu-id="3d09c-213">CORS 支持</span><span class="sxs-lookup"><span data-stu-id="3d09c-213">CORS support</span></span>

<span data-ttu-id="3d09c-214">CORS 或跨域资源共享是一种 W3C 标准，使服务器可以接受来自他们所提供的响应的请求。</span><span class="sxs-lookup"><span data-stu-id="3d09c-214">CORS, or Cross-Origin Resource Sharing, is a W3C standard that allows servers to accept requests that don't originate from responses they've served.</span></span> <span data-ttu-id="3d09c-215">ASP.NET MVC 5 和 ASP.NET Web API 2 以不同方式支持 CORS。</span><span class="sxs-lookup"><span data-stu-id="3d09c-215">ASP.NET MVC 5 and ASP.NET Web API 2 support CORS in different ways.</span></span> <span data-ttu-id="3d09c-216">在 ASP.NET MVC 5 中启用 CORS 支持的最简单方法是使用如下所示的操作筛选器：</span><span class="sxs-lookup"><span data-stu-id="3d09c-216">The simplest way to enable CORS support in ASP.NET MVC 5 is with an action filter like this one:</span></span>

```csharp
public class AllowCrossSiteAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext filterContext)
    {
        filterContext.RequestContext.HttpContext.Response.AddHeader(
            "Access-Control-Allow-Origin", "example.com");
        base.OnActionExecuting(filterContext);
    }
}
```

<span data-ttu-id="3d09c-217">ASP.NET Web API 还可以使用此类筛选器，但它也具有 [启用 CORS 的内置支持](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors) ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-217">ASP.NET Web API can also use such a filter, but it has [built-in support for enabling CORS](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors) as well:</span></span>

```csharp
public static class WebApiConfig
{
    public static void Register(HttpConfiguration config)
    {
        config.EnableCors();
        // ...
    }
}
```

<span data-ttu-id="3d09c-218">添加后，可以使用属性配置允许的源、标头和方法 `EnableCors` ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3d09c-218">Once this is added, you can configure allowed origins, headers, and methods using the `EnableCors` attribute, like so:</span></span>

```csharp
[EnableCors(origins: "https://dot.net", headers: "*", methods: "*")]
public class TestController : ApiController
{
    // Controller methods not shown...
}
```

<span data-ttu-id="3d09c-219">在从 ASP.NET MVC 5 或 ASP.NET Web API 2 迁移 CORS 实现之前，请务必查看 [cors 的工作原理](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works) ，并创建一些自动测试，这些测试演示了在当前系统中按预期方式工作的 cors。</span><span class="sxs-lookup"><span data-stu-id="3d09c-219">Before migrating your CORS implementation from ASP.NET MVC 5 or ASP.NET Web API 2, be sure to review [how CORS works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works) and create some automated tests that demonstrate CORS is working as expected in your current system.</span></span>

<span data-ttu-id="3d09c-220">在 ASP.NET Core 中，有三种内置方式来启用 CORS：</span><span class="sxs-lookup"><span data-stu-id="3d09c-220">In ASP.NET Core, there are three built-in ways to enable CORS:</span></span>

- <span data-ttu-id="3d09c-221">[通过策略](/aspnet/core/security/cors?#cors-with-named-policy-and-middleware) 在中配置 `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="3d09c-221">[Configured via policy](/aspnet/core/security/cors?#cors-with-named-policy-and-middleware) in `ConfigureServices`</span></span>
- <span data-ttu-id="3d09c-222">通过[终结点路由](/aspnet/core/security/cors?#enable-cors-with-endpoint-routing)启用</span><span class="sxs-lookup"><span data-stu-id="3d09c-222">Enabled with [endpoint routing](/aspnet/core/security/cors?#enable-cors-with-endpoint-routing)</span></span>
- <span data-ttu-id="3d09c-223">已启用[ `EnableCors` 属性](/aspnet/core/security/cors?view=aspnetcore-5.0#enable-cors-with-attributes)</span><span class="sxs-lookup"><span data-stu-id="3d09c-223">Enabled with the [`EnableCors` attribute](/aspnet/core/security/cors?view=aspnetcore-5.0#enable-cors-with-attributes)</span></span>

<span data-ttu-id="3d09c-224">其中的每个方法都在文档中进行了详细介绍，这些文档与上述选项关联。</span><span class="sxs-lookup"><span data-stu-id="3d09c-224">Each of these approaches is covered in detail in the docs, which are linked from the above options.</span></span> <span data-ttu-id="3d09c-225">你选择哪一种取决于现有应用如何支持 CORS。</span><span class="sxs-lookup"><span data-stu-id="3d09c-225">Which one you choose will largely depend on how your existing app supports CORS.</span></span> <span data-ttu-id="3d09c-226">如果应用使用属性，则可能会迁移到 `EnableCors` 最轻松地使用属性。</span><span class="sxs-lookup"><span data-stu-id="3d09c-226">If the app uses attributes, you can probably migrate to use the `EnableCors` attribute most easily.</span></span> <span data-ttu-id="3d09c-227">如果你的应用使用筛选器，你可以继续使用该方法 (但这并不是 ASP.NET Core) 中使用的典型方法，或者迁移以使用属性或策略。</span><span class="sxs-lookup"><span data-stu-id="3d09c-227">If your app uses filters, you could continue using that approach (though it's not the typical approach used in ASP.NET Core), or migrate to use attributes or policies.</span></span> <span data-ttu-id="3d09c-228">终结点路由是 ASP.NET Core 3 中引入的一项相对较新的功能，因此它在 ASP.NET MVC 5 或 ASP.NET Web API 2 应用程序中没有关闭模拟功能。</span><span class="sxs-lookup"><span data-stu-id="3d09c-228">Endpoint routing is a relatively new feature introduced with ASP.NET Core 3 and as such it doesn't have a close analog in ASP.NET MVC 5 or ASP.NET Web API 2 apps.</span></span>

## <a name="custom-areas"></a><span data-ttu-id="3d09c-229">自定义区域</span><span class="sxs-lookup"><span data-stu-id="3d09c-229">Custom areas</span></span>

<span data-ttu-id="3d09c-230">许多 ASP.NET MVC 应用使用区域来组织项目。</span><span class="sxs-lookup"><span data-stu-id="3d09c-230">Many ASP.NET MVC apps use Areas to organize the project.</span></span> <span data-ttu-id="3d09c-231">区域通常位于项目根目录中的 " *区域* " 文件夹中，并且必须在应用程序启动时注册，通常在中 `Application_Start()` ：</span><span class="sxs-lookup"><span data-stu-id="3d09c-231">Areas typically reside in the root of the project in an *Areas* folder, and must be registered when the application starts, typically in `Application_Start()`:</span></span>

```csharp
AreaRegistration.RegisterAllAreas();
```

<span data-ttu-id="3d09c-232">注册启动中的所有区域的替代方法是在 `RouteArea` 各个控制器上使用属性：</span><span class="sxs-lookup"><span data-stu-id="3d09c-232">An alternative to registering all areas in startup is to use the `RouteArea` attribute on individual controllers:</span></span>

```csharp
[RouteArea("Admin")]
public class SomeController : Controller
```

<span data-ttu-id="3d09c-233">使用区域时，会将其他参数传递给 HTML 帮助器方法，以生成到不同区域中的操作的链接：</span><span class="sxs-lookup"><span data-stu-id="3d09c-233">When using Areas, additional arguments are passed into HTML helper methods to generate links to actions in different areas:</span></span>

```cshtml
@Html.ActionLink("News", "Index", "News", new { area = "News" }, null)
```

<span data-ttu-id="3d09c-234">ASP.NET Web API 应用通常不会显式使用区域，因为它们的控制器可以放置在项目中的任何文件夹中。</span><span class="sxs-lookup"><span data-stu-id="3d09c-234">ASP.NET Web API apps don't typically use areas explicitly, since their controllers can be placed in any folder in the project.</span></span> <span data-ttu-id="3d09c-235">团队可以使用他们喜欢的任何文件夹结构来组织其 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="3d09c-235">Teams can use any folder structure they like to organize their API controllers.</span></span>

<span data-ttu-id="3d09c-236">ASP.NET Core MVC 中支持[区域](/aspnet/core/mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="3d09c-236">[Areas](/aspnet/core/mvc/controllers/areas) are supported in ASP.NET Core MVC.</span></span> <span data-ttu-id="3d09c-237">所使用的方法与 ASP.NET MVC 5 中的区域几乎相同。</span><span class="sxs-lookup"><span data-stu-id="3d09c-237">The approach used is nearly identical to the use of areas in ASP.NET MVC 5.</span></span> <span data-ttu-id="3d09c-238">使用区域迁移代码的开发人员应注意以下差异：</span><span class="sxs-lookup"><span data-stu-id="3d09c-238">Developers migrating code using areas should keep in mind the following differences:</span></span>

- <span data-ttu-id="3d09c-239">`AreaRegistration.RegisterAllAreas` ASP.NET Core MVC 中未使用</span><span class="sxs-lookup"><span data-stu-id="3d09c-239">`AreaRegistration.RegisterAllAreas` is not used in ASP.NET Core MVC</span></span>
- <span data-ttu-id="3d09c-240">使用 `[Area("name")]` 属性（而不是 `RouteArea` ASP.NET MVC 5）应用区域 () </span><span class="sxs-lookup"><span data-stu-id="3d09c-240">Areas are applied using the `[Area("name")]` attribute (not `RouteArea` as in ASP.NET MVC 5)</span></span>
- <span data-ttu-id="3d09c-241">如果需要，可以将区域添加到路由表模板中 (或者可以使用属性路由) </span><span class="sxs-lookup"><span data-stu-id="3d09c-241">Areas can be added to the route table templates, if desired (or they can use attribute routing)</span></span>

<span data-ttu-id="3d09c-242">若要将区域支持添加到 ASP.NET Core MVC 中的路由表，请在 "启动" 中添加以下 `Configure` 内容： </span><span class="sxs-lookup"><span data-stu-id="3d09c-242">To add area support to the route table in ASP.NET Core MVC, you would add the following in `Configure` in *Startup.cs*:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "MyArea",
        pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="3d09c-243">区域还可与属性路由一起使用， (是 `{area}` 在路由定义中使用关键字，它是可用于) 路由模板的多个 [保留路由名称](/aspnet/core/mvc/controllers/routing#reserved-routing-names) 之一。</span><span class="sxs-lookup"><span data-stu-id="3d09c-243">Areas can also be used with attribute routing, using the `{area}` keyword in the route definition (it's one of several [reserved routing names](/aspnet/core/mvc/controllers/routing#reserved-routing-names) that can be used with route templates).</span></span>

<span data-ttu-id="3d09c-244">标记帮助程序支持具有特性的区域 `asp-area` ，这些区域可用于生成 Razor 视图和页面中的链接：</span><span class="sxs-lookup"><span data-stu-id="3d09c-244">Tag helpers support areas with the `asp-area` attribute, which can be used to generate links in Razor views and pages:</span></span>

```razor
<ul>
    <li>
        <a asp-area="Products" asp-controller="Home" asp-action="About">
            Products/Home/About
        </a>
    </li>
    <li>
        <a asp-area="Services" asp-controller="Home" asp-action="About">
            Services About
        </a>
    </li>
    <li>
        <a asp-area="" asp-controller="Home" asp-action="About">
            /Home/About
        </a>
    </li>
</ul>
```

<span data-ttu-id="3d09c-245">如果要迁移到 Razor Pages 需要使用 *页面* 文件夹中的 "*区域*" 文件夹。</span><span class="sxs-lookup"><span data-stu-id="3d09c-245">If you're migrating to Razor Pages you will need to use an *Areas* folder in your *Pages* folder.</span></span> <span data-ttu-id="3d09c-246">有关详细信息，请参阅 [具有 Razor Pages 的区域](/aspnet/core/mvc/controllers/areas#areas-with-razor-pages)。</span><span class="sxs-lookup"><span data-stu-id="3d09c-246">For more information, see [Areas with Razor Pages](/aspnet/core/mvc/controllers/areas#areas-with-razor-pages).</span></span>

<span data-ttu-id="3d09c-247">除了上述指南之外，团队还应查看 [ASP.NET Core 中的路由如何在](/aspnet/core/mvc/controllers/routing#areas) 迁移规划过程中处理区域。</span><span class="sxs-lookup"><span data-stu-id="3d09c-247">In addition to the above guidance, teams should review [how routing in ASP.NET Core works with areas](/aspnet/core/mvc/controllers/routing#areas) as part of their migration planning process.</span></span>

## <a name="integration-tests-for-aspnet-mvc-and-aspnet-web-api"></a><span data-ttu-id="3d09c-248">ASP.NET MVC 和 ASP.NET Web API 的集成测试</span><span class="sxs-lookup"><span data-stu-id="3d09c-248">Integration tests for ASP.NET MVC and ASP.NET Web API</span></span>

<span data-ttu-id="3d09c-249">集成测试是一种自动测试，用于验证应用程序的多个不同部分是否正常合作。</span><span class="sxs-lookup"><span data-stu-id="3d09c-249">Integration tests are automated tests that verify several different parts of an app work together correctly.</span></span> <span data-ttu-id="3d09c-250">编写 ASP.NET MVC 和 ASP.NET Web API 的集成测试通常涉及到将应用程序部署到实际 Web 服务器（如 IIS 的本地实例或 IIS Express），然后使用 HTTP 客户端向此托管应用程序发出请求。</span><span class="sxs-lookup"><span data-stu-id="3d09c-250">Writing integration tests for ASP.NET MVC and ASP.NET Web API usually involved deploying the app to a real web server, such as a local instance of IIS or IIS Express, and then making requests to this hosted application using an HTTP client.</span></span> <span data-ttu-id="3d09c-251">其中一些测试可以使用浏览器自动化工具（如 [Selenium](https://www.selenium.dev/)）与客户端用户界面进行交互，但通常这些测试称为 *UI 测试* ，而不是集成测试。</span><span class="sxs-lookup"><span data-stu-id="3d09c-251">Some of these tests may interact with the client-side user interface using browser automation tools like [Selenium](https://www.selenium.dev/), though often these are referred to as *UI tests* rather than integration tests.</span></span>

<span data-ttu-id="3d09c-252">如果已迁移的应用共享与原始版本相同的行为，则团队用于执行集成测试的任何现有技术 (和 UI 测试) 应继续像以前一样工作。</span><span class="sxs-lookup"><span data-stu-id="3d09c-252">If your migrated app shares the same behavior as its original version, whatever existing technology the team is using to perform integration tests (and UI tests) should continue to work just as it did before.</span></span> <span data-ttu-id="3d09c-253">这些测试通常会不关心到用来托管要测试的应用程序的基础技术，并只通过 HTTP 请求与它交互。</span><span class="sxs-lookup"><span data-stu-id="3d09c-253">These tests are usually indifferent to the underlying technology used to host the app they're testing, and interact with it only through HTTP requests.</span></span> <span data-ttu-id="3d09c-254">在某些情况下，可能会有更多挑战，即测试如何与应用程序进行交互，使其进入每个测试之前的已知良好状态。</span><span class="sxs-lookup"><span data-stu-id="3d09c-254">Where things may get more challenging is with how the tests interact with the app to get it into a known good state prior to each test.</span></span> <span data-ttu-id="3d09c-255">这可能需要进行一些迁移工作，因为与 ASP.NET MVC 或 ASP.NET Web API 相比，配置和启动在 ASP.NET Core 中有明显不同。</span><span class="sxs-lookup"><span data-stu-id="3d09c-255">This may require some migration effort, since configuration and startup are significantly different in ASP.NET Core compared to ASP.NET MVC or ASP.NET Web API.</span></span>

<span data-ttu-id="3d09c-256">团队应认真考虑迁移其集成测试，以使用 [ASP.NET Core 的内置集成测试](/aspnet/core/test/integration-tests) 支持。</span><span class="sxs-lookup"><span data-stu-id="3d09c-256">Teams should strongly consider migrating their integration tests to use [ASP.NET Core's built-in integration testing](/aspnet/core/test/integration-tests) support.</span></span> <span data-ttu-id="3d09c-257">在 ASP.NET Core 中，可以通过将应用部署到使用进行配置的来对其进行测试 `TestHost` `WebApplicationFactory` 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-257">In ASP.NET Core, apps can be tested by deploying them to a `TestHost`, which is configured using a `WebApplicationFactory`.</span></span> <span data-ttu-id="3d09c-258">托管应用程序需要进行一些安装程序以进行测试，但一旦准备就绪，创建单个集成测试就会非常简单。</span><span class="sxs-lookup"><span data-stu-id="3d09c-258">There's a little bit of setup required to host the app for testing, but once this is in place, creating individual integration tests is very straightforward.</span></span>

<span data-ttu-id="3d09c-259">ASP.NET Core 的集成测试支持的最重要功能之一是，应用托管在内存中。</span><span class="sxs-lookup"><span data-stu-id="3d09c-259">One of the best features of ASP.NET Core's integration testing support is that the app is hosted in memory.</span></span> <span data-ttu-id="3d09c-260">不需要配置真实的 web 服务器来托管应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d09c-260">There's no need to configure a real webserver to host the app.</span></span> <span data-ttu-id="3d09c-261">如果只测试 ASP.NET Core 而不是客户端的行为) ，则无需使用浏览器自动化工具 (。</span><span class="sxs-lookup"><span data-stu-id="3d09c-261">There's no need to use a browser automation tool (if you're only testing ASP.NET Core and not client-side behavior).</span></span> <span data-ttu-id="3d09c-262">使用此方法时，尝试使用实际的 web 服务器进行自动集成测试（如防火墙问题或进程开始/停止问题）时可能会遇到许多问题。</span><span class="sxs-lookup"><span data-stu-id="3d09c-262">Many of the problems that can be encountered when trying to use a real web server for automated integration tests, such as firewall issues or process start/stop issues, are eliminated with this approach.</span></span> <span data-ttu-id="3d09c-263">由于请求是在内存中进行的，无需网络要求，因此测试的运行速度往往比必须设置单独的 web 服务器并通过网络与它进行通信的测试速度快得多 (即使在同一台计算机上运行也是如此) 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-263">Since the requests are all made in memory with no network requirement, the tests also tend to run much faster than tests that must set up a separate webserver and communicate with it over the network (even if it's running on the same machine).</span></span>

<span data-ttu-id="3d09c-264">在下面，你可以看到一个示例 ASP.NET Core 集成测试 (有时称为 " *功能测试* "，以将它们与 [eShopOnWeb 引用应用程序](https://github.com/dotnet-architecture/eShopOnWeb)) 的低级别集成测试区分开来：</span><span class="sxs-lookup"><span data-stu-id="3d09c-264">Below you can see an example ASP.NET Core integration test (sometimes referred to as *functional tests* to distinguish them from lower-level integration tests) from the [eShopOnWeb reference application](https://github.com/dotnet-architecture/eShopOnWeb):</span></span>

```csharp
public class GetByIdEndpoint : IClassFixture<ApiTestFixture>
{
    JsonSerializerOptions _jsonOptions = new JsonSerializerOptions { PropertyNameCaseInsensitive = true };

    public GetByIdEndpoint(ApiTestFixture factory)
    {
        Client = factory.CreateClient();
    }

    public HttpClient Client { get; }

    [Fact]
    public async Task ReturnsItemGivenValidId()
    {
        var response = await Client.GetAsync("api/catalog-items/5");
        response.EnsureSuccessStatusCode();
        var stringResponse = await response.Content.ReadAsStringAsync();
        var model = stringResponse.FromJson<GetByIdCatalogItemResponse>();

        Assert.Equal(5, model.CatalogItem.Id);
        Assert.Equal("Roslyn Red Sheet", model.CatalogItem.Name);
    }
}
```

<span data-ttu-id="3d09c-265">如果要迁移的应用没有集成测试，迁移过程可能是添加某些应用的极佳机会。</span><span class="sxs-lookup"><span data-stu-id="3d09c-265">If the app being migrated has no integration tests, the migration process can be a great opportunity to add some.</span></span> <span data-ttu-id="3d09c-266">这些测试可以验证已迁移的应用程序的行为是否与团队期望的行为相同。</span><span class="sxs-lookup"><span data-stu-id="3d09c-266">These tests can verify that the migrated app behaves as the team expects.</span></span> <span data-ttu-id="3d09c-267">如果在迁移中及早部署此类测试，则可以确保以后的迁移工作不会中断以前迁移的应用部分。</span><span class="sxs-lookup"><span data-stu-id="3d09c-267">When such tests are in place early in a migration, they can ensure that later migration efforts do not break previously migrated portions of the app.</span></span> <span data-ttu-id="3d09c-268">由于在 ASP.NET Core 中设置和运行集成测试是多么简单，因此，设置此类测试所用的投资回报通常相当高。</span><span class="sxs-lookup"><span data-stu-id="3d09c-268">Given how easy it is to set up and run integration tests in ASP.NET Core, the return on the investment spent setting up such tests is usually pretty high.</span></span>

## <a name="wcf-client-configuration"></a><span data-ttu-id="3d09c-269">WCF 客户端配置</span><span class="sxs-lookup"><span data-stu-id="3d09c-269">WCF client configuration</span></span>

<span data-ttu-id="3d09c-270">如果你的应用当前依赖 WCF 服务作为客户端，则支持此方案。</span><span class="sxs-lookup"><span data-stu-id="3d09c-270">If your app currently relies on WCF services as a client, this scenario is supported.</span></span> <span data-ttu-id="3d09c-271">但是，你需要将配置从 *web.config* [迁移](/aspnet/core/migration/configuration)到使用文件上的新 *appsettings.js* 。</span><span class="sxs-lookup"><span data-stu-id="3d09c-271">However, you will need to [migrate your configuration](/aspnet/core/migration/configuration) from *web.config* to use the new *appsettings.json* file.</span></span> <span data-ttu-id="3d09c-272">另一种方法是在创建客户端时以编程方式向其添加任何所需的配置。</span><span class="sxs-lookup"><span data-stu-id="3d09c-272">Another option is to add any necessary configuration to your clients programmatically when you create them.</span></span> <span data-ttu-id="3d09c-273">例如：</span><span class="sxs-lookup"><span data-stu-id="3d09c-273">For example:</span></span>

```csharp
var wcfClient = new OrderServiceClient(
    new BasicHttpBinding(BasicHttpSecurityMode.None),
    new EndpointAddress("http://localhost:5050/OrderService.svc"));
```

<span data-ttu-id="3d09c-274">如果你的组织具有使用应用依赖的 WCF 生成的广泛服务，请考虑将它们迁移到使用 gRPC。</span><span class="sxs-lookup"><span data-stu-id="3d09c-274">If your organization has extensive services built using WCF that your app relies on, consider migrating them to use gRPC instead.</span></span> <span data-ttu-id="3d09c-275">有关 gRPC 的更多详细信息，你可能希望迁移的原因，以及详细的迁移指南，请参阅 [WCF 开发人员的 gRPC](/dotnet/architecture/grpc-for-wcf-developers/) 电子书。</span><span class="sxs-lookup"><span data-stu-id="3d09c-275">For more details on gRPC, why you may wish to migrate, and a detailed migration guide, consult the [gRPC for WCF Developers](/dotnet/architecture/grpc-for-wcf-developers/) eBook.</span></span>

## <a name="references"></a><span data-ttu-id="3d09c-276">参考</span><span class="sxs-lookup"><span data-stu-id="3d09c-276">References</span></span>

- [<span data-ttu-id="3d09c-277">ASP.NET Web API 内容协商</span><span class="sxs-lookup"><span data-stu-id="3d09c-277">ASP.NET Web API Content Negotiation</span></span>](/aspnet/web-api/overview/formats-and-model-binding/content-negotiation)
- [<span data-ttu-id="3d09c-278">设置 ASP.NET Core Web API 中响应数据的格式</span><span class="sxs-lookup"><span data-stu-id="3d09c-278">Format response data in ASP.NET Core Web API</span></span>](/aspnet/core/web-api/advanced/formatting)
- [<span data-ttu-id="3d09c-279">ASP.NET Web API 中的自定义模型联编</span><span class="sxs-lookup"><span data-stu-id="3d09c-279">Custom Model Binders in ASP.NET Web API</span></span>](/aspnet/web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api#model-binders)
- [<span data-ttu-id="3d09c-280">ASP.NET Core 中的自定义模型联编</span><span class="sxs-lookup"><span data-stu-id="3d09c-280">Custom Model Binders in ASP.NET Core</span></span>](/aspnet/core/mvc/advanced/custom-model-binding#custom-model-binder-sample)
- <span data-ttu-id="3d09c-281">[ASP.NET Web API 2 中的媒体格式化程序](/aspnet/web-api/overview/formats-and-model-binding/media-formatters)</span><span class="sxs-lookup"><span data-stu-id="3d09c-281">[Media Formatters in ASP.NET Web API 2](/aspnet/web-api/overview/formats-and-model-binding/media-formatters)</span></span>\
- [<span data-ttu-id="3d09c-282">ASP.NET Core Web API 中的自定义格式化程序</span><span class="sxs-lookup"><span data-stu-id="3d09c-282">Custom formatters in ASP.NET Core Web API</span></span>](/aspnet/core/web-api/advanced/custom-formatters)
- [<span data-ttu-id="3d09c-283">ASP.NET Core 中的筛选器</span><span class="sxs-lookup"><span data-stu-id="3d09c-283">Filters in ASP.NET Core</span></span>](/aspnet/core/mvc/controllers/filters)
- [<span data-ttu-id="3d09c-284">ASP.NET Web API 2 中的路由约束</span><span class="sxs-lookup"><span data-stu-id="3d09c-284">Route constraints in ASP.NET Web API 2</span></span>](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints)
- [<span data-ttu-id="3d09c-285">ASP.NET MVC 5 中的路由约束</span><span class="sxs-lookup"><span data-stu-id="3d09c-285">Route constraints in ASP.NET MVC 5</span></span>](https://devblogs.microsoft.com/aspnet/attribute-routing-in-asp-net-mvc-5/#route-constraints)
- [<span data-ttu-id="3d09c-286">ASP.NET Core 路由约束引用</span><span class="sxs-lookup"><span data-stu-id="3d09c-286">ASP.NET Core Route Constraint Reference</span></span>](/aspnet/core/fundamentals/routing#route-constraint-reference)
- [<span data-ttu-id="3d09c-287">ASP.NET Web API 2 中的自定义消息处理程序</span><span class="sxs-lookup"><span data-stu-id="3d09c-287">Custom message handlers in ASP.NET Web API 2</span></span>](/aspnet/web-api/overview/advanced/http-message-handlers#custom-message-handlers)
- [<span data-ttu-id="3d09c-288">MVC 5 和 Web API 2 中的简单 CORS 控制</span><span class="sxs-lookup"><span data-stu-id="3d09c-288">Simple CORS control in MVC 5 and Web API 2</span></span>](https://stackoverflow.com/questions/6290053/setting-access-control-allow-origin-in-asp-net-mvc-simplest-possible-method)
- [<span data-ttu-id="3d09c-289">在 Web API 中启用跨域请求</span><span class="sxs-lookup"><span data-stu-id="3d09c-289">Enabling Cross-Origin Requests in Web API</span></span>](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors)
- [<span data-ttu-id="3d09c-290"> (CORS) 启用跨域请求 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d09c-290">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](/aspnet/core/security/cors)
- [<span data-ttu-id="3d09c-291">ASP.NET Core 中的区域</span><span class="sxs-lookup"><span data-stu-id="3d09c-291">Areas in ASP.NET Core</span></span>](/aspnet/core/mvc/controllers/areas)
- <span data-ttu-id="3d09c-292">ASP.NET Core 中的集成测试  </span><span class="sxs-lookup"><span data-stu-id="3d09c-292">[Integration tests in ASP.NET Core](/aspnet/core/test/integration-tests)</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="3d09c-293">[上一页](example-migration-eshop.md)
>[下一页](deployment-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="3d09c-293">[Previous](example-migration-eshop.md)
[Next](deployment-scenarios.md)</span></span>
