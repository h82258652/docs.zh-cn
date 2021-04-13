---
title: 中断性变更：身份验证：标记为已过时的 AzureAD.UI 和 AzureADB2C.UI API 和包
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：身份验证：标记为已过时的 AzureAD.UI 和 AzureADB2C.UI API 和包
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 179b0b248131a7c02c5f79fbba4562be0ad49d77
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498264"
---
# <a name="authentication-azureadui-and-azureadb2cui-apis-and-packages-marked-obsolete"></a>身份验证：标记为已过时的 AzureAD.UI 和 AzureADB2C.UI API 和包

在 ASP.NET Core 2.1 中，与 Azure Active Directory (Azure AD) 和 Azure Active Directory B2C (Azure AD B2C) 身份验证的集成通过 [Microsoft.AspNetCore.Authentication.AzureAD.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI) 和 [Microsoft.AspNetCore.Authentication.AzureADB2C.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI) 包提供。 这些包提供的此功能基于 Azure AD v1.0 终结点。

在 ASP.NET Core 5.0 及更高版本中，与 Azure AD 和 Azure AD B2C 身份验证的集成通过 [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web) 包提供。 此包基于 Microsoft 标识平台，该平台以前称为 Azure AD v2.0 终结点。 因此，`Microsoft.AspNetCore.Authentication.AzureAD.UI` 和 `Microsoft.AspNetCore.Authentication.AzureADB2C.UI` 包中的旧 API 已弃用。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#25807](https://github.com/dotnet/aspnetcore/issues/25807)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 8

## <a name="old-behavior"></a>旧行为

这些 API 未标记为已过时。

## <a name="new-behavior"></a>新行为

这些 API 标记为已过时。

## <a name="reason-for-change"></a>更改原因

Azure AD 和 Azure AD B2C 身份验证功能已迁移到 `Microsoft.Identity.Web` 提供的 Microsoft 身份验证库 (MSAL) API。

## <a name="recommended-action"></a>建议操作

按照适用于 [Web 应用](https://github.com/azuread/microsoft-identity-web/wiki/web-apps) 的 `Microsoft.Identity.Web` API 指南和 [Web API](https://github.com/azuread/microsoft-identity-web/wiki/web-apis) 进行操作。

## <a name="affected-apis"></a>受影响的 API

* [Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions](/dotnet/api/microsoft.aspnetcore.authentication.azureadauthenticationbuilderextensions?view=aspnetcore-3.0)
* [Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADDefaults](/dotnet/api/microsoft.aspnetcore.authentication.azuread.ui.azureaddefaults?view=aspnetcore-3.0)
* [Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADOptions](/dotnet/api/microsoft.aspnetcore.authentication.azuread.ui.azureadoptions?view=aspnetcore-3.0)
* [Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions](/dotnet/api/microsoft.aspnetcore.authentication.azureadb2cauthenticationbuilderextensions?view=aspnetcore-3.0)
* [Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2CDefaults](/dotnet/api/microsoft.aspnetcore.authentication.azureadb2c.ui.azureadb2cdefaults?view=aspnetcore-3.0)
* [Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2COptions](/dotnet/api/microsoft.aspnetcore.authentication.azureadb2c.ui.azureadb2coptions?view=aspnetcore-3.0)

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions`
- `T:Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADDefaults`
- `T:Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADOptions`
- `T:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions`
- `T:Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2CDefaults`
- `T:Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2COptions`

-->
