---
title: 更多迁移方案
description: 本部分介绍将 .NET Framework 应用升级到 .NET Core/.NET 5 的其他迁移方案和技术。
author: ardalis
ms.date: 02/11/2021
ms.openlocfilehash: c819fd42cd02da9b643873cda5f2ecf8bc21e559
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231162"
---
# <a name="more-migration-scenarios"></a>更多迁移方案

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

本部分介绍了几种不同的 ASP.NET 应用方案，并提供了解决每个应用方案的特定方法。 你可以使用此部分来确定适用于你的应用程序的方案，并评估适用于你的应用程序及其宿主环境的哪些方法。

## <a name="migrate-aspnet-mvc-5-and-webapi-2-to-aspnet-core-mvc"></a>将 ASP.NET MVC 5 和 WebApi 2 迁移到 ASP.NET Core MVC

ASP.NET MVC 5 和 Web API 2 应用的常见方案是，这两个产品都安装在同一应用程序中。 这是一种支持的、对许多团队使用的相对常见方法，但由于这两种产品使用不同的抽象，因此需要执行一些冗余工作。 例如，设置 ASP.NET MVC 的路由是使用上的方法（ `RouteCollection` 如和）完成 `MapMvcAttributeRoutes()` 的 `MapRoute()` 。 但 ASP.NET Web API 2 路由由和等 `HttpConfiguration` 方法管理 `MapHttpAttributeRoutes()` `MapHttpRoute()` 。

此 `eShopLegacyMVC` 应用包括 ASP.NET MVC 和 WEB API，并在其文件夹中包含 `App_Start` 用于为两者设置路由的方法。 它还支持使用 Autofac 进行依赖关系注入，这还需要配置两组相似的工作：

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

当升级这些应用程序以使用 ASP.NET Core 时，这种重复的工作以及有时会造成混淆。 ASP.NET Core MVC 是一个统一框架，其中包含一组用于路由、筛选器和其他规则的规则。 依赖关系注入内置于 .NET Core 本身。 所有这些都可以在中进行配置 `Startup.cs` ，如示例中的应用程序中所示 `eShopPorted` 。

## <a name="migrate-httpresponsemessage-to-aspnet-core"></a>将 HttpResponseMessage 迁移到 ASP.NET Core

某些 ASP.NET Web API 应用可能具有返回的操作方法 `HttpResponseMessage` 。 ASP.NET Core 中不存在此类型。 下面是在 `Delete` 操作方法中使用的 `ResponseMessage` 帮助器方法的示例 `ApiController` ：

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

在 ASP.NET Core MVC 中，有可用于所有常见 HTTP 响应状态代码的帮助器方法，因此以上方法将移植到以下代码：

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

如果你发现需要返回不存在任何帮助程序的自定义状态代码，你始终可以使用 `return StatusCode(int statusCode)` 返回你喜欢的任何数值代码。

## <a name="migrate-content-negotiation-from-aspnet-web-api-to-aspnet-core"></a>将内容协商从 ASP.NET Web API 迁移到 ASP.NET Core

ASP.NET Web API 2 以本机方式支持 [内容协商](/aspnet/web-api/overview/formats-and-model-binding/content-negotiation) 。 该示例应用包含一个 `BrandsController` ，它通过在 XML 或 JSON 中列出其结果来演示此支持。 这取决于请求的 `Accept` 标头，并在包含或时进行 `application/xml` 更改 `application/json` 。

ASP.NET MVC 5 应用未内置内容协商支持。

内容协商比返回特定的编码类型更好，因为它更灵活，并使 API 可用于更多的客户端。 如果当前具有返回特定格式的操作方法，应考虑在将代码移植到 ASP.NET Core 时修改它们以返回支持内容协商的结果类型。

以下代码将返回 JSON 格式的数据，而不考虑客户端 `Accept` 标头内容：

```csharp
[HttpGet]
public ActionResult Index()
{
    return Json(new { Message = "Hello World!" });
}
```

如果使用了适当的[返回类型](/aspnet/core/web-api/action-return-types)， [ASP.NET Core MVC 将以本机方式支持内容协商](/aspnet/core/web-api/advanced/formatting)。 内容协商由 [ObjectResult] 实现，由控制器 helper 方法返回的状态代码特定操作结果返回。 在 ASP.NET Core MVC 和使用内容协商中实现的上一操作方法为：

