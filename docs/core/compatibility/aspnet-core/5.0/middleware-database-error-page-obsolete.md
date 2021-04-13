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
# <a name="middleware-database-error-page-marked-as-obsolete"></a>中间件：数据库错误页标记为已过时

在 ASP.NET Core 5.0 中，将 [DatabaseErrorPageMiddleware](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0) 及其关联的扩展方法标记为已过时。 将删除 ASP.NET Core 6.0 中的中间件和扩展方法。 该功能将改为由 `DatabaseDeveloperPageExceptionFilter` 及其扩展方法提供。

有关讨论，请参阅 [dotnet/aspnetcore#24987](https://github.com/dotnet/aspnetcore/issues/24987) 上的 GitHub 问题。

## <a name="version-introduced"></a>引入的版本

5.0 RC 1

## <a name="old-behavior"></a>旧行为

`DatabaseErrorPageMiddleware` 及其关联的扩展方法未过时。

## <a name="new-behavior"></a>新行为

`DatabaseErrorPageMiddleware` 及其关联的扩展方法已过时。

## <a name="reason-for-change"></a>更改原因

`DatabaseErrorPageMiddleware` 已迁移到[开发人员异常页面](/aspnet/core/fundamentals/error-handling#developer-exception-page)的可扩展 API。 有关可扩展 API 的详细信息，请参阅 GitHub 问题 [dotnet/aspnetcore # 8536](https://github.com/dotnet/aspnetcore/issues/8536)。

## <a name="recommended-action"></a>建议操作

完成以下步骤：

1. 停止在项目中使用 `DatabaseErrorPageMiddleware`。 例如，从 `Startup.Configure` 中删除 `UseDatabaseErrorPage` 方法调用：

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDatabaseErrorPage();
        }
    }
    ```

1. 将开发人员异常页面添加到你的项目。 例如，在 `Startup.Configure` 中调用 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage%2A> 方法：

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
    }
    ```

1. 向项目文件添加 [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) NuGet 包。

1. 将数据库开发人员异常页面筛选器添加到服务集合。 例如，在 `Startup.ConfigureServices` 中调用 `AddDatabaseDeveloperPageExceptionFilter` 方法：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDatabaseDeveloperPageExceptionFilter();
    }
    ```

## <a name="affected-apis"></a>受影响的 API

- [Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage](/dotnet/api/microsoft.aspnetcore.builder.databaseerrorpageextensions.usedatabaseerrorpage?view=aspnetcore-3.0)
- [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware](/dotnet/api/microsoft.aspnetcore.diagnostics.entityframeworkcore.databaseerrorpagemiddleware?view=aspnetcore-3.0)

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`
- `T:Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore.DatabaseErrorPageMiddleware`

-->
