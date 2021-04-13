---
title: 中断性变更：静态文件：CSV 内容类型已更改为符合标准
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Static 静态文件：CSV 内容类型已更改为符合标准
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 3a7a1b2fc94f5cc956d85fc93ae72362e2e89b0e
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497081"
---
# <a name="static-files-csv-content-type-changed-to-standards-compliant"></a><span data-ttu-id="ab630-103">静态文件：CSV 内容类型已更改为符合标准</span><span class="sxs-lookup"><span data-stu-id="ab630-103">Static files: CSV content type changed to standards-compliant</span></span>

<span data-ttu-id="ab630-104">在 ASP.NET Core 5.0 中，[静态文件中间件](/aspnet/core/fundamentals/static-files)用于 .csv  文件的默认 `Content-Type` 响应标头值已更改为符合标准的值 `text/csv`。</span><span class="sxs-lookup"><span data-stu-id="ab630-104">In ASP.NET Core 5.0, the default `Content-Type` response header value that the [Static File Middleware](/aspnet/core/fundamentals/static-files) uses for *.csv* files has changed to the standards-compliant value `text/csv`.</span></span>

<span data-ttu-id="ab630-105">有关此问题的讨论，请参阅 [dotnet/aspnetcore#17385](https://github.com/dotnet/AspNetCore/issues/17385)。</span><span class="sxs-lookup"><span data-stu-id="ab630-105">For discussion on this issue, see [dotnet/aspnetcore#17385](https://github.com/dotnet/AspNetCore/issues/17385).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="ab630-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ab630-106">Version introduced</span></span>

<span data-ttu-id="ab630-107">5.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="ab630-107">5.0 Preview 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="ab630-108">旧行为</span><span class="sxs-lookup"><span data-stu-id="ab630-108">Old behavior</span></span>

<span data-ttu-id="ab630-109">使用 `Content-Type` 标头值 `application/octet-stream`。</span><span class="sxs-lookup"><span data-stu-id="ab630-109">The `Content-Type` header value `application/octet-stream` was used.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="ab630-110">新行为</span><span class="sxs-lookup"><span data-stu-id="ab630-110">New behavior</span></span>

<span data-ttu-id="ab630-111">使用 `Content-Type` 标头值 `text/csv`。</span><span class="sxs-lookup"><span data-stu-id="ab630-111">The `Content-Type` header value `text/csv` is used.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="ab630-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="ab630-112">Reason for change</span></span>

<span data-ttu-id="ab630-113">符合 [RFC 7111](https://tools.ietf.org/html/rfc7111#section-5.1) 标准。</span><span class="sxs-lookup"><span data-stu-id="ab630-113">Compliance with the [RFC 7111](https://tools.ietf.org/html/rfc7111#section-5.1) standard.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="ab630-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="ab630-114">Recommended action</span></span>

<span data-ttu-id="ab630-115">如果此更改影响你的应用，则可以自定义文件扩展名到 MIME 类型的映射。</span><span class="sxs-lookup"><span data-stu-id="ab630-115">If this change impacts your app, you can customize the file extension-to-MIME type mapping.</span></span> <span data-ttu-id="ab630-116">若要还原到 `application/octet-stream` MIME 类型，请在 `Startup.Configure` 中修改 <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A> 方法调用。</span><span class="sxs-lookup"><span data-stu-id="ab630-116">To revert to the `application/octet-stream` MIME type, modify the <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A> method call in `Startup.Configure`.</span></span> <span data-ttu-id="ab630-117">例如：</span><span class="sxs-lookup"><span data-stu-id="ab630-117">For example:</span></span>

```csharp
var provider = new FileExtensionContentTypeProvider();
provider.Mappings[".csv"] = MediaTypeNames.Application.Octet;

app.UseStaticFiles(new StaticFileOptions
{
    ContentTypeProvider = provider
});
```

<span data-ttu-id="ab630-118">有关自定义映射的详细信息，请参阅 [FileExtensionContentTypeProvider](/aspnet/core/fundamentals/static-files#fileextensioncontenttypeprovider)。</span><span class="sxs-lookup"><span data-stu-id="ab630-118">For more information on customizing the mapping, see [FileExtensionContentTypeProvider](/aspnet/core/fundamentals/static-files#fileextensioncontenttypeprovider).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="ab630-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ab630-119">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.StaticFiles.FileExtensionContentTypeProvider?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:Microsoft.AspNetCore.StaticFiles.FileExtensionContentTypeProvider`

-->
