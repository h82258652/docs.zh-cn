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
# <a name="authentication-azureadui-and-azureadb2cui-apis-and-packages-marked-obsolete"></a><span data-ttu-id="ed010-103">身份验证：标记为已过时的 AzureAD.UI 和 AzureADB2C.UI API 和包</span><span class="sxs-lookup"><span data-stu-id="ed010-103">Authentication: AzureAD.UI and AzureADB2C.UI APIs and packages marked obsolete</span></span>

<span data-ttu-id="ed010-104">在 ASP.NET Core 2.1 中，与 Azure Active Directory (Azure AD) 和 Azure Active Directory B2C (Azure AD B2C) 身份验证的集成通过 [Microsoft.AspNetCore.Authentication.AzureAD.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI) 和 [Microsoft.AspNetCore.Authentication.AzureADB2C.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI) 包提供。</span><span class="sxs-lookup"><span data-stu-id="ed010-104">In ASP.NET Core 2.1, integration with Azure Active Directory (Azure AD) and Azure Active Directory B2C (Azure AD B2C) authentication is provided by the [Microsoft.AspNetCore.Authentication.AzureAD.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI) and [Microsoft.AspNetCore.Authentication.AzureADB2C.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI) packages.</span></span> <span data-ttu-id="ed010-105">这些包提供的此功能基于 Azure AD v1.0 终结点。</span><span class="sxs-lookup"><span data-stu-id="ed010-105">The functionality provided by these packages is based on the Azure AD v1.0 endpoint.</span></span>

<span data-ttu-id="ed010-106">在 ASP.NET Core 5.0 及更高版本中，与 Azure AD 和 Azure AD B2C 身份验证的集成通过 [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web) 包提供。</span><span class="sxs-lookup"><span data-stu-id="ed010-106">In ASP.NET Core 5.0 and later, integration with Azure AD and Azure AD B2C authentication is provided by the [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web) package.</span></span> <span data-ttu-id="ed010-107">此包基于 Microsoft 标识平台，该平台以前称为 Azure AD v2.0 终结点。</span><span class="sxs-lookup"><span data-stu-id="ed010-107">This package is based on the Microsoft Identity Platform, which is formerly known as the Azure AD v2.0 endpoint.</span></span> <span data-ttu-id="ed010-108">因此，`Microsoft.AspNetCore.Authentication.AzureAD.UI` 和 `Microsoft.AspNetCore.Authentication.AzureADB2C.UI` 包中的旧 API 已弃用。</span><span class="sxs-lookup"><span data-stu-id="ed010-108">Consequently, the old APIs in the `Microsoft.AspNetCore.Authentication.AzureAD.UI` and `Microsoft.AspNetCore.Authentication.AzureADB2C.UI` packages were deprecated.</span></span>

<span data-ttu-id="ed010-109">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#25807](https://github.com/dotnet/aspnetcore/issues/25807)。</span><span class="sxs-lookup"><span data-stu-id="ed010-109">For discussion, see GitHub issue [dotnet/aspnetcore#25807](https://github.com/dotnet/aspnetcore/issues/25807).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="ed010-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ed010-110">Version introduced</span></span>

<span data-ttu-id="ed010-111">5.0 预览版 8</span><span class="sxs-lookup"><span data-stu-id="ed010-111">5.0 Preview 8</span></span>

## <a name="old-behavior"></a><span data-ttu-id="ed010-112">旧行为</span><span class="sxs-lookup"><span data-stu-id="ed010-112">Old behavior</span></span>

<span data-ttu-id="ed010-113">这些 API 未标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="ed010-113">The APIs weren't marked as obsolete.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="ed010-114">新行为</span><span class="sxs-lookup"><span data-stu-id="ed010-114">New behavior</span></span>

<span data-ttu-id="ed010-115">这些 API 标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="ed010-115">The APIs are marked as obsolete.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="ed010-116">更改原因</span><span class="sxs-lookup"><span data-stu-id="ed010-116">Reason for change</span></span>

<span data-ttu-id="ed010-117">Azure AD 和 Azure AD B2C 身份验证功能已迁移到 `Microsoft.Identity.Web` 提供的 Microsoft 身份验证库 (MSAL) API。</span><span class="sxs-lookup"><span data-stu-id="ed010-117">The Azure AD and Azure AD B2C authentication functionality was migrated to Microsoft Authentication Library (MSAL) APIs that are provided by `Microsoft.Identity.Web`.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="ed010-118">建议操作</span><span class="sxs-lookup"><span data-stu-id="ed010-118">Recommended action</span></span>

<span data-ttu-id="ed010-119">按照适用于 [Web 应用](https://github.com/azuread/microsoft-identity-web/wiki/web-apps) 的 `Microsoft.Identity.Web` API 指南和 [Web API](https://github.com/azuread/microsoft-identity-web/wiki/web-apis) 进行操作。</span><span class="sxs-lookup"><span data-stu-id="ed010-119">Follow the `Microsoft.Identity.Web` API guidance for [web apps](https://github.com/azuread/microsoft-identity-web/wiki/web-apps) and [web APIs](https://github.com/azuread/microsoft-identity-web/wiki/web-apis).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="ed010-120">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ed010-120">Affected APIs</span></span>

* [<span data-ttu-id="ed010-121">Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions</span><span class="sxs-lookup"><span data-stu-id="ed010-121">Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.azureadauthenticationbuilderextensions?view=aspnetcore-3.0)
* [<span data-ttu-id="ed010-122">Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADDefaults</span><span class="sxs-lookup"><span data-stu-id="ed010-122">Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADDefaults</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.azuread.ui.azureaddefaults?view=aspnetcore-3.0)
* [<span data-ttu-id="ed010-123">Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADOptions</span><span class="sxs-lookup"><span data-stu-id="ed010-123">Microsoft.AspNetCore.Authentication.AzureAD.UI.AzureADOptions</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.azuread.ui.azureadoptions?view=aspnetcore-3.0)
* [<span data-ttu-id="ed010-124">Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions</span><span class="sxs-lookup"><span data-stu-id="ed010-124">Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.azureadb2cauthenticationbuilderextensions?view=aspnetcore-3.0)
* [<span data-ttu-id="ed010-125">Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2CDefaults</span><span class="sxs-lookup"><span data-stu-id="ed010-125">Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2CDefaults</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.azureadb2c.ui.azureadb2cdefaults?view=aspnetcore-3.0)
* [<span data-ttu-id="ed010-126">Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2COptions</span><span class="sxs-lookup"><span data-stu-id="ed010-126">Microsoft.AspNetCore.Authentication.AzureADB2C.UI.AzureADB2COptions</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.azureadb2c.ui.azureadb2coptions?view=aspnetcore-3.0)

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
