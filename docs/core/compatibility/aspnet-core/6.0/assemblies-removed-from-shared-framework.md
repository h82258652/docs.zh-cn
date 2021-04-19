---
title: 中断性变更：从 Microsoft.AspNetCore.App 共享框架中删除了程序集
description: 了解 ASP.NET Core 6.0 中的中断性变更：从 Microsoft.AspNetCore.App 共享框架中删除了一些程序集。
ms.date: 04/02/2021
ms.openlocfilehash: e6a0aa97dd97eae2cdc5aa8ae64e277840d6a083
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255254"
---
# <a name="assemblies-removed-from-microsoftaspnetcoreapp-shared-framework"></a><span data-ttu-id="c1ea1-103">从 Microsoft.AspNetCore.App 共享框架中删除了程序集</span><span class="sxs-lookup"><span data-stu-id="c1ea1-103">Assemblies removed from Microsoft.AspNetCore.App shared framework</span></span>

<span data-ttu-id="c1ea1-104">从 ASP.NET Core 目标包中删除了以下两个程序集：</span><span class="sxs-lookup"><span data-stu-id="c1ea1-104">The following two assemblies were removed from the ASP.NET Core targeting pack:</span></span>

- <span data-ttu-id="c1ea1-105">System.Security.Permissions</span><span class="sxs-lookup"><span data-stu-id="c1ea1-105">System.Security.Permissions</span></span>
- <span data-ttu-id="c1ea1-106">System.Windows.Extensions</span><span class="sxs-lookup"><span data-stu-id="c1ea1-106">System.Windows.Extensions</span></span>

<span data-ttu-id="c1ea1-107">此外，从 ASP.NET Core 运行时包中删除了以下程序集：</span><span class="sxs-lookup"><span data-stu-id="c1ea1-107">In addition, the following assemblies were removed from the ASP.NET Core runtime pack:</span></span>

- <span data-ttu-id="c1ea1-108">Microsoft.Win32.SystemEvents</span><span class="sxs-lookup"><span data-stu-id="c1ea1-108">Microsoft.Win32.SystemEvents</span></span>
- <span data-ttu-id="c1ea1-109">System.Drawing.Common</span><span class="sxs-lookup"><span data-stu-id="c1ea1-109">System.Drawing.Common</span></span>
- <span data-ttu-id="c1ea1-110">System.Security.Permissions</span><span class="sxs-lookup"><span data-stu-id="c1ea1-110">System.Security.Permissions</span></span>
- <span data-ttu-id="c1ea1-111">System.Windows.Extensions</span><span class="sxs-lookup"><span data-stu-id="c1ea1-111">System.Windows.Extensions</span></span>

## <a name="version-introduced"></a><span data-ttu-id="c1ea1-112">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c1ea1-112">Version introduced</span></span>

<span data-ttu-id="c1ea1-113">ASP.NET Core 6.0</span><span class="sxs-lookup"><span data-stu-id="c1ea1-113">ASP.NET Core 6.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="c1ea1-114">旧行为</span><span class="sxs-lookup"><span data-stu-id="c1ea1-114">Old behavior</span></span>

<span data-ttu-id="c1ea1-115">应用程序可通过引用 [Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app) 共享框架来使用这些库提供的 API。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-115">Applications could use APIs provided by these libraries by referencing the [Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app) shared framework.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="c1ea1-116">新行为</span><span class="sxs-lookup"><span data-stu-id="c1ea1-116">New behavior</span></span>

<span data-ttu-id="c1ea1-117">如果使用受影响的程序集中的 API，而项目文件中没有 [PackageReference](../../../project-sdk/msbuild-props.md#packagereference)，则可能会出现运行时错误。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-117">If you use APIs from the affected assemblies without having a [PackageReference](../../../project-sdk/msbuild-props.md#packagereference) in your project file, you might see run-time errors.</span></span> <span data-ttu-id="c1ea1-118">例如，如果某应用程序使用反射从其中某个程序集访问 API，但不添加对包的显式引用，则应用程序将出现运行时错误。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-118">For example, an application that uses reflection to access APIs from one of these assemblies without adding an explicit reference to the package will have run-time errors.</span></span> <span data-ttu-id="c1ea1-119">`PackageReference` 确保程序集作为应用程序输出的一部分而存在。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-119">The `PackageReference` ensures that the assemblies are present as part of the application output.</span></span>

<span data-ttu-id="c1ea1-120">有关讨论内容，请参阅 <https://github.com/dotnet/aspnetcore/issues/31007>。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-120">For discussion, see <https://github.com/dotnet/aspnetcore/issues/31007>.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="c1ea1-121">更改原因</span><span class="sxs-lookup"><span data-stu-id="c1ea1-121">Reason for change</span></span>

<span data-ttu-id="c1ea1-122">引入此更改是为了减小 ASP.NET Core 共享框架的大小。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-122">This change was introduced to reduce the size of the ASP.NET Core shared framework.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="c1ea1-123">建议操作</span><span class="sxs-lookup"><span data-stu-id="c1ea1-123">Recommended action</span></span>

<span data-ttu-id="c1ea1-124">要在项目中继续使用这些 API，请添加 [PackageReference](../../../project-sdk/msbuild-props.md#packagereference)。</span><span class="sxs-lookup"><span data-stu-id="c1ea1-124">To continue using these APIs in your project, add a [PackageReference](../../../project-sdk/msbuild-props.md#packagereference).</span></span> <span data-ttu-id="c1ea1-125">例如：</span><span class="sxs-lookup"><span data-stu-id="c1ea1-125">For example:</span></span>

```xml
<PackageReference Include="System.Security.Permissions" Version="6.0.0" />
```

## <a name="affected-apis"></a><span data-ttu-id="c1ea1-126">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="c1ea1-126">Affected APIs</span></span>

- <xref:System.Security.Permissions?displayProperty=fullName>
- <xref:System.Media?displayProperty=fullName>
- <xref:System.Security.Cryptography.X509Certificates.X509Certificate2UI?displayProperty=fullName>
- <xref:System.Xaml.Permissions.XamlAccessLevel?displayProperty=fullName>

<!--

## Category

ASP.NET Core

## Affected APIs

- `N:System.Security.Permissions`
- `N:System.Media`
- `N:System.Security.Cryptography.X509Certificates.X509Certificate2UI`
- `N:System.Xaml.Permissions.XamlAccessLevel`

-->
