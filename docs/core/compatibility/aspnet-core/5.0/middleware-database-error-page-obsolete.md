---
title: 中断性变更：中间件：数据库错误页标记为已过时
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：中间件：数据库错误页标记为已过时
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 8bc8852d7fb54b0d558342ee3c69085117512a1a
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497614"
---
# <a name="middleware-database-error-page-marked-as-obsolete"></a><span data-ttu-id="2aadb-103">中间件：数据库错误页标记为已过时</span><span class="sxs-lookup"><span data-stu-id="2aadb-103">Middleware: Database error page marked as obsolete</span></span>

<span data-ttu-id="2aadb-104">在 ASP.NET Core 5.0 中，将 [DatabaseErrorPageMiddleware](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0) 及其关联的扩展方法标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="2aadb-104">The [DatabaseErrorPageMiddleware](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0) and its associated extension methods were marked as obsolete in ASP.NET Core 5.0.</span></span> <span data-ttu-id="2aadb-105">将删除 ASP.NET Core 6.0 中的中间件和扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2aadb-105">The middleware and extension methods will be removed in ASP.NET Core 6.0.</span></span> <span data-ttu-id="2aadb-106">该功能将改为由 `DatabaseDeveloperPageExceptionFilter` 及其扩展方法提供。</span><span class="sxs-lookup"><span data-stu-id="2aadb-106">The functionality will instead be provided by `DatabaseDeveloperPageExceptionFilter` and its extension methods.</span></span>

<span data-ttu-id="2aadb-107">有关讨论，请参阅 [dotnet/aspnetcore#24987](https://github.com/dotnet/aspnetcore/issues/24987) 上的 GitHub 问题。</span><span class="sxs-lookup"><span data-stu-id="2aadb-107">For discussion, see the GitHub issue at [dotnet/aspnetcore#24987](https://github.com/dotnet/aspnetcore/issues/24987).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="2aadb-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="2aadb-108">Version introduced</span></span>

<span data-ttu-id="2aadb-109">5.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="2aadb-109">5.0 RC 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="2aadb-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="2aadb-110">Old behavior</span></span>

<span data-ttu-id="2aadb-111">`DatabaseErrorPageMiddleware` 及其关联的扩展方法未过时。</span><span class="sxs-lookup"><span data-stu-id="2aadb-111">`DatabaseErrorPageMiddleware` and its associated extension methods weren't obsolete.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="2aadb-112">新行为</span><span class="sxs-lookup"><span data-stu-id="2aadb-112">New behavior</span></span>

<span data-ttu-id="2aadb-113">`DatabaseErrorPageMiddleware` 及其关联的扩展方法已过时。</span><span class="sxs-lookup"><span data-stu-id="2aadb-113">`DatabaseErrorPageMiddleware` and its associated extension methods are obsolete.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="2aadb-114">更改原因</span><span class="sxs-lookup"><span data-stu-id="2aadb-114">Reason for change</span></span>

<span data-ttu-id="2aadb-115">`DatabaseErrorPageMiddleware` 已迁移到[开发人员异常页面](/aspnet/core/fundamentals/error-handling#developer-exception-page)的可扩展 API。</span><span class="sxs-lookup"><span data-stu-id="2aadb-115">`DatabaseErrorPageMiddleware` was migrated to an extensible API for the [developer exception page](/aspnet/core/fundamentals/error-handling#developer-exception-page).</span></span> <span data-ttu-id="2aadb-116">有关可扩展 API 的详细信息，请参阅 GitHub 问题 [dotnet/aspnetcore # 8536](https://github.com/dotnet/aspnetcore/issues/8536)。</span><span class="sxs-lookup"><span data-stu-id="2aadb-116">For more information on the extensible API, see GitHub issue [dotnet/aspnetcore#8536](https://github.com/dotnet/aspnetcore/issues/8536).</span></span>

## <a name="recommended-action"></a><span data-ttu-id="2aadb-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="2aadb-117">Recommended action</span></span>

<span data-ttu-id="2aadb-118">完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="2aadb-118">Complete the following steps:</span></span>

1. <span data-ttu-id="2aadb-119">停止在项目中使用 `DatabaseErrorPageMiddleware`。</span><span class="sxs-lookup"><span data-stu-id="2aadb-119">Stop using `DatabaseErrorPageMiddleware` in your project.</span></span> <span data-ttu-id="2aadb-120">例如，从 `Startup.Configure` 中删除 `UseDatabaseErrorPage` 方法调用：</span><span class="sxs-lookup"><span data-stu-id="2aadb-120">For example, remove the `UseDatabaseErrorPage` method call from `Startup.Configure`:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDatabaseErrorPage();
        }
    }
    ```

1. <span data-ttu-id="2aadb-121">将开发人员异常页面添加到你的项目。</span><span class="sxs-lookup"><span data-stu-id="2aadb-121">Add the developer exception page to your project.</span></span> <span data-ttu-id="2aadb-122">例如，在 `Startup.Configure` 中调用 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage%2A> 方法：</span><span class="sxs-lookup"><span data-stu-id="2aadb-122">For example, call the <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage%2A> method in `Startup.Configure`:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
    }
    ```

1. <span data-ttu-id="2aadb-123">向项目文件添加 [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="2aadb-123">Add the [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) NuGet package to the project file.</span></span>

1. <span data-ttu-id="2aadb-124">将数据库开发人员异常页面筛选器添加到服务集合。</span><span class="sxs-lookup"><span data-stu-id="2aadb-124">Add the database developer page exception filter to the services collection.</span></span> <span data-ttu-id="2aadb-125">例如，在 `Startup.ConfigureServices` 中调用 `AddDatabaseDeveloperPageExceptionFilter` 方法：</span><span class="sxs-lookup"><span data-stu-id="2aadb-125">For example, call the `AddDatabaseDeveloperPageExceptionFilter` method in `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDatabaseDeveloperPageExceptionFilter();
    }
    ```

## <a name="affected-apis"></a><span data-ttu-id="2aadb-126">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="2aadb-126">Affected APIs</span></span>

- [<span data-ttu-id="2aadb-127">Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage</span><span class="sxs-lookup"><span data-stu-id="2aadb-127">Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage</span></span>](/dotnet/api/microsoft.aspnetcore.builder.databaseerrorpageextensions.usedatabaseerrorpage?view=aspnetcore-3.0)
- [<span data-ttu-id="2aadb-128">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware</span><span class="sxs-lookup"><span data-stu-id="2aadb-128">Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware</span></span>](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0)

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`
- `T:Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware`

-->
