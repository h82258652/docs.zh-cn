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
# <a name="azure-microsoft-prefixed-azure-integration-packages-removed"></a>Azure：Microsoft 预先指定的 Azure 集成包已删除

提供 ASP.NET Core 和 Azure SDK 之间的集成的以下 `Microsoft.*` 包未包含在 ASP.NET Core 5.0 中：

* [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)，它将 [Azure Key Vault](/azure/key-vault/) 集成到[配置系统](/aspnet/core/fundamentals/configuration/)中。
* [Microsoft.AspNetCore.DataProtection.AzureKeyVault](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureKeyVault/)，它将 Azure Key Vault 集成到 [ASP.NET Core 数据保护系统](/aspnet/core/security/data-protection/introduction)中。
* [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/)，它将 [Azure Blob 存储](/azure/storage/blobs/)集成到 ASP.NET Core 数据保护系统中。

有关此问题的讨论，请参阅 [dotnet/aspnetcore#19570](https://github.com/dotnet/aspnetcore/issues/19570)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 1

## <a name="old-behavior"></a>旧行为

`Microsoft.*` 包将 Azure 服务与配置 API 和数据保护 API 集成。

## <a name="new-behavior"></a>新行为

新的 `Azure.*` 包将 Azure 服务与配置 API 和数据保护 API 集成。

## <a name="reason-for-change"></a>更改原因

之所以更改，是因为 `Microsoft.*` 包：

* 使用过时的 Azure SDK 版本。 无法进行简单更新，因为新版本的 Azure SDK 包含重大更改。
* 与 .NET Core 发布计划相关。 将包的所有权转让给 Azure SDK 团队可以在更新 Azure SDK 时进行包更新。

## <a name="recommended-action"></a>建议操作

在 ASP.NET Core 2.1 或更高版本的项目中，用新的 `Azure.*` 包替换旧的 `Microsoft.*`。

| 旧 | 新建 |
|--|--|
| `Microsoft.AspNetCore.DataProtection.AzureKeyVault` | [Azure.Extensions.AspNetCore.DataProtection.Keys](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.DataProtection.Keys) |
| `Microsoft.AspNetCore.DataProtection.AzureStorage` | [Azure.Extensions.AspNetCore.DataProtection.Blobs](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.DataProtection.Blobs) |
| `Microsoft.Extensions.Configuration.AzureKeyVault` | [Azure.Extensions.AspNetCore.Configuration.Secrets](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.Configuration.Secrets) |

新包使用包含重大更改的新版 Azure SDK。 常规使用模式不变。 一些重载和选项可能有所不同，以适应基础 Azure SDK API 中的更改。

旧包将：

* ASP.NET Core 团队将在 .NET Core 2.1 和 3.1 的生命周期中为其提供支持。
* 不包含在 .NET 5 中。

将项目升级到 .NET 5 时，请转换为 `Azure.*` 包以保持支持。

## <a name="affected-apis"></a>受影响的 API

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
