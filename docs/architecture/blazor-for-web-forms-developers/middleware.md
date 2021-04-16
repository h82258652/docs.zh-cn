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
# <a name="modules-handlers-and-middleware"></a><span data-ttu-id="81d8d-103">模块、处理程序和中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-103">Modules, handlers, and middleware</span></span>

<span data-ttu-id="81d8d-104">ASP.NET Core 应用基于一系列中间件构建。</span><span class="sxs-lookup"><span data-stu-id="81d8d-104">An ASP.NET Core app is built upon a series of *middleware*.</span></span> <span data-ttu-id="81d8d-105">中间件是排列到管道中的处理程序，用于处理请求和响应。</span><span class="sxs-lookup"><span data-stu-id="81d8d-105">Middleware is handlers that are arranged into a pipeline to handle requests and responses.</span></span> <span data-ttu-id="81d8d-106">在 Web Forms 应用中，HTTP 处理程序和模块解决了类似的问题。</span><span class="sxs-lookup"><span data-stu-id="81d8d-106">In a Web Forms app, HTTP handlers and modules solve similar problems.</span></span> <span data-ttu-id="81d8d-107">ASP.NET Core 中，中间件取代了模块、处理程序、Global.asax.cs 和应用生命周期。</span><span class="sxs-lookup"><span data-stu-id="81d8d-107">In ASP.NET Core, modules, handlers, *Global.asax.cs*, and the app life cycle are replaced with middleware.</span></span> <span data-ttu-id="81d8d-108">在本章中，你将了解 Blazor 应用背景下的中间件。</span><span class="sxs-lookup"><span data-stu-id="81d8d-108">In this chapter, you'll learn about middleware in the context of a Blazor app.</span></span>

## <a name="overview"></a><span data-ttu-id="81d8d-109">概述</span><span class="sxs-lookup"><span data-stu-id="81d8d-109">Overview</span></span>

<span data-ttu-id="81d8d-110">ASP.NET Core 请求管道包含一系列请求委托，依次调用。</span><span class="sxs-lookup"><span data-stu-id="81d8d-110">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="81d8d-111">下图演示了这一概念。</span><span class="sxs-lookup"><span data-stu-id="81d8d-111">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="81d8d-112">沿黑色箭头执行。</span><span class="sxs-lookup"><span data-stu-id="81d8d-112">The thread of execution follows the black arrows.</span></span>

![管道](media/middleware/request-delegate-pipeline.png)

<span data-ttu-id="81d8d-114">上面的关系图缺少生命周期事件的概念。</span><span class="sxs-lookup"><span data-stu-id="81d8d-114">The preceding diagram lacks a concept of lifecycle events.</span></span> <span data-ttu-id="81d8d-115">此概念是处理 ASP.NET Web Forms 请求的基础。</span><span class="sxs-lookup"><span data-stu-id="81d8d-115">This concept is foundational to how ASP.NET Web Forms requests are handled.</span></span> <span data-ttu-id="81d8d-116">此系统使推理正在发生的过程更加容易，并允许在任意点插入中间件。</span><span class="sxs-lookup"><span data-stu-id="81d8d-116">This system makes it easier to reason about what process is occurring and allows middleware to be inserted at any point.</span></span> <span data-ttu-id="81d8d-117">中间件按其添加到请求管道中的顺序执行。</span><span class="sxs-lookup"><span data-stu-id="81d8d-117">Middleware executes in the order in which it's added to the request pipeline.</span></span> <span data-ttu-id="81d8d-118">它们还可以添加到代码中，而不是配置文件中（通常是在 Startup.cs 中）。</span><span class="sxs-lookup"><span data-stu-id="81d8d-118">They're also added in code instead of configuration files, usually in *Startup.cs*.</span></span>

## <a name="katana"></a><span data-ttu-id="81d8d-119">Katana</span><span class="sxs-lookup"><span data-stu-id="81d8d-119">Katana</span></span>

