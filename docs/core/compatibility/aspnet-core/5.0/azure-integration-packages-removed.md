---
title: 中断性变更：Azure：Microsoft 预先指定的 Azure 集成包已删除
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Azure：Microsoft 预先指定的 Azure 集成包已删除
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: be7b2eeb6b12d033517c15ecb29e0d5364d64817
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498225"
---
# <a name="azure-microsoft-prefixed-azure-integration-packages-removed"></a><span data-ttu-id="95f54-103">Azure：Microsoft 预先指定的 Azure 集成包已删除</span><span class="sxs-lookup"><span data-stu-id="95f54-103">Azure: Microsoft-prefixed Azure integration packages removed</span></span>

<span data-ttu-id="95f54-104">提供 ASP.NET Core 和 Azure SDK 之间的集成的以下 `Microsoft.*` 包未包含在 ASP.NET Core 5.0 中：</span><span class="sxs-lookup"><span data-stu-id="95f54-104">The following `Microsoft.*` packages that provide integration between ASP.NET Core and Azure SDKs aren't included in ASP.NET Core 5.0:</span></span>

* <span data-ttu-id="95f54-105">[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)，它将 [Azure Key Vault](/azure/key-vault/) 集成到[配置系统](/aspnet/core/fundamentals/configuration/)中。</span><span class="sxs-lookup"><span data-stu-id="95f54-105">[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/), which integrates [Azure Key Vault](/azure/key-vault/) into the [Configuration system](/aspnet/core/fundamentals/configuration/).</span></span>
* <span data-ttu-id="95f54-106">[Microsoft.AspNetCore.DataProtection.AzureKeyVault](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureKeyVault/)，它将 Azure Key Vault 集成到 [ASP.NET Core 数据保护系统](/aspnet/core/security/data-protection/introduction)中。</span><span class="sxs-lookup"><span data-stu-id="95f54-106">[Microsoft.AspNetCore.DataProtection.AzureKeyVault](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureKeyVault/), which integrates Azure Key Vault into the [ASP.NET Core Data Protection system](/aspnet/core/security/data-protection/introduction).</span></span>
* <span data-ttu-id="95f54-107">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)，它将 [Azure Blob 存储](/azure/storage/blobs/)集成到 ASP.NET Core 数据保护系统中。</span><span class="sxs-lookup"><span data-stu-id="95f54-107">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/), which integrates [Azure Blob Storage](/azure/storage/blobs/) into the ASP.NET Core Data Protection system.</span></span>