```csharp
public IActionResult Index()
{
    return Ok(new { Message = "Hello World!"} );
}
```

这将默认返回 JSON 格式的数据。 [如果已使用适当的格式化程序配置了应用程序](/aspnet/core/web-api/advanced/formatting)，则将使用 XML 和其他格式。

## <a name="custom-model-binding"></a>自定义模型绑定

大多数 ASP.NET MVC 和 Web API 应用使用模型绑定。 默认模型绑定语法在这些应用和 ASP.NET Core MVC 之间完全迁移。 但是，在某些情况下，客户已编写 [自定义模型联编](/aspnet/web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api#model-binders) 以支持特定的模型类型或使用方案。 ASP.NET MVC 和 Web API 项目中的自定义模型联编器 `IModelBinder` 分别使用 `System.Web.Mvc` 和命名空间中定义的单独接口 `System.Web.Http` 。 在这两种情况下，自定义联编程序都公开了一 `Bind` 种方法，该方法接受控制器或操作上下文以及作为参数的模型绑定上下文。

创建自定义联编程序后，必须将其注册到应用程序。 此步骤需要创建另一个类型， `ModelBinderProvider` 它充当工厂并在请求期间创建模型联编程序。 可以 `ApplicationStart` 在 MVC 应用中添加联编程序，如下所示：

```csharp
ModelBinderProviders.BinderProviders.Insert(0, new MyCustomBinderProvider()); // MVC
```

在 Web API 应用程序中，可以使用属性引用自定义联编程序。 `ModelBinder`可以将特性添加到操作方法参数或参数的类型定义，如下所示：

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

若要在 ASP.NET Web API 中全局注册模型联编程序，必须在应用程序启动期间添加其提供程序：

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

将 [自定义模型提供程序迁移到 ASP.NET Core](/aspnet/core/mvc/advanced/custom-model-binding#custom-model-binder-sample)时，Web API 模式比 ASP.NET MVC 5 ASP.NET Core 方法更接近。 ASP.NET Core 的 `IModelBinder` 接口和 WEB API 之间的主要区别在于 ASP.NET Core 方法 (异步 `BindModelAsync`) ，它只需要一个 `BindingModelContext` 参数，而不需要使用 Web API 的版本所需的两个参数。 在 ASP.NET Core 中，可以 `[ModelBinder]` 对各个操作方法参数或其关联的类型使用特性。 你还可以创建一个 `ModelBinderProvider` ，它将在适当的情况下在应用中全局使用。 若要配置此类提供程序，请将代码添加到 `Startup` 中的 `ConfigureServices` ：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers(options =>
    {
        options.ModelBinderProviders.Insert(0, new CustomModelBinderProvider());
    });
}
```

## <a name="media-formatters"></a>媒体格式化程序

ASP.NET Web API 支持多种媒体格式，可以使用自定义媒体格式化程序进行扩展。 文档介绍可用于以逗号分隔值格式发送数据的 [示例 CSV 媒体格式化程序](/aspnet/web-api/overview/formats-and-model-binding/media-formatters#example-creating-a-csv-media-formatter) 。 如果 Web API 应用使用自定义媒体格式化程序，则需要将其转换为 [ASP.NET Core 自定义格式化](/aspnet/core/web-api/advanced/custom-formatters)程序。

若要在 Web API 2 中创建自定义格式化程序，请从相应的基类继承，然后使用对象将格式化程序添加到 Web API 管道 `HttpConfiguration` ：

```csharp
public static void ConfigureApis(HttpConfiguration config)
{
    config.Formatters.Add(new ProductCsvFormatter());
}
```

在 ASP.NET Core 中，此过程类似。 ASP.NET Core 支持) 的和输出格式化程序 (使用的输入格式化程序， (用于对) 响应进行格式设置。 以特定方式向输出响应添加自定义格式化程序涉及从适当的基类继承，并将格式化程序添加到中的 MVC `Startup` ：

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

你将在 [AspNetCore](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.formatters) 命名空间中找到基类的完整列表。

从 Web API 格式化程序迁移到 ASP.NET Core MVC 格式化程序的步骤如下：

1. 为新的格式化程序标识相应的基类。
1. 创建基类的新实例，并实现其所需的方法。
1. 将 Web API 格式化程序中的功能复制到新的实现。
1. 将 ASP.NET Core 应用的方法中的 MVC 配置 `ConfigureServices` 为使用新的格式化程序。

## <a name="custom-filters"></a>自定义筛选器

筛选器在 ASP.NET Core 应用中用于在请求处理管道中的特定阶段之前和/或之后执行代码。 ASP.NET MVC 和 Web API 还使用筛选器的方式几乎相同，但详细信息会有所不同。 例如， [ASP.NET MVC 支持四种类型的筛选器](/aspnet/mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs#the-different-types-of-filters)。 ASP.NET Web API 2 支持类似的筛选器，并且 MVC 和 Web API 都包含用于 [重写筛选器](/dotnet/api/system.web.mvc.filters.ioverridefilter)的属性。

ASP.NET MVC 和 Web API 应用中使用的最常见筛选器是由 [IActionFilter 接口](/dotnet/api/system.web.mvc.iactionfilter)定义的操作筛选器。 此接口在)  (之前提供方法，在 `OnActionExecuting` `OnActionExecuted` 执行操作之前和/或之后 () 可用于执行代码，如每个方法所述。

ASP.NET Core 继续支持筛选器，并且其 MVC 和 Web API 的统一是指其实现仅有一种方法。 [文档包括 ASP.NET CORE MVC 中内置的5个 () 类筛选器的详细信息](/aspnet/core/mvc/controllers/filters#filter-types)。 ASP.NET MVC 和 ASP.NET Web API 中支持的所有筛选器变体在 ASP.NET Core 中都具有关联的版本，因此，通常只需标识适当的接口和/或基类，并将代码迁移到。

除同步接口外，ASP.NET Core 还提供了异步接口，如 `IAsyncActionFilter` 提供可用于合并代码以在操作之前和之后运行的异步方法，如下所示：

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

迁移异步代码 (或应异步) 的代码时，团队应考虑利用为此目的提供的内置异步类型。

大多数 ASP.NET MVC 和 Web API 应用不使用大量的自定义筛选器。 由于 ASP.NET Core MVC 中筛选器的方法与 ASP.NET MVC 和 Web API 中的筛选器密切一致，因此，迁移自定义筛选器通常非常简单。 请务必阅读 ASP.NET Core 文档中有关筛选器的详细文档，一旦确信已充分了解这些文档，请将旧系统中的逻辑移植到新系统的筛选器。

## <a name="route-constraints"></a>路由约束

ASP.NET Core 使用路由约束来帮助确保正确路由请求以便路由请求。 [ASP.NET Core 支持大量不同的路由约束用于此目的]/aspnet/core/fundamentals/routing # 路由约束-引用) 。 路由约束可以应用于路由表中，但使用 ASP.NET MVC 5 和/或 [ASP.NET Web API 2](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints) 生成的大多数应用都使用应用于属性路由的内联路由约束。 内联路由约束使用如下格式：

```csharp
[Route("/customer/{id:int}")]
```

`:int` `id` 路由参数后的值会限制值，以匹配 `int` 类型。 使用路由约束的一个优点是，它们允许两个完全相同的路由存在，其中的参数仅有其类型不同。 这使得仅基于参数类型的路由的 [方法重载](/dotnet/standard/design-guidelines/member-overloading) 是等效的。

路由约束、其语法和用法的集合在所有这三种方法之间非常类似。 自定义路由约束在客户应用程序中非常罕见。 如果你的应用程序使用自定义路由约束，并且需要 ASP.NET Core 端口，则这些文档将包含演示 [如何在 ASP.NET Core 中创建自定义路由约束](/aspnet/core/fundamentals/routing#custom-route-constraints)的示例。 实质上，只需实现 `IRouteConstraint` 和 `Match` 方法，然后在配置应用的路由时添加自定义约束：

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

这与在 ASP.NET Web API 中使用自定义约束的方式非常类似，后者使用 `IHttpRouteConstraint` 解析程序并对其进行配置 `HttpConfiguration.MapHttpAttributeRoutes` ：

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

ASP.NET MVC 5 遵循非常类似的方法， `IRouteConstraint` 它使用作为其接口名称并将约束配置为路由配置的一部分：

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

将路由约束使用情况以及自定义路由约束迁移到 ASP.NET Core 通常非常简单。

## <a name="custom-route-handlers"></a>自定义路由处理程序

ASP.NET MVC 5 的另一个相当高级的功能是路由处理程序。 自定义路由处理程序实现 `IRouteHandler` ，其中包括一个方法，该方法返回 `IHttpHandler` 给定请求的。 反过来，会 `IHttpHandler` 公开一个 `IsReusable` 属性和一个 `ProcessRequest` 方法。 在 ASP.NET MVC 5 中，您可以将路由表中的特定路由配置为使用您的自定义处理程序：

```csharp
public static void RegisterRoutes(RouteCollection routes)
{
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

    routes.Add(new Route("custom", new CustomRouteHandler()));
}
```

若要将自定义路由处理程序从 ASP.NET MVC 5 迁移到 ASP.NET Core，可以使用筛选器 (例如操作筛选器) 或自定义 [`IRouter`](/dotnet/api/microsoft.aspnetcore.routing.irouter) 。 筛选器方法相对简单，并且在 MVC 添加到中时，可以作为全局筛选器添加 `ConfigureServices` 。 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Filters.Add(typeof(CustomActionFilter));
    });
}
```

