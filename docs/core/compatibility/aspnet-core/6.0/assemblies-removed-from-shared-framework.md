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
# <a name="assemblies-removed-from-microsoftaspnetcoreapp-shared-framework"></a>从 Microsoft.AspNetCore.App 共享框架中删除了程序集

从 ASP.NET Core 目标包中删除了以下两个程序集：

- System.Security.Permissions
- System.Windows.Extensions

此外，从 ASP.NET Core 运行时包中删除了以下程序集：

- Microsoft.Win32.SystemEvents
- System.Drawing.Common
- System.Security.Permissions
- System.Windows.Extensions

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 6.0

## <a name="old-behavior"></a>旧行为

应用程序可通过引用 [Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app) 共享框架来使用这些库提供的 API。

## <a name="new-behavior"></a>新行为

如果使用受影响的程序集中的 API，而项目文件中没有 [PackageReference](../../../project-sdk/msbuild-props.md#packagereference)，则可能会出现运行时错误。 例如，如果某应用程序使用反射从其中某个程序集访问 API，但不添加对包的显式引用，则应用程序将出现运行时错误。 `PackageReference` 确保程序集作为应用程序输出的一部分而存在。

有关讨论内容，请参阅 <https://github.com/dotnet/aspnetcore/issues/31007>。

## <a name="reason-for-change"></a>更改原因

引入此更改是为了减小 ASP.NET Core 共享框架的大小。

## <a name="recommended-action"></a>建议操作

要在项目中继续使用这些 API，请添加 [PackageReference](../../../project-sdk/msbuild-props.md#packagereference)。 例如：

```xml
<PackageReference Include="System.Security.Permissions" Version="6.0.0" />
```

## <a name="affected-apis"></a>受影响的 API

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