<span data-ttu-id="95f54-108">有关此问题的讨论，请参阅 [dotnet/aspnetcore#19570](https://github.com/dotnet/aspnetcore/issues/19570)。</span><span class="sxs-lookup"><span data-stu-id="95f54-108">For discussion on this issue, see [dotnet/aspnetcore#19570](https://github.com/dotnet/aspnetcore/issues/19570).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="95f54-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="95f54-109">Version introduced</span></span>

<span data-ttu-id="95f54-110">5.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="95f54-110">5.0 Preview 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="95f54-111">旧行为</span><span class="sxs-lookup"><span data-stu-id="95f54-111">Old behavior</span></span>

<span data-ttu-id="95f54-112">`Microsoft.*` 包将 Azure 服务与配置 API 和数据保护 API 集成。</span><span class="sxs-lookup"><span data-stu-id="95f54-112">The `Microsoft.*` packages integrated Azure services with the Configuration and Data Protection APIs.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="95f54-113">新行为</span><span class="sxs-lookup"><span data-stu-id="95f54-113">New behavior</span></span>

<span data-ttu-id="95f54-114">新的 `Azure.*` 包将 Azure 服务与配置 API 和数据保护 API 集成。</span><span class="sxs-lookup"><span data-stu-id="95f54-114">New `Azure.*` packages integrate Azure services with the Configuration and Data Protection APIs.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="95f54-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="95f54-115">Reason for change</span></span>

<span data-ttu-id="95f54-116">之所以更改，是因为 `Microsoft.*` 包：</span><span class="sxs-lookup"><span data-stu-id="95f54-116">The change was made because the `Microsoft.*` packages were:</span></span>

* <span data-ttu-id="95f54-117">使用过时的 Azure SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="95f54-117">Using outdated versions of the Azure SDK.</span></span> <span data-ttu-id="95f54-118">无法进行简单更新，因为新版本的 Azure SDK 包含重大更改。</span><span class="sxs-lookup"><span data-stu-id="95f54-118">Simple updates weren't possible because the new versions of the Azure SDK included breaking changes.</span></span>
* <span data-ttu-id="95f54-119">与 .NET Core 发布计划相关。</span><span class="sxs-lookup"><span data-stu-id="95f54-119">Tied to the .NET Core release schedule.</span></span> <span data-ttu-id="95f54-120">将包的所有权转让给 Azure SDK 团队可以在更新 Azure SDK 时进行包更新。</span><span class="sxs-lookup"><span data-stu-id="95f54-120">Transferring ownership of the packages to the Azure SDK team enables package updates as the Azure SDK is updated.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="95f54-121">建议操作</span><span class="sxs-lookup"><span data-stu-id="95f54-121">Recommended action</span></span>

<span data-ttu-id="95f54-122">在 ASP.NET Core 2.1 或更高版本的项目中，用新的 `Azure.*` 包替换旧的 `Microsoft.*`。</span><span class="sxs-lookup"><span data-stu-id="95f54-122">In ASP.NET Core 2.1 or later projects, replace the old `Microsoft.*` with the new `Azure.*` packages.</span></span>

| <span data-ttu-id="95f54-123">旧</span><span class="sxs-lookup"><span data-stu-id="95f54-123">Old</span></span> | <span data-ttu-id="95f54-124">新建</span><span class="sxs-lookup"><span data-stu-id="95f54-124">New</span></span> |
|--|--|
| `Microsoft.AspNetCore.DataProtection.AzureKeyVault` | [<span data-ttu-id="95f54-125">Azure.Extensions.AspNetCore.DataProtection.Keys</span><span class="sxs-lookup"><span data-stu-id="95f54-125">Azure.Extensions.AspNetCore.DataProtection.Keys</span></span>](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.DataProtection.Keys) |
| `Microsoft.AspNetCore.DataProtection.AzureStorage` | [<span data-ttu-id="95f54-126">Azure.Extensions.AspNetCore.DataProtection.Blobs</span><span class="sxs-lookup"><span data-stu-id="95f54-126">Azure.Extensions.AspNetCore.DataProtection.Blobs</span></span>](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.DataProtection.Blobs) |
| `Microsoft.Extensions.Configuration.AzureKeyVault` | [<span data-ttu-id="95f54-127">Azure.Extensions.AspNetCore.Configuration.Secrets</span><span class="sxs-lookup"><span data-stu-id="95f54-127">Azure.Extensions.AspNetCore.Configuration.Secrets</span></span>](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.Configuration.Secrets) |

<span data-ttu-id="95f54-128">新包使用包含重大更改的新版 Azure SDK。</span><span class="sxs-lookup"><span data-stu-id="95f54-128">The new packages use a new version of the Azure SDK that includes breaking changes.</span></span> <span data-ttu-id="95f54-129">常规使用模式不变。</span><span class="sxs-lookup"><span data-stu-id="95f54-129">The general usage patterns are unchanged.</span></span> <span data-ttu-id="95f54-130">一些重载和选项可能有所不同，以适应基础 Azure SDK API 中的更改。</span><span class="sxs-lookup"><span data-stu-id="95f54-130">Some overloads and options may differ to adapt to changes in the underlying Azure SDK APIs.</span></span>

<span data-ttu-id="95f54-131">旧包将：</span><span class="sxs-lookup"><span data-stu-id="95f54-131">The old packages will:</span></span>

* <span data-ttu-id="95f54-132">ASP.NET Core 团队将在 .NET Core 2.1 和 3.1 的生命周期中为其提供支持。</span><span class="sxs-lookup"><span data-stu-id="95f54-132">Be supported by the ASP.NET Core team for the lifetime of .NET Core 2.1 and 3.1.</span></span>
* <span data-ttu-id="95f54-133">不包含在 .NET 5 中。</span><span class="sxs-lookup"><span data-stu-id="95f54-133">Not be included in .NET 5.</span></span>

<span data-ttu-id="95f54-134">将项目升级到 .NET 5 时，请转换为 `Azure.*` 包以保持支持。</span><span class="sxs-lookup"><span data-stu-id="95f54-134">When upgrading your project to .NET 5, transition to the `Azure.*` packages to maintain support.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="95f54-135">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="95f54-135">Affected APIs</span></span>

- <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.ProtectKeysWithAzureKeyVault%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.PersistKeysToAzureBlobStorage%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault`
- `Overload:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.ProtectKeysWithAzureKeyVault`
- `Overload:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.PersistKeysToAzureBlobStorage`

-->
