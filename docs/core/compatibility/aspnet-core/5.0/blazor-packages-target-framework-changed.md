---
title: 中断性变更：Blazor：NuGet 包的目标框架已更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：NuGet 包的目标框架已更改
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 02945c223410860c9336b6046afe54cacce878dd
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498186"
---
# <a name="blazor-target-framework-of-nuget-packages-changed"></a><span data-ttu-id="7fe4f-103">Blazor：NuGet 包的目标框架已更改</span><span class="sxs-lookup"><span data-stu-id="7fe4f-103">Blazor: Target framework of NuGet packages changed</span></span>

<span data-ttu-id="7fe4f-104">Blazor 3.2 WebAssembly 项目编译为面向 .NET Standard 2.1 (`<TargetFramework>netstandard2.1</TargetFramework>`)。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-104">Blazor 3.2 WebAssembly projects were compiled to target .NET Standard 2.1 (`<TargetFramework>netstandard2.1</TargetFramework>`).</span></span> <span data-ttu-id="7fe4f-105">在 ASP.NET Core 5.0 中，Blazor Server 和 Blazor WebAssembly 项目都面向 .NET 5.0 (`<TargetFramework>net5.0</TargetFramework>`)。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-105">In ASP.NET Core 5.0, both Blazor Server and Blazor WebAssembly projects target .NET 5.0 (`<TargetFramework>net5.0</TargetFramework>`).</span></span> <span data-ttu-id="7fe4f-106">为了更好地适应目标框架更改，以下 Blazor 包不再面向 .NET Standard 2.1：</span><span class="sxs-lookup"><span data-stu-id="7fe4f-106">To better align with the target framework change, the following Blazor packages no longer target .NET Standard 2.1:</span></span>

* [<span data-ttu-id="7fe4f-107">Microsoft.AspNetCore.Components</span><span class="sxs-lookup"><span data-stu-id="7fe4f-107">Microsoft.AspNetCore.Components</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
* [<span data-ttu-id="7fe4f-108">Microsoft.AspNetCore.Components.Authorization</span><span class="sxs-lookup"><span data-stu-id="7fe4f-108">Microsoft.AspNetCore.Components.Authorization</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization)
* [<span data-ttu-id="7fe4f-109">Microsoft.AspNetCore.Components.Forms</span><span class="sxs-lookup"><span data-stu-id="7fe4f-109">Microsoft.AspNetCore.Components.Forms</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Forms)
* [<span data-ttu-id="7fe4f-110">Microsoft.AspNetCore.Components.Web</span><span class="sxs-lookup"><span data-stu-id="7fe4f-110">Microsoft.AspNetCore.Components.Web</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)
* [<span data-ttu-id="7fe4f-111">Microsoft.AspNetCore.Components.WebAssembly</span><span class="sxs-lookup"><span data-stu-id="7fe4f-111">Microsoft.AspNetCore.Components.WebAssembly</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly)
* [<span data-ttu-id="7fe4f-112">Microsoft.AspNetCore.Components.WebAssembly.Authentication</span><span class="sxs-lookup"><span data-stu-id="7fe4f-112">Microsoft.AspNetCore.Components.WebAssembly.Authentication</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication)
* [<span data-ttu-id="7fe4f-113">Microsoft.JSInterop</span><span class="sxs-lookup"><span data-stu-id="7fe4f-113">Microsoft.JSInterop</span></span>](https://www.nuget.org/packages/Microsoft.JSInterop)
* [<span data-ttu-id="7fe4f-114">Microsoft.JSInterop.WebAssembly</span><span class="sxs-lookup"><span data-stu-id="7fe4f-114">Microsoft.JSInterop.WebAssembly</span></span>](https://www.nuget.org/packages/Microsoft.JSInterop.WebAssembly)
* [<span data-ttu-id="7fe4f-115">Microsoft.Authentication.WebAssembly.Msal</span><span class="sxs-lookup"><span data-stu-id="7fe4f-115">Microsoft.Authentication.WebAssembly.Msal</span></span>](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)

<span data-ttu-id="7fe4f-116">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#23424](https://github.com/dotnet/aspnetcore/issues/23424)。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-116">For discussion, see GitHub issue [dotnet/aspnetcore#23424](https://github.com/dotnet/aspnetcore/issues/23424).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="7fe4f-117">引入的版本</span><span class="sxs-lookup"><span data-stu-id="7fe4f-117">Version introduced</span></span>

<span data-ttu-id="7fe4f-118">5.0 预览版 7</span><span class="sxs-lookup"><span data-stu-id="7fe4f-118">5.0 Preview 7</span></span>

## <a name="old-behavior"></a><span data-ttu-id="7fe4f-119">旧行为</span><span class="sxs-lookup"><span data-stu-id="7fe4f-119">Old behavior</span></span>

<span data-ttu-id="7fe4f-120">在 Blazor 3.1 和 3.2 中，包面向 .NET Standard 2.1 和 .NET Core 3.1。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-120">In Blazor 3.1 and 3.2, packages target .NET Standard 2.1 and .NET Core 3.1.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="7fe4f-121">新行为</span><span class="sxs-lookup"><span data-stu-id="7fe4f-121">New behavior</span></span>

<span data-ttu-id="7fe4f-122">在 ASP.NET Core 5.0 中，包面向 .NET 5.0。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-122">In ASP.NET Core 5.0, packages target .NET 5.0.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="7fe4f-123">更改原因</span><span class="sxs-lookup"><span data-stu-id="7fe4f-123">Reason for change</span></span>

<span data-ttu-id="7fe4f-124">进行此更改是为了更好地符合 .NET 目标框架要求。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-124">The change was made to better align with .NET target framework requirements.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="7fe4f-125">建议操作</span><span class="sxs-lookup"><span data-stu-id="7fe4f-125">Recommended action</span></span>

<span data-ttu-id="7fe4f-126">Blazor 3.2 WebAssembly 项目应在将其包引用更新为 5.x.x 的过程中以 .NET 5.0 为目标。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-126">Blazor 3.2 WebAssembly projects should target .NET 5.0 as part of updating their package references to 5.x.x.</span></span> <span data-ttu-id="7fe4f-127">引用其中一个包的库可以根据这些包的要求面向 .NET 5.0 或多个目标。</span><span class="sxs-lookup"><span data-stu-id="7fe4f-127">Libraries that reference one of these packages can either target .NET 5.0 or multi-target depending on their requirements.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="7fe4f-128">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="7fe4f-128">Affected APIs</span></span>

<span data-ttu-id="7fe4f-129">无</span><span class="sxs-lookup"><span data-stu-id="7fe4f-129">None</span></span>

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
