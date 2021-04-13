---
title: 中断性变更：本地化：ResourceManagerWithCultureStringLocalizer 类和 WithCulture 接口成员已删除
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：本地化：ResourceManagerWithCultureStringLocalizer 类和 WithCulture 接口成员已删除
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 0c02a0e0cc78e4bfbf6f4a1ba5951cd13b56fe0c
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498082"
---
# <a name="localization-resourcemanagerwithculturestringlocalizer-class-and-withculture-interface-member-removed"></a><span data-ttu-id="6c443-103">本地化：ResourceManagerWithCultureStringLocalizer 类和 WithCulture 接口成员已删除</span><span class="sxs-lookup"><span data-stu-id="6c443-103">Localization: ResourceManagerWithCultureStringLocalizer class and WithCulture interface member removed</span></span>

<span data-ttu-id="6c443-104">.NET 5.0 预览版 1 删除了 [ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1) 类和 [WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1) 方法。</span><span class="sxs-lookup"><span data-stu-id="6c443-104">The [ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1) class and [WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1) method were removed in .NET 5.0 Preview 1.</span></span>

<span data-ttu-id="6c443-105">有关上下文，请参阅 [aspnet/Announcements#346](https://github.com/aspnet/Announcements/issues/346) 和 [dotnet/aspnetcore#3324](https://github.com/dotnet/aspnetcore/issues/3324)。</span><span class="sxs-lookup"><span data-stu-id="6c443-105">For context, see [aspnet/Announcements#346](https://github.com/aspnet/Announcements/issues/346) and [dotnet/aspnetcore#3324](https://github.com/dotnet/aspnetcore/issues/3324).</span></span> <span data-ttu-id="6c443-106">有关此更改的讨论，请参阅 [dotnet/aspnetcore#7756](https://github.com/dotnet/aspnetcore/issues/7756)。</span><span class="sxs-lookup"><span data-stu-id="6c443-106">For discussion on this change, see [dotnet/aspnetcore#7756](https://github.com/dotnet/aspnetcore/issues/7756).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="6c443-107">引入的版本</span><span class="sxs-lookup"><span data-stu-id="6c443-107">Version introduced</span></span>

<span data-ttu-id="6c443-108">5.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="6c443-108">5.0 Preview 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="6c443-109">旧行为</span><span class="sxs-lookup"><span data-stu-id="6c443-109">Old behavior</span></span>

<span data-ttu-id="6c443-110">`ResourceManagerWithCultureStringLocalizer` 类和 `ResourceManagerStringLocalizer.WithCulture` 方法[在 .NET Core 3.0 预览版 3 及更高版本中已过时](../../3.0.md#localization-resourcemanagerwithculturestringlocalizer-and-withculture-marked-obsolete)。</span><span class="sxs-lookup"><span data-stu-id="6c443-110">The `ResourceManagerWithCultureStringLocalizer` class and the `ResourceManagerStringLocalizer.WithCulture` method are [obsolete in .NET Core 3.0 Preview 3 and later](../../3.0.md#localization-resourcemanagerwithculturestringlocalizer-and-withculture-marked-obsolete).</span></span>

## <a name="new-behavior"></a><span data-ttu-id="6c443-111">新行为</span><span class="sxs-lookup"><span data-stu-id="6c443-111">New behavior</span></span>

<span data-ttu-id="6c443-112">.NET 5.0 预览版 1 中删除了 `ResourceManagerWithCultureStringLocalizer` 类和 `ResourceManagerStringLocalizer.WithCulture` 方法。</span><span class="sxs-lookup"><span data-stu-id="6c443-112">The `ResourceManagerWithCultureStringLocalizer` class and the `ResourceManagerStringLocalizer.WithCulture` method have been removed in .NET 5.0 Preview 1.</span></span> <span data-ttu-id="6c443-113">有关所做更改的清单，请参阅 [dotnet/extensions#2562](https://github.com/dotnet/extensions/pull/2562/files) 上的拉取请求。</span><span class="sxs-lookup"><span data-stu-id="6c443-113">For an inventory of the changes made, see the pull request at [dotnet/extensions#2562](https://github.com/dotnet/extensions/pull/2562/files).</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="6c443-114">更改原因</span><span class="sxs-lookup"><span data-stu-id="6c443-114">Reason for change</span></span>

<span data-ttu-id="6c443-115">[ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1) 类和 [ResourceManagerStringLocalizer.WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1) 方法通常会让本地化用户混淆。</span><span class="sxs-lookup"><span data-stu-id="6c443-115">The [ResourceManagerWithCultureStringLocalizer](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1) class and [ResourceManagerStringLocalizer.WithCulture](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1) method were often sources of confusion for users of localization.</span></span> <span data-ttu-id="6c443-116">创建自定义 <xref:Microsoft.Extensions.Localization.IStringLocalizer> 实现时，混淆尤其严重。</span><span class="sxs-lookup"><span data-stu-id="6c443-116">The confusion was especially high when creating a custom <xref:Microsoft.Extensions.Localization.IStringLocalizer> implementation.</span></span> <span data-ttu-id="6c443-117">这个类和方法给使用者的印象是 `IStringLocalizer` 实例应“按语言、按资源”。</span><span class="sxs-lookup"><span data-stu-id="6c443-117">This class and method give consumers the impression that an `IStringLocalizer` instance is expected to be "per-language, per-resource".</span></span> <span data-ttu-id="6c443-118">实际上，实例应该仅“按资源”。</span><span class="sxs-lookup"><span data-stu-id="6c443-118">In reality, the instance should only be "per-resource".</span></span> <span data-ttu-id="6c443-119">在运行时，<xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性确定要使用的语言。</span><span class="sxs-lookup"><span data-stu-id="6c443-119">At run time, the <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> property determines the language to be used.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="6c443-120">建议操作</span><span class="sxs-lookup"><span data-stu-id="6c443-120">Recommended action</span></span>

<span data-ttu-id="6c443-121">停止使用 `ResourceManagerWithCultureStringLocalizer` 类和 `ResourceManagerStringLocalizer.WithCulture` 方法。</span><span class="sxs-lookup"><span data-stu-id="6c443-121">Stop using the `ResourceManagerWithCultureStringLocalizer` class and the `ResourceManagerStringLocalizer.WithCulture` method.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="6c443-122">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="6c443-122">Affected APIs</span></span>

- [<span data-ttu-id="6c443-123">ResourceManagerWithCultureStringLocalizer</span><span class="sxs-lookup"><span data-stu-id="6c443-123">ResourceManagerWithCultureStringLocalizer</span></span>](/dotnet/api/microsoft.extensions.localization.resourcemanagerwithculturestringlocalizer?view=dotnet-plat-ext-3.1)
- [<span data-ttu-id="6c443-124">ResourceManagerStringLocalizer.WithCulture</span><span class="sxs-lookup"><span data-stu-id="6c443-124">ResourceManagerStringLocalizer.WithCulture</span></span>](/dotnet/api/microsoft.extensions.localization.resourcemanagerstringlocalizer.withculture?view=dotnet-plat-ext-3.1)

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.Extensions.Localization.ResourceManagerWithCultureStringLocalizer`
- `Overload:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.WithCulture`

-->