<span data-ttu-id="81d8d-120">熟悉 Katana 的读者会觉得使用 ASP.NET Core 非常轻松。</span><span class="sxs-lookup"><span data-stu-id="81d8d-120">Readers familiar with Katana will feel comfortable in ASP.NET Core.</span></span> <span data-ttu-id="81d8d-121">事实上，Katana 是派生出 ASP.NET Core 的框架。</span><span class="sxs-lookup"><span data-stu-id="81d8d-121">In fact, Katana is a framework from which ASP.NET Core derives.</span></span> <span data-ttu-id="81d8d-122">它为 ASP.NET 4.x 引入了类似的中间件和管道模式。</span><span class="sxs-lookup"><span data-stu-id="81d8d-122">It introduced similar middleware and pipeline patterns for ASP.NET 4.x.</span></span> <span data-ttu-id="81d8d-123">针对 Katana 设计的中间件可以改为使用 ASP.NET Core 管道。</span><span class="sxs-lookup"><span data-stu-id="81d8d-123">Middleware designed for Katana can be adapted to work with the ASP.NET Core pipeline.</span></span>

## <a name="common-middleware"></a><span data-ttu-id="81d8d-124">常见中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-124">Common middleware</span></span>

<span data-ttu-id="81d8d-125">ASP.NET 4.x 包含许多模块。</span><span class="sxs-lookup"><span data-stu-id="81d8d-125">ASP.NET 4.x includes many modules.</span></span> <span data-ttu-id="81d8d-126">同样，ASP.NET Core 还提供了许多中间件组件。</span><span class="sxs-lookup"><span data-stu-id="81d8d-126">In a similar fashion, ASP.NET Core has many middleware components available as well.</span></span> <span data-ttu-id="81d8d-127">在某些情况下，可以将 IIS 模块 与 ASP.NET Core 配合使用。</span><span class="sxs-lookup"><span data-stu-id="81d8d-127">IIS modules may be used in some cases with ASP.NET Core.</span></span> <span data-ttu-id="81d8d-128">在其他情况下，可以使用本机 ASP.NET Core 中间件。</span><span class="sxs-lookup"><span data-stu-id="81d8d-128">In other cases, native ASP.NET Core middleware may be available.</span></span>

<span data-ttu-id="81d8d-129">下表列出了 ASP.NET Core 中的替换中间件和组件。</span><span class="sxs-lookup"><span data-stu-id="81d8d-129">The following table lists replacement middleware and components in ASP.NET Core.</span></span>

