---
title: 中断性变更：扩展：影响某些 NuGet 包的包引用更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：扩展：影响某些 NuGet 包的包引用更改
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 82ee5129d6422d0a5ea4a0f14632a42e779f6ded
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497731"
---
# <a name="extensions-package-reference-changes-affecting-some-nuget-packages"></a><span data-ttu-id="f9370-103">扩展：影响某些 NuGet 包的包引用更改</span><span class="sxs-lookup"><span data-stu-id="f9370-103">Extensions: Package reference changes affecting some NuGet packages</span></span>

<span data-ttu-id="f9370-104">如 [aspnet/Announcements#411](https://github.com/aspnet/Announcements/issues/411) 中所述，通过将某些 `Microsoft.Extensions.*` NuGet 包从 [dotnet/extensions](https://github.com/dotnet/extensions) 存储库迁移到 [dotnet/runtime](https://github.com/dotnet/runtime)，可将打包更改应用于某些已迁移的包。</span><span class="sxs-lookup"><span data-stu-id="f9370-104">With the migration of some `Microsoft.Extensions.*` NuGet packages from the [dotnet/extensions](https://github.com/dotnet/extensions) repository to [dotnet/runtime](https://github.com/dotnet/runtime), as described in [aspnet/Announcements#411](https://github.com/aspnet/Announcements/issues/411), packaging changes are being applied to some of the migrated packages.</span></span> <span data-ttu-id="f9370-105">有关此问题的讨论，请参阅 [dotnet/aspnetcore#21033](https://github.com/dotnet/aspnetcore/issues/21033)。</span><span class="sxs-lookup"><span data-stu-id="f9370-105">For discussion on this issue, see [dotnet/aspnetcore#21033](https://github.com/dotnet/aspnetcore/issues/21033).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="f9370-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="f9370-106">Version introduced</span></span>

<span data-ttu-id="f9370-107">5.0 预览版 4</span><span class="sxs-lookup"><span data-stu-id="f9370-107">5.0 Preview 4</span></span>

## <a name="old-behavior"></a><span data-ttu-id="f9370-108">旧行为</span><span class="sxs-lookup"><span data-stu-id="f9370-108">Old behavior</span></span>

<span data-ttu-id="f9370-109">某些 `Microsoft.Extensions.*` 包包含应用依赖的 API 的包引用。</span><span class="sxs-lookup"><span data-stu-id="f9370-109">Some `Microsoft.Extensions.*` packages included package references for APIs on which your app relied.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="f9370-110">新行为</span><span class="sxs-lookup"><span data-stu-id="f9370-110">New behavior</span></span>

<span data-ttu-id="f9370-111">你的应用可能必须添加 `Microsoft.Extensions.*` 包依赖项。</span><span class="sxs-lookup"><span data-stu-id="f9370-111">Your app may have to add `Microsoft.Extensions.*` package dependencies.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="f9370-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="f9370-112">Reason for change</span></span>

<span data-ttu-id="f9370-113">打包策略已更新，以更好地与 *dotnet/runtime* 存储库保持一致。</span><span class="sxs-lookup"><span data-stu-id="f9370-113">Packaging policies were updated to better align with the *dotnet/runtime* repository.</span></span> <span data-ttu-id="f9370-114">如果采用新策略，在打包过程中，未使用的包引用将从 .nupkg 文件中删除  。</span><span class="sxs-lookup"><span data-stu-id="f9370-114">Under the new policy, unused package references are removed from *.nupkg* files during packaging.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="f9370-115">建议操作</span><span class="sxs-lookup"><span data-stu-id="f9370-115">Recommended action</span></span>

<span data-ttu-id="f9370-116">如果使用删除的包依赖项中的 API，受影响的包的使用者应在项目中添加对删除的包依赖项的直接依赖项。</span><span class="sxs-lookup"><span data-stu-id="f9370-116">Consumers of the affected packages should add a direct dependency on the removed package dependency in their project if APIs from removed package dependency are used.</span></span> <span data-ttu-id="f9370-117">下表列出了受影响的包和相应的更改。</span><span class="sxs-lookup"><span data-stu-id="f9370-117">The following table lists the affected packages and the corresponding changes.</span></span>

|<span data-ttu-id="f9370-118">包名称</span><span class="sxs-lookup"><span data-stu-id="f9370-118">Package name</span></span>|<span data-ttu-id="f9370-119">更改描述</span><span class="sxs-lookup"><span data-stu-id="f9370-119">Change description</span></span>|
|------------|------------------|
|[<span data-ttu-id="f9370-120">Microsoft.Extensions.Configuration.Binder</span><span class="sxs-lookup"><span data-stu-id="f9370-120">Microsoft.Extensions.Configuration.Binder</span></span>](https://nuget.org/packages/Microsoft.Extensions.Configuration.Binder)|<span data-ttu-id="f9370-121">已删除对 `Microsoft.Extensions.Configuration` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-121">Removed reference to `Microsoft.Extensions.Configuration`</span></span>|
|[<span data-ttu-id="f9370-122">Microsoft.Extensions.Configuration.Json</span><span class="sxs-lookup"><span data-stu-id="f9370-122">Microsoft.Extensions.Configuration.Json</span></span>](https://nuget.org/packages/Microsoft.Extensions.Configuration.Json)    |<span data-ttu-id="f9370-123">已删除对 `System.Threading.Tasks.Extensions` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-123">Removed reference to `System.Threading.Tasks.Extensions`</span></span>|
|[<span data-ttu-id="f9370-124">Microsoft.Extensions.Hosting.Abstractions</span><span class="sxs-lookup"><span data-stu-id="f9370-124">Microsoft.Extensions.Hosting.Abstractions</span></span>](https://nuget.org/packages/Microsoft.Extensions.Hosting.Abstractions)|<span data-ttu-id="f9370-125">已删除对 `Microsoft.Extensions.Logging.Abstractions` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-125">Removed reference to `Microsoft.Extensions.Logging.Abstractions`</span></span>|
|[<span data-ttu-id="f9370-126">Microsoft.Extensions.Logging</span><span class="sxs-lookup"><span data-stu-id="f9370-126">Microsoft.Extensions.Logging</span></span>](https://nuget.org/packages/Microsoft.Extensions.Logging)                          |<span data-ttu-id="f9370-127">已删除对 `Microsoft.Extensions.Configuration.Binder` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-127">Removed reference to `Microsoft.Extensions.Configuration.Binder`</span></span>|
|[<span data-ttu-id="f9370-128">Microsoft.Extensions.Logging.Console</span><span class="sxs-lookup"><span data-stu-id="f9370-128">Microsoft.Extensions.Logging.Console</span></span>](https://nuget.org/packages/Microsoft.Extensions.Logging.Console)          |<span data-ttu-id="f9370-129">已删除对 `Microsoft.Extensions.Configuration.Abstractions` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-129">Removed reference to `Microsoft.Extensions.Configuration.Abstractions`</span></span>|
|[<span data-ttu-id="f9370-130">Microsoft.Extensions.Logging.EventLog</span><span class="sxs-lookup"><span data-stu-id="f9370-130">Microsoft.Extensions.Logging.EventLog</span></span>](https://nuget.org/packages/Microsoft.Extensions.Logging.EventLog)        |<span data-ttu-id="f9370-131">已删除对 .NET Framework 4.6.1 目标框架名字对象的 `System.Diagnostics.EventLog` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-131">Removed reference to `System.Diagnostics.EventLog` for the .NET Framework 4.6.1 target framework moniker</span></span>|
|[<span data-ttu-id="f9370-132">Microsoft.Extensions.Logging.EventSource</span><span class="sxs-lookup"><span data-stu-id="f9370-132">Microsoft.Extensions.Logging.EventSource</span></span>](https://nuget.org/packages/Microsoft.Extensions.Logging.EventSource)  |<span data-ttu-id="f9370-133">已删除对 `System.Threading.Tasks.Extensions` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-133">Removed reference to `System.Threading.Tasks.Extensions`</span></span>|
|[<span data-ttu-id="f9370-134">Microsoft.Extensions.Options</span><span class="sxs-lookup"><span data-stu-id="f9370-134">Microsoft.Extensions.Options</span></span>](https://nuget.org/packages/Microsoft.Extensions.Options)                          |<span data-ttu-id="f9370-135">已删除对 `System.ComponentModel.Annotations` 的引用</span><span class="sxs-lookup"><span data-stu-id="f9370-135">Removed reference to `System.ComponentModel.Annotations`</span></span>|

<span data-ttu-id="f9370-136">例如，对 `Microsoft.Extensions.Configuration` 的包引用已从 `Microsoft.Extensions.Configuration.Binder` 中删除。</span><span class="sxs-lookup"><span data-stu-id="f9370-136">For example, the package reference to `Microsoft.Extensions.Configuration` was removed from `Microsoft.Extensions.Configuration.Binder`.</span></span> <span data-ttu-id="f9370-137">包中未使用依赖项的 API。</span><span class="sxs-lookup"><span data-stu-id="f9370-137">No API from the dependency was used in the package.</span></span> <span data-ttu-id="f9370-138">依赖于 `Microsoft.Extensions.Configuration` 中 API 的 `Microsoft.Extensions.Configuration.Binder` 用户应该在项目中添加对它的直接引用。</span><span class="sxs-lookup"><span data-stu-id="f9370-138">Users of `Microsoft.Extensions.Configuration.Binder` who depend on APIs from `Microsoft.Extensions.Configuration` should add a direct reference to it in their project.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="f9370-139">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="f9370-139">Affected APIs</span></span>

<span data-ttu-id="f9370-140">无</span><span class="sxs-lookup"><span data-stu-id="f9370-140">None</span></span>

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