`IRouter`选项需要实现接口的 `RouteAsync` 和 `GetVirtualPath` 方法。 自定义路由器将添加到 `Configure` *启动 .cs* 的方法中的请求管道。

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

在 ASP.NET Web API 中，这些处理程序称为 [自定义消息处理程序](/aspnet/web-api/overview/advanced/http-message-handlers#custom-message-handlers)，而不是 *路由处理程序*。 消息处理程序必须派生自 `DelegatingHandler` 并重写其 `SendAsync` 方法。 消息处理程序可以链接在一起形成管道，其方式与 ASP.NET Core 中间件及其请求管道非常类似。

ASP.NET Core 没有 `DelegatingHandler` 类型或单独的消息处理程序管道。 相反，此类处理程序应使用全局筛选器、自定义 `IRouter` 实例 (参见以上) 或自定义中间件。 ASP.NET Core MVC 筛选器和 `IRouter` 类型具有对 mvc 构造（如控制器和操作）的内置访问的优势，而中间件则是与 mvc 无任何依赖项的较低级别方法。 这使得它更灵活，但如果需要访问 MVC 组件，还需要更多的精力。

## <a name="cors-support"></a>CORS 支持

CORS 或跨域资源共享是一种 W3C 标准，使服务器可以接受来自他们所提供的响应的请求。 ASP.NET MVC 5 和 ASP.NET Web API 2 以不同方式支持 CORS。 在 ASP.NET MVC 5 中启用 CORS 支持的最简单方法是使用如下所示的操作筛选器：

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

ASP.NET Web API 还可以使用此类筛选器，但它也具有 [启用 CORS 的内置支持](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors) ：

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

添加后，可以使用属性配置允许的源、标头和方法 `EnableCors` ，如下所示：

```csharp
[EnableCors(origins: "https://dot.net", headers: "*", methods: "*")]
public class TestController : ApiController
{
    // Controller methods not shown...
}
```

在从 ASP.NET MVC 5 或 ASP.NET Web API 2 迁移 CORS 实现之前，请务必查看 [cors 的工作原理](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works) ，并创建一些自动测试，这些测试演示了在当前系统中按预期方式工作的 cors。

在 ASP.NET Core 中，有三种内置方式来启用 CORS：

- [通过策略](/aspnet/core/security/cors?#cors-with-named-policy-and-middleware) 在中配置 `ConfigureServices`
- 通过[终结点路由](/aspnet/core/security/cors?#enable-cors-with-endpoint-routing)启用
- 已启用[ `EnableCors` 属性](/aspnet/core/security/cors?view=aspnetcore-5.0#enable-cors-with-attributes)

其中的每个方法都在文档中进行了详细介绍，这些文档与上述选项关联。 你选择哪一种取决于现有应用如何支持 CORS。 如果应用使用属性，则可能会迁移到 `EnableCors` 最轻松地使用属性。 如果你的应用使用筛选器，你可以继续使用该方法 (但这并不是 ASP.NET Core) 中使用的典型方法，或者迁移以使用属性或策略。 终结点路由是 ASP.NET Core 3 中引入的一项相对较新的功能，因此它在 ASP.NET MVC 5 或 ASP.NET Web API 2 应用程序中没有关闭模拟功能。

## <a name="custom-areas"></a>自定义区域

许多 ASP.NET MVC 应用使用区域来组织项目。 区域通常位于项目根目录中的 " *区域* " 文件夹中，并且必须在应用程序启动时注册，通常在中 `Application_Start()` ：

```csharp
AreaRegistration.RegisterAllAreas();
```

注册启动中的所有区域的替代方法是在 `RouteArea` 各个控制器上使用属性：

```csharp
[RouteArea("Admin")]
public class SomeController : Controller
```

使用区域时，会将其他参数传递给 HTML 帮助器方法，以生成到不同区域中的操作的链接：

```cshtml
@Html.ActionLink("News", "Index", "News", new { area = "News" }, null)
```

ASP.NET Web API 应用通常不会显式使用区域，因为它们的控制器可以放置在项目中的任何文件夹中。 团队可以使用他们喜欢的任何文件夹结构来组织其 API 控制器。

ASP.NET Core MVC 中支持[区域](/aspnet/core/mvc/controllers/areas)。 所使用的方法与 ASP.NET MVC 5 中的区域几乎相同。 使用区域迁移代码的开发人员应注意以下差异：

- `AreaRegistration.RegisterAllAreas` ASP.NET Core MVC 中未使用
- 使用 `[Area("name")]` 属性（而不是 `RouteArea` ASP.NET MVC 5）应用区域 () 
- 如果需要，可以将区域添加到路由表模板中 (或者可以使用属性路由) 

若要将区域支持添加到 ASP.NET Core MVC 中的路由表，请在 "启动" 中添加以下 `Configure` 内容： 

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

区域还可与属性路由一起使用， (是 `{area}` 在路由定义中使用关键字，它是可用于) 路由模板的多个 [保留路由名称](/aspnet/core/mvc/controllers/routing#reserved-routing-names) 之一。

标记帮助程序支持具有特性的区域 `asp-area` ，这些区域可用于生成 Razor 视图和页面中的链接：

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

如果要迁移到 Razor Pages 需要使用 *页面* 文件夹中的 "*区域*" 文件夹。 有关详细信息，请参阅 [具有 Razor Pages 的区域](/aspnet/core/mvc/controllers/areas#areas-with-razor-pages)。

除了上述指南之外，团队还应查看 [ASP.NET Core 中的路由如何在](/aspnet/core/mvc/controllers/routing#areas) 迁移规划过程中处理区域。

## <a name="integration-tests-for-aspnet-mvc-and-aspnet-web-api"></a>ASP.NET MVC 和 ASP.NET Web API 的集成测试

集成测试是一种自动测试，用于验证应用程序的多个不同部分是否正常合作。 编写 ASP.NET MVC 和 ASP.NET Web API 的集成测试通常涉及到将应用程序部署到实际 Web 服务器（如 IIS 的本地实例或 IIS Express），然后使用 HTTP 客户端向此托管应用程序发出请求。 其中一些测试可以使用浏览器自动化工具（如 [Selenium](https://www.selenium.dev/)）与客户端用户界面进行交互，但通常这些测试称为 *UI 测试* ，而不是集成测试。

如果已迁移的应用共享与原始版本相同的行为，则团队用于执行集成测试的任何现有技术 (和 UI 测试) 应继续像以前一样工作。 这些测试通常会不关心到用来托管要测试的应用程序的基础技术，并只通过 HTTP 请求与它交互。 在某些情况下，可能会有更多挑战，即测试如何与应用程序进行交互，使其进入每个测试之前的已知良好状态。 这可能需要进行一些迁移工作，因为与 ASP.NET MVC 或 ASP.NET Web API 相比，配置和启动在 ASP.NET Core 中有明显不同。

团队应认真考虑迁移其集成测试，以使用 [ASP.NET Core 的内置集成测试](/aspnet/core/test/integration-tests) 支持。 在 ASP.NET Core 中，可以通过将应用部署到使用进行配置的来对其进行测试 `TestHost` `WebApplicationFactory` 。 托管应用程序需要进行一些安装程序以进行测试，但一旦准备就绪，创建单个集成测试就会非常简单。

ASP.NET Core 的集成测试支持的最重要功能之一是，应用托管在内存中。 不需要配置真实的 web 服务器来托管应用程序。 如果只测试 ASP.NET Core 而不是客户端的行为) ，则无需使用浏览器自动化工具 (。 使用此方法时，尝试使用实际的 web 服务器进行自动集成测试（如防火墙问题或进程开始/停止问题）时可能会遇到许多问题。 由于请求是在内存中进行的，无需网络要求，因此测试的运行速度往往比必须设置单独的 web 服务器并通过网络与它进行通信的测试速度快得多 (即使在同一台计算机上运行也是如此) 。

在下面，你可以看到一个示例 ASP.NET Core 集成测试 (有时称为 " *功能测试* "，以将它们与 [eShopOnWeb 引用应用程序](https://github.com/dotnet-architecture/eShopOnWeb)) 的低级别集成测试区分开来：

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

如果要迁移的应用没有集成测试，迁移过程可能是添加某些应用的极佳机会。 这些测试可以验证已迁移的应用程序的行为是否与团队期望的行为相同。 如果在迁移中及早部署此类测试，则可以确保以后的迁移工作不会中断以前迁移的应用部分。 由于在 ASP.NET Core 中设置和运行集成测试是多么简单，因此，设置此类测试所用的投资回报通常相当高。

## <a name="wcf-client-configuration"></a>WCF 客户端配置

如果你的应用当前依赖 WCF 服务作为客户端，则支持此方案。 但是，你需要将配置从 *web.config* [迁移](/aspnet/core/migration/configuration)到使用文件上的新 *appsettings.js* 。 另一种方法是在创建客户端时以编程方式向其添加任何所需的配置。 例如：

```csharp
var wcfClient = new OrderServiceClient(
    new BasicHttpBinding(BasicHttpSecurityMode.None),
    new EndpointAddress("http://localhost:5050/OrderService.svc"));
```

如果你的组织具有使用应用依赖的 WCF 生成的广泛服务，请考虑将它们迁移到使用 gRPC。 有关 gRPC 的更多详细信息，你可能希望迁移的原因，以及详细的迁移指南，请参阅 [WCF 开发人员的 gRPC](/dotnet/architecture/grpc-for-wcf-developers/) 电子书。

## <a name="references"></a>参考

- [ASP.NET Web API 内容协商](/aspnet/web-api/overview/formats-and-model-binding/content-negotiation)
- [设置 ASP.NET Core Web API 中响应数据的格式](/aspnet/core/web-api/advanced/formatting)
- [ASP.NET Web API 中的自定义模型联编](/aspnet/web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api#model-binders)
- [ASP.NET Core 中的自定义模型联编](/aspnet/core/mvc/advanced/custom-model-binding#custom-model-binder-sample)
- [ASP.NET Web API 2 中的媒体格式化程序](/aspnet/web-api/overview/formats-and-model-binding/media-formatters)\
- [ASP.NET Core Web API 中的自定义格式化程序](/aspnet/core/web-api/advanced/custom-formatters)
- [ASP.NET Core 中的筛选器](/aspnet/core/mvc/controllers/filters)
- [ASP.NET Web API 2 中的路由约束](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints)
- [ASP.NET MVC 5 中的路由约束](https://devblogs.microsoft.com/aspnet/attribute-routing-in-asp-net-mvc-5/#route-constraints)
- [ASP.NET Core 路由约束引用](/aspnet/core/fundamentals/routing#route-constraint-reference)
- [ASP.NET Web API 2 中的自定义消息处理程序](/aspnet/web-api/overview/advanced/http-message-handlers#custom-message-handlers)
- [MVC 5 和 Web API 2 中的简单 CORS 控制](https://stackoverflow.com/questions/6290053/setting-access-control-allow-origin-in-asp-net-mvc-simplest-possible-method)
- [在 Web API 中启用跨域请求](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors)
- [ (CORS) 启用跨域请求 ASP.NET Core](/aspnet/core/security/cors)
- [ASP.NET Core 中的区域](/aspnet/core/mvc/controllers/areas)
- ASP.NET Core 中的集成测试  

>[!div class="step-by-step"]
>[上一页](example-migration-eshop.md)
>[下一页](deployment-scenarios.md)
