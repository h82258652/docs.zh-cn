---
title: 中断性变更：IIS：保留 UrlRewrite 中间件查询字符串
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：IIS：保留 UrlRewrite 中间件查询字符串
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 3439844f04c89059a027c1264401efc46cd0d57e
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497654"
---
# <a name="iis-urlrewrite-middleware-query-strings-are-preserved"></a><span data-ttu-id="8aaf9-103">IIS：保留 UrlRewrite 中间件查询字符串</span><span class="sxs-lookup"><span data-stu-id="8aaf9-103">IIS: UrlRewrite middleware query strings are preserved</span></span>

<span data-ttu-id="8aaf9-104">IIS UrlRewrite 中间件缺陷阻止了在重写规则中保留查询字符串。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-104">An IIS UrlRewrite middleware defect prevented the query string from being preserved in rewrite rules.</span></span> <span data-ttu-id="8aaf9-105">此缺陷已修复，以与 IIS UrlRewrite 模块的行为保持一致。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-105">That defect has been fixed to maintain consistency with the IIS UrlRewrite Module's behavior.</span></span>

<span data-ttu-id="8aaf9-106">有关讨论，请参阅问题 [dotnet/aspnetcore#22972](https://github.com/dotnet/aspnetcore/issues/22972)。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-106">For discussion, see issue [dotnet/aspnetcore#22972](https://github.com/dotnet/aspnetcore/issues/22972).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="8aaf9-107">引入的版本</span><span class="sxs-lookup"><span data-stu-id="8aaf9-107">Version introduced</span></span>

<span data-ttu-id="8aaf9-108">ASP.NET Core 5.0 预览版 7</span><span class="sxs-lookup"><span data-stu-id="8aaf9-108">ASP.NET Core 5.0 Preview 7</span></span>

## <a name="old-behavior"></a><span data-ttu-id="8aaf9-109">旧行为</span><span class="sxs-lookup"><span data-stu-id="8aaf9-109">Old behavior</span></span>

<span data-ttu-id="8aaf9-110">考虑以下重写规则：</span><span class="sxs-lookup"><span data-stu-id="8aaf9-110">Consider the following rewrite rule:</span></span>

```xml
<rule name="MyRule" stopProcessing="true">
  <match url="^about" />
  <action type="Redirect" url="/contact" redirectType="Temporary" appendQueryString="true" />
</rule>
```

<span data-ttu-id="8aaf9-111">前面的规则不追加查询字符串。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-111">The preceding rule doesn't append the query string.</span></span> <span data-ttu-id="8aaf9-112">URI（如 `/about?id=1`）会重定向到 `/contact` 而不是 `/contact?id=1`。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-112">A URI like `/about?id=1` redirects to `/contact` instead of `/contact?id=1`.</span></span> <span data-ttu-id="8aaf9-113">`appendQueryString` 属性也默认为 `true`。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-113">The `appendQueryString` attribute defaults to `true` as well.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="8aaf9-114">新行为</span><span class="sxs-lookup"><span data-stu-id="8aaf9-114">New behavior</span></span>

<span data-ttu-id="8aaf9-115">保留查询字符串。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-115">The query string is preserved.</span></span> <span data-ttu-id="8aaf9-116">[旧行为](#old-behavior)的示例中的 URI 将为 `/contact?id=1`。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-116">The URI from the example in [Old behavior](#old-behavior) would be `/contact?id=1`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="8aaf9-117">更改原因</span><span class="sxs-lookup"><span data-stu-id="8aaf9-117">Reason for change</span></span>

<span data-ttu-id="8aaf9-118">旧行为与 IIS UrlRewrite 模块的行为不匹配。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-118">The old behavior didn't match the IIS UrlRewrite Module's behavior.</span></span> <span data-ttu-id="8aaf9-119">若要支持在中间件与模块之间进行移植，应以保持一致的行为为目标。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-119">To support porting between the middleware and module, the goal is to maintain consistent behaviors.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="8aaf9-120">建议操作</span><span class="sxs-lookup"><span data-stu-id="8aaf9-120">Recommended action</span></span>

<span data-ttu-id="8aaf9-121">如果首选删除查询字符串的行为，请将 `action` 元素设置为 `appendQueryString="false"`。</span><span class="sxs-lookup"><span data-stu-id="8aaf9-121">If the behavior of removing the query string is preferred, set the `action` element to `appendQueryString="false"`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="8aaf9-122">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="8aaf9-122">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`Overload:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite`

-->