|<span data-ttu-id="81d8d-130">模块</span><span class="sxs-lookup"><span data-stu-id="81d8d-130">Module</span></span>                 |<span data-ttu-id="81d8d-131">ASP.NET 4.x 模块</span><span class="sxs-lookup"><span data-stu-id="81d8d-131">ASP.NET 4.x module</span></span>           |<span data-ttu-id="81d8d-132">ASP.NET Core 选项</span><span class="sxs-lookup"><span data-stu-id="81d8d-132">ASP.NET Core option</span></span>|
|-----------------------|-----------------------------|-------------------|
|<span data-ttu-id="81d8d-133">HTTP 错误</span><span class="sxs-lookup"><span data-stu-id="81d8d-133">HTTP errors</span></span>            |`CustomErrorModule`          |[<span data-ttu-id="81d8d-134">状态代码页中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-134">Status Code Pages Middleware</span></span>](/aspnet/core/fundamentals/error-handling#usestatuscodepages)|
|<span data-ttu-id="81d8d-135">默认文档</span><span class="sxs-lookup"><span data-stu-id="81d8d-135">Default document</span></span>       |`DefaultDocumentModule`      |[<span data-ttu-id="81d8d-136">默认文件中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-136">Default Files Middleware</span></span>](/aspnet/core/fundamentals/static-files#serve-a-default-document)|
|<span data-ttu-id="81d8d-137">目录浏览</span><span class="sxs-lookup"><span data-stu-id="81d8d-137">Directory browsing</span></span>     |`DirectoryListingModule`     |[<span data-ttu-id="81d8d-138">目录浏览中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-138">Directory Browsing Middleware</span></span>](/aspnet/core/fundamentals/static-files#enable-directory-browsing)|
|<span data-ttu-id="81d8d-139">动态压缩</span><span class="sxs-lookup"><span data-stu-id="81d8d-139">Dynamic compression</span></span>    |`DynamicCompressionModule`   |[<span data-ttu-id="81d8d-140">响应压缩中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-140">Response Compression Middleware</span></span>](/aspnet/core/performance/response-compression)|
|<span data-ttu-id="81d8d-141">失败的请求跟踪</span><span class="sxs-lookup"><span data-stu-id="81d8d-141">Failed requests tracing</span></span>|`FailedRequestsTracingModule`|[<span data-ttu-id="81d8d-142">ASP.NET Core 日志记录</span><span class="sxs-lookup"><span data-stu-id="81d8d-142">ASP.NET Core Logging</span></span>](/aspnet/core/fundamentals/logging/index#tracesource-provider)|
|<span data-ttu-id="81d8d-143">文件缓存</span><span class="sxs-lookup"><span data-stu-id="81d8d-143">File caching</span></span>           |`FileCacheModule`            |[<span data-ttu-id="81d8d-144">响应缓存中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-144">Response Caching Middleware</span></span>](/aspnet/core/performance/caching/middleware)|
|<span data-ttu-id="81d8d-145">HTTP 缓存</span><span class="sxs-lookup"><span data-stu-id="81d8d-145">HTTP caching</span></span>           |`HttpCacheModule`            |[<span data-ttu-id="81d8d-146">响应缓存中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-146">Response Caching Middleware</span></span>](/aspnet/core/performance/caching/middleware)|
|<span data-ttu-id="81d8d-147">HTTP 日志记录</span><span class="sxs-lookup"><span data-stu-id="81d8d-147">HTTP logging</span></span>           |`HttpLoggingModule`          |[<span data-ttu-id="81d8d-148">ASP.NET Core 日志记录</span><span class="sxs-lookup"><span data-stu-id="81d8d-148">ASP.NET Core Logging</span></span>](/aspnet/core/fundamentals/logging/index)|
|<span data-ttu-id="81d8d-149">HTTP 重定向</span><span class="sxs-lookup"><span data-stu-id="81d8d-149">HTTP redirection</span></span>       |`HttpRedirectionModule`      |[<span data-ttu-id="81d8d-150">URL 重写中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-150">URL Rewriting Middleware</span></span>](/aspnet/core/fundamentals/url-rewriting)|
|<span data-ttu-id="81d8d-151">ISAPI 筛选器</span><span class="sxs-lookup"><span data-stu-id="81d8d-151">ISAPI filters</span></span>          |`IsapiFilterModule`          |[<span data-ttu-id="81d8d-152">中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-152">Middleware</span></span>](/aspnet/core/fundamentals/middleware/index)|
|<span data-ttu-id="81d8d-153">ISAPI</span><span class="sxs-lookup"><span data-stu-id="81d8d-153">ISAPI</span></span>                  |`IsapiModule`                |[<span data-ttu-id="81d8d-154">中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-154">Middleware</span></span>](/aspnet/core/fundamentals/middleware/index)|
|<span data-ttu-id="81d8d-155">请求筛选</span><span class="sxs-lookup"><span data-stu-id="81d8d-155">Request filtering</span></span>      |`RequestFilteringModule`     |[<span data-ttu-id="81d8d-156">URL 重写中间件 IRule</span><span class="sxs-lookup"><span data-stu-id="81d8d-156">URL Rewriting Middleware IRule</span></span>](/aspnet/core/fundamentals/url-rewriting#irule-based-rule)|
|<span data-ttu-id="81d8d-157">URL 重写&#8224;</span><span class="sxs-lookup"><span data-stu-id="81d8d-157">URL rewriting&#8224;</span></span>   |`RewriteModule`              |[<span data-ttu-id="81d8d-158">URL 重写中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-158">URL Rewriting Middleware</span></span>](/aspnet/core/fundamentals/url-rewriting)|
|<span data-ttu-id="81d8d-159">静态压缩</span><span class="sxs-lookup"><span data-stu-id="81d8d-159">Static compression</span></span>     |`StaticCompressionModule`    |[<span data-ttu-id="81d8d-160">响应压缩中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-160">Response Compression Middleware</span></span>](/aspnet/core/performance/response-compression)|
|<span data-ttu-id="81d8d-161">静态内容</span><span class="sxs-lookup"><span data-stu-id="81d8d-161">Static content</span></span>         |`StaticFileModule`           |[<span data-ttu-id="81d8d-162">静态文件中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-162">Static File Middleware</span></span>](/aspnet/core/fundamentals/static-files)|
|<span data-ttu-id="81d8d-163">URL 授权</span><span class="sxs-lookup"><span data-stu-id="81d8d-163">URL authorization</span></span>      |`UrlAuthorizationModule`     |[<span data-ttu-id="81d8d-164">ASP.NET Core 标识</span><span class="sxs-lookup"><span data-stu-id="81d8d-164">ASP.NET Core Identity</span></span>](/aspnet/core/security/authentication/identity)|

<span data-ttu-id="81d8d-165">此列表并不详尽，但它应该能够提供两个框架之间存在哪些映射的概念。</span><span class="sxs-lookup"><span data-stu-id="81d8d-165">This list isn't exhaustive but should give an idea of what mapping exists between the two frameworks.</span></span> <span data-ttu-id="81d8d-166">若要获取更详细的列表，请参阅[使用 ASP.NET Core 的 IIS 模块](/aspnet/core/host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="81d8d-166">For a more detailed list, see [IIS modules with ASP.NET Core](/aspnet/core/host-and-deploy/iis/modules).</span></span>

## <a name="custom-middleware"></a><span data-ttu-id="81d8d-167">自定义中间件</span><span class="sxs-lookup"><span data-stu-id="81d8d-167">Custom middleware</span></span>

<span data-ttu-id="81d8d-168">内置中间件可能无法处理应用所需的所有场景。</span><span class="sxs-lookup"><span data-stu-id="81d8d-168">Built-in middleware may not handle all scenarios needed for an app.</span></span> <span data-ttu-id="81d8d-169">在这种情况下，创建自己的中间件非常有意义。</span><span class="sxs-lookup"><span data-stu-id="81d8d-169">In such cases, it makes sense to create your own middleware.</span></span> <span data-ttu-id="81d8d-170">定义中间件的方式有多种，最简单的方法是使用简单的委托。</span><span class="sxs-lookup"><span data-stu-id="81d8d-170">There are multiple ways of defining middleware, with the simplest being a simple delegate.</span></span> <span data-ttu-id="81d8d-171">请考虑以下中间件，它接受来自查询字符串的区域性请求：</span><span class="sxs-lookup"><span data-stu-id="81d8d-171">Consider the following middleware, which accepts a culture request from a query string:</span></span>

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

<span data-ttu-id="81d8d-172">还可以通过实现 `IMiddleware` 接口或按照中间件约定将中间件定义为类。</span><span class="sxs-lookup"><span data-stu-id="81d8d-172">Middleware can also be defined as class, either by implementing the `IMiddleware` interface or by following middleware convention.</span></span> <span data-ttu-id="81d8d-173">有关详细信息，请参阅[编写自定义 ASP.NET Core 中间件](/aspnet/core/fundamentals/middleware/write)。</span><span class="sxs-lookup"><span data-stu-id="81d8d-173">For more information, see [Write custom ASP.NET Core middleware](/aspnet/core/fundamentals/middleware/write).</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="81d8d-174">[上一页](data.md)
>[下一页](config.md)</span><span class="sxs-lookup"><span data-stu-id="81d8d-174">[Previous](data.md)
[Next](config.md)</span></span>
