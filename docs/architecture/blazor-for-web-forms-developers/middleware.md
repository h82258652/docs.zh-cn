---
title: 模块、处理程序和中间件
description: 了解如何使用模块、处理程序和中间件处理 HTTP 请求。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 10/11/2019
ms.openlocfilehash: dbb0a94b0401d58139c024fd8ca3e00353a19efa
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97678032"
---
# <a name="modules-handlers-and-middleware"></a>模块、处理程序和中间件

ASP.NET Core 应用基于一系列中间件构建。 中间件是排列到管道中的处理程序，用于处理请求和响应。 在 Web Forms 应用中，HTTP 处理程序和模块解决了类似的问题。 ASP.NET Core 中，中间件取代了模块、处理程序、Global.asax.cs 和应用生命周期。 在本章中，你将了解 Blazor 应用背景下的中间件。

## <a name="overview"></a>概述

ASP.NET Core 请求管道包含一系列请求委托，依次调用。 下图演示了这一概念。 沿黑色箭头执行。

![管道](media/middleware/request-delegate-pipeline.png)

上面的关系图缺少生命周期事件的概念。 此概念是处理 ASP.NET Web Forms 请求的基础。 此系统使推理正在发生的过程更加容易，并允许在任意点插入中间件。 中间件按其添加到请求管道中的顺序执行。 它们还可以添加到代码中，而不是配置文件中（通常是在 Startup.cs 中）。

## <a name="katana"></a>Katana

熟悉 Katana 的读者会觉得使用 ASP.NET Core 非常轻松。 事实上，Katana 是派生出 ASP.NET Core 的框架。 它为 ASP.NET 4.x 引入了类似的中间件和管道模式。 针对 Katana 设计的中间件可以改为使用 ASP.NET Core 管道。

## <a name="common-middleware"></a>常见中间件

ASP.NET 4.x 包含许多模块。 同样，ASP.NET Core 还提供了许多中间件组件。 在某些情况下，可以将 IIS 模块 与 ASP.NET Core 配合使用。 在其他情况下，可以使用本机 ASP.NET Core 中间件。

下表列出了 ASP.NET Core 中的替换中间件和组件。

|模块                 |ASP.NET 4.x 模块           |ASP.NET Core 选项|
|-----------------------|-----------------------------|-------------------|
|HTTP 错误            |`CustomErrorModule`          |[状态代码页中间件](/aspnet/core/fundamentals/error-handling#usestatuscodepages)|
|默认文档       |`DefaultDocumentModule`      |[默认文件中间件](/aspnet/core/fundamentals/static-files#serve-a-default-document)|
|目录浏览     |`DirectoryListingModule`     |[目录浏览中间件](/aspnet/core/fundamentals/static-files#enable-directory-browsing)|
|动态压缩    |`DynamicCompressionModule`   |[响应压缩中间件](/aspnet/core/performance/response-compression)|
|失败的请求跟踪|`FailedRequestsTracingModule`|[ASP.NET Core 日志记录](/aspnet/core/fundamentals/logging/index#tracesource-provider)|
|文件缓存           |`FileCacheModule`            |[响应缓存中间件](/aspnet/core/performance/caching/middleware)|
|HTTP 缓存           |`HttpCacheModule`            |[响应缓存中间件](/aspnet/core/performance/caching/middleware)|
|HTTP 日志记录           |`HttpLoggingModule`          |[ASP.NET Core 日志记录](/aspnet/core/fundamentals/logging/index)|
|HTTP 重定向       |`HttpRedirectionModule`      |[URL 重写中间件](/aspnet/core/fundamentals/url-rewriting)|
|ISAPI 筛选器          |`IsapiFilterModule`          |[中间件](/aspnet/core/fundamentals/middleware/index)|
|ISAPI                  |`IsapiModule`                |[中间件](/aspnet/core/fundamentals/middleware/index)|
|请求筛选      |`RequestFilteringModule`     |[URL 重写中间件 IRule](/aspnet/core/fundamentals/url-rewriting#irule-based-rule)|
|URL 重写&#8224;   |`RewriteModule`              |[URL 重写中间件](/aspnet/core/fundamentals/url-rewriting)|
|静态压缩     |`StaticCompressionModule`    |[响应压缩中间件](/aspnet/core/performance/response-compression)|
|静态内容         |`StaticFileModule`           |[静态文件中间件](/aspnet/core/fundamentals/static-files)|
|URL 授权      |`UrlAuthorizationModule`     |[ASP.NET Core 标识](/aspnet/core/security/authentication/identity)|

此列表并不详尽，但它应该能够提供两个框架之间存在哪些映射的概念。 若要获取更详细的列表，请参阅[使用 ASP.NET Core 的 IIS 模块](/aspnet/core/host-and-deploy/iis/modules)。

## <a name="custom-middleware"></a>自定义中间件

内置中间件可能无法处理应用所需的所有场景。 在这种情况下，创建自己的中间件非常有意义。 定义中间件的方式有多种，最简单的方法是使用简单的委托。 请考虑以下中间件，它接受来自查询字符串的区域性请求：

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            var cultureQuery = context.Request.Query["culture"];

            if (!string.IsNullOrWhiteSpace(cultureQuery))
            {
                var culture = new CultureInfo(cultureQuery);

                CultureInfo.CurrentCulture = culture;
                CultureInfo.CurrentUICulture = culture;
            }

            // Call the next delegate/middleware in the pipeline
            await next();
        });

        app.Run(async (context) =>
            await context.Response.WriteAsync(
                $"Hello {CultureInfo.CurrentCulture.DisplayName}"));
    }
}
```

还可以通过实现 `IMiddleware` 接口或按照中间件约定将中间件定义为类。 有关详细信息，请参阅[编写自定义 ASP.NET Core 中间件](/aspnet/core/fundamentals/middleware/write)。

>[!div class="step-by-step"]
>[上一页](data.md)
>[下一页](config.md)
