---
title: ASP.NET Core 中断性变更
titleSuffix: ''
description: 列出 ASP.NET Core 3.0 和 3.1 中的中断性变更。
ms.date: 11/03/2020
ms.author: scaddie
ms.openlocfilehash: 080cd3c34617e932b64806a6aa1268b4e397e410
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497211"
---
# <a name="aspnet-core-breaking-changes-for-versions-30-and-31"></a>ASP.NET Core 3.0 和 3.1 版的中断性变更

ASP.NET Core 提供 .NET Core 使用的 Web 应用开发功能。

选择以下链接之一，了解特定版本中的中断性变更：

* [ASP.NET Core 3.1](#aspnet-core-31)
* [ASP.NET Core 3.0](#aspnet-core-30)

本页记录了 ASP.NET Core 3.0 和 3.1 中的以下中断性变更：

- [已删除过时防伪、CORS、诊断、MVC 和路由 API](#obsolete-antiforgery-cors-diagnostics-mvc-and-routing-apis-removed)
- [身份验证：Google+ 弃用](#authentication-google-deprecated-and-replaced)
- [身份验证：已删除 HttpContext.Authentication 属性](#authentication-httpcontextauthentication-property-removed)
- [身份验证：已替换 Newtonsoft.json 类型](#authentication-newtonsoftjson-types-replaced)
- [身份验证：已更改 OAuthHandler ExchangeCodeAsync 签名](#authentication-oauthhandler-exchangecodeasync-signature-changed)
- [授权：AddAuthorization 重载已移到不同的程序集](#authorization-addauthorization-overload-moved-to-different-assembly)
- [授权：已从 AuthorizationFilterContext.Filters 中删除 IAllowAnonymous](#authorization-iallowanonymous-removed-from-authorizationfiltercontextfilters)
- [授权：IAuthorizationPolicyProvider 实现需要新方法](#authorization-iauthorizationpolicyprovider-implementations-require-new-method)
- [缓存：已删除 CompactOnMemoryPressure 属性](#caching-compactonmemorypressure-property-removed)
- [缓存：Microsoft.Extensions.Caching.SqlServer 使用新的 SqlClient 包](#caching-microsoftextensionscachingsqlserver-uses-new-sqlclient-package)
- [数据保护：DataProtection.Blobs 使用新的 Azure 存储 API](#data-protection-dataprotectionblobs-uses-new-azure-storage-apis)
- [托管：已从 Windows 托管捆绑包中删除 AspNetCoreModule V1](#hosting-aspnetcoremodule-v1-removed-from-windows-hosting-bundle)
- [托管：通用主机限制 Startup 构造函数注入](#hosting-generic-host-restricts-startup-constructor-injection)
- [托管：已为 IIS 进程外应用启用 HTTPS 重定向](#hosting-https-redirection-enabled-for-iis-out-of-process-apps)
- [托管：已替换 IHostingEnvironment 和 IApplicationLifetime 类型](#hosting-ihostingenvironment-and-iapplicationlifetime-types-marked-obsolete-and-replaced)
- [托管：已从 WebHostBuilder 依赖项中删除 ObjectPoolProvider](#hosting-objectpoolprovider-removed-from-webhostbuilder-dependencies)
- [HTTP：浏览器的 SameSite 更改会影响身份验证](#http-browser-samesite-changes-impact-authentication)
- [HTTP：已删除 DefaultHttpContext 扩展性](#http-defaulthttpcontext-extensibility-removed)
- [HTTP：HeaderNames 字段已更改为静态只读](#http-headernames-constants-changed-to-static-readonly)
- [HTTP：响应正文基础结构更改](#http-response-body-infrastructure-changes)
- [HTTP：已更改某些 Cookie SameSite 默认值](#http-some-cookie-samesite-defaults-changed-to-none)
- [HTTP：已默认禁用同步 IO](#http-synchronous-io-disabled-in-all-servers)
- [标识：已删除 AddDefaultUI 方法重载](#identity-adddefaultui-method-overload-removed)
- [标识：UI 启动版本更改](#identity-default-bootstrap-version-of-ui-changed)
- [标识：对于未经身份验证的标识，SignInAsync 会引发异常](#identity-signinasync-throws-exception-for-unauthenticated-identity)
- [标识：SignInManager 构造函数接受新参数](#identity-signinmanager-constructor-accepts-new-parameter)
- [标识：UI 使用静态 Web 资产功能](#identity-ui-uses-static-web-assets-feature)
- [Kestrel：已删除连接适配器](#kestrel-connection-adapters-removed)
- [Kestrel：已删除空 HTTPS 程序集](#kestrel-empty-https-assembly-removed)
- [Kestrel：请求尾部标头已移到新集合](#kestrel-request-trailer-headers-moved-to-new-collection)
- [Kestrel：传输抽象层更改](#kestrel-transport-abstractions-removed-and-made-public)
- [本地化：API 已标记为已过时](#localization-resourcemanagerwithculturestringlocalizer-and-withculture-marked-obsolete)
- [日志记录：已将 DebugLogger 类设为内部类](#logging-debuglogger-class-made-internal)
- [MVC：已删除控制器操作 Async 后缀](#mvc-async-suffix-trimmed-from-controller-action-names)
- [MVC：JsonResult 已移至 Microsoft.AspNetCore.Mvc.Core](#mvc-jsonresult-moved-to-microsoftaspnetcoremvccore)
- [MVC：已弃用预编译工具](#mvc-precompilation-tool-deprecated)
- [MVC：类型已更改为内部](#mvc-pubternal-types-changed-to-internal)
- [MVC：已删除 Web API 兼容性填充码](#mvc-web-api-compatibility-shim-removed)
- [Razor：已删除 RazorTemplateEngine API](#razor-razortemplateengine-api-removed)
- [Razor：运行时编译已移到包](#razor-runtime-compilation-moved-to-a-package)
- [会话状态：已删除过时的 API](#session-state-obsolete-apis-removed)
- [共享框架：已从 Microsoft.AspNetCore.App 中删除程序集](#shared-framework-assemblies-removed-from-microsoftaspnetcoreapp)
- [共享框架：已删除 Microsoft.AspNetCore.All](#shared-framework-removed-microsoftaspnetcoreall)
- [SignalR：已替换 HandshakeProtocol.SuccessHandshakeData](#signalr-handshakeprotocolsuccesshandshakedata-replaced)
- [SignalR：已删除 HubConnection 方法](#signalr-hubconnection-resetsendping-and-resettimeout-methods-removed)
- [SignalR：已更改 HubConnectionContext 构造函数](#signalr-hubconnectioncontext-constructors-changed)
- [SignalR：JavaScript 客户端包名称更改](#signalr-javascript-client-package-name-changed)
- [SignalR：过时的 API](#signalr-usesignalr-and-useconnections-methods-marked-obsolete)
- [SPA：SpaServices 和 NodeServices 控制台记录器回退默认更改](#spas-spaservices-and-nodeservices-no-longer-fall-back-to-console-logger)
- [SPA：SpaServices 和 NodeServices 已标记为过时](#spas-spaservices-and-nodeservices-marked-obsolete)
- [目标框架：不支持 .NET Framework](#target-framework-net-framework-support-dropped)

## <a name="aspnet-core-31"></a>ASP.NET Core 3.1

[!INCLUDE[HTTP: Browser SameSite changes impact authentication](~/includes/core-changes/aspnetcore/3.1/http-cookie-samesite-authn-impacts.md)]

***

## <a name="aspnet-core-30"></a>ASP.NET Core 3.0

[!INCLUDE[Obsolete Antiforgery, CORS, Diagnostics, MVC, and Routing APIs removed](~/includes/core-changes/aspnetcore/3.0/obsolete-apis-removed.md)]

***

[!INCLUDE[Authentication: Google+ deprecation](~/includes/core-changes/aspnetcore/3.0/authn-google-plus-authn-changes.md)]

***

[!INCLUDE[Authentication: HttpContext.Authentication property removed](~/includes/core-changes/aspnetcore/3.0/authn-httpcontext-property-removed.md)]

***

[!INCLUDE[Authentication: Newtonsoft.Json types replaced](~/includes/core-changes/aspnetcore/3.0/authn-apis-json-types.md)]

***

[!INCLUDE[Authentication: OAuthHandler ExchangeCodeAsync signature changed](~/includes/core-changes/aspnetcore/3.0/authn-exchangecodeasync-signature-change.md)]

***

[!INCLUDE[Authorization: AddAuthorization overload assembly change](~/includes/core-changes/aspnetcore/3.0/authz-assembly-change.md)]

***

[!INCLUDE[Authorization: IAllowAnonymous removed from AuthorizationFilterContext.Filters](~/includes/core-changes/aspnetcore/3.0/authz-iallowanonymous-removed-from-collection.md)]

***

[!INCLUDE[Authorization: IAuthorizationPolicyProvider implementations require new method](~/includes/core-changes/aspnetcore/3.0/authz-iauthzpolicyprovider-new-method.md)]

***

[!INCLUDE[Caching: CompactOnMemoryPressure property removed](~/includes/core-changes/aspnetcore/3.0/caching-memory-property-removed.md)]

***

[!INCLUDE[Caching: Microsoft.Extensions.Caching.SqlServer uses new SqlClient package](~/includes/core-changes/aspnetcore/3.0/caching-new-sqlclient-package.md)]

***

[!INCLUDE[Caching: ResponseCaching types changed to internal](~/includes/core-changes/aspnetcore/3.0/caching-response-pubternal-to-internal.md)]

***

[!INCLUDE[Data Protection: DataProtection.AzureStorage uses new Azure Storage APIs](~/includes/core-changes/aspnetcore/3.0/dataprotection-azstorage-using-azstorage-apis.md)]

***

[!INCLUDE[Hosting: ANCM version 1 removed from hosting bundle](~/includes/core-changes/aspnetcore/3.0/hosting-ancmv1-hosting-bundle-removal.md)]

***

[!INCLUDE[Hosting: Generic host restriction on Startup constructor injection](~/includes/core-changes/aspnetcore/3.0/hosting-generic-host-startup-ctor-restriction.md)]

***

[!INCLUDE[Hosting: HTTPS redirection enabled for IIS OutOfProcess](~/includes/core-changes/aspnetcore/3.0/hosting-https-redirection-iis-outofprocess.md)]

***

[!INCLUDE[Hosting: IHostingEnvironment and IApplicationLifetime types replaced](~/includes/core-changes/aspnetcore/3.0/hosting-ihostingenv-iapplifetime-types-replaced.md)]

***

[!INCLUDE[Hosting: ObjectPoolProvider removed from WebHostBuilder dependencies](~/includes/core-changes/aspnetcore/3.0/hosting-objectpoolprovider-webhostbuilder-dependencies.md)]

***

[!INCLUDE[HTTP: DefaultHttpContext extensibility removed](~/includes/core-changes/aspnetcore/3.0/http-defaulthttpcontext-extensibility-removed.md)]

***

[!INCLUDE[HTTP: HeaderNames fields changed to static readonly](~/includes/core-changes/aspnetcore/3.0/http-headernames-constants-staticreadonly.md)]

***

[!INCLUDE[HTTP: Response body infrastructure changes](~/includes/core-changes/aspnetcore/3.0/http-response-body-changes.md)]

***

[!INCLUDE[HTTP: Some cookie SameSite default values changed](~/includes/core-changes/aspnetcore/3.0/http-cookie-samesite-defaults-change.md)]

***

[!INCLUDE[HTTP: Synchronous IO disabled by default](~/includes/core-changes/aspnetcore/3.0/http-synchronous-io-disabled.md)]

***

[!INCLUDE[Identity: AddDefaultUI method overload removed](~/includes/core-changes/aspnetcore/3.0/identity-ui-adddefaultui-overload-removed.md)]

***

[!INCLUDE[Identity: UI Bootstrap version change](~/includes/core-changes/aspnetcore/3.0/identity-ui-bootstrap-version.md)]

***

[!INCLUDE[Identity: SignInAsync throws exception for unauthenticated identity](~/includes/core-changes/aspnetcore/3.0/identity-signinasync-throws-exception.md)]

***

[!INCLUDE[Identity: SignInManager constructor accepts new parameter](~/includes/core-changes/aspnetcore/3.0/identity-signinmanager-ctor-parameter.md)]

***

[!INCLUDE[Identity: UI uses static web assets feature](~/includes/core-changes/aspnetcore/3.0/identity-ui-static-web-assets.md)]

***

[!INCLUDE[Kestrel: Connection adapters removed](~/includes/core-changes/aspnetcore/3.0/kestrel-connection-adapters-removed.md)]

***

[!INCLUDE[Kestrel: Empty HTTPS assembly removed](~/includes/core-changes/aspnetcore/3.0/kestrel-empty-assembly-removed.md)]

***

[!INCLUDE[Kestrel: Request trailer headers moved to new collection](~/includes/core-changes/aspnetcore/3.0/kestrel-request-trailer-headers.md)]

***

[!INCLUDE[Kestrel: Transport abstraction layer changes](~/includes/core-changes/aspnetcore/3.0/kestrel-transport-abstractions.md)]

***

[!INCLUDE[Localization: APIs marked obsolete](~/includes/core-changes/aspnetcore/3.0/localization-apis-marked-obsolete.md)]

***

[!INCLUDE[Logging: DebugLogger class made internal](~/includes/core-changes/aspnetcore/3.0/logging-debuglogger-to-internal.md)]

***

[!INCLUDE[MVC: Controller action Async suffix removed](~/includes/core-changes/aspnetcore/3.0/mvc-action-async-suffix-trimmed.md)]

***

[!INCLUDE[MVC: JsonResult moved to Microsoft.AspNetCore.Mvc.Core](~/includes/core-changes/aspnetcore/3.0/mvc-jsonresult-moved.md)]

***

[!INCLUDE[MVC: Precompilation tool deprecated](~/includes/core-changes/aspnetcore/3.0/mvc-precompilation-tool-deprecated.md)]

***

[!INCLUDE[MVC: Types changed to internal](~/includes/core-changes/aspnetcore/3.0/mvc-pubternal-to-internal.md)]

***

[!INCLUDE[MVC: Web API compatibility shim removed](~/includes/core-changes/aspnetcore/3.0/mvc-webapi-compat-shim-removed.md)]

***

[!INCLUDE[Razor: RazorTemplatEengine API removed](~/includes/core-changes/aspnetcore/3.0/razor-razortemplateengine-api-removed.md)]

***

[!INCLUDE[Razor: Runtime compilation moved to a package](~/includes/core-changes/aspnetcore/3.0/razor-runtime-compilation-package.md)]

***

[!INCLUDE[Session state: Obsolete APIs removed](~/includes/core-changes/aspnetcore/3.0/session-obsolete-apis-removed.md)]

***

[!INCLUDE[Shared framework: Assembly removal from Microsoft.AspNetCore.App](~/includes/core-changes/aspnetcore/3.0/sharedfx-app-shared-framework-assemblies.md)]

***

[!INCLUDE[Shared framework: Microsoft.AspNetCore.All removed](~/includes/core-changes/aspnetcore/3.0/sharedfx-all-framework-removed.md)]

***

[!INCLUDE[SignalR: HandshakeProtocol.SuccessHandshakeData replaced](~/includes/core-changes/aspnetcore/3.0/signalr-successhandshakedata-replaced.md)]

***

[!INCLUDE[SignalR: HubConnection methods removed](~/includes/core-changes/aspnetcore/3.0/signalr-hubconnection-methods-removed.md)]

***

[!INCLUDE[SignalR: HubConnectionContext constructors changed](~/includes/core-changes/aspnetcore/3.0/signalr-hubconnectioncontext-ctors.md)]

***

[!INCLUDE[SignalR: JavaScript client package name change](~/includes/core-changes/aspnetcore/3.0/signalr-js-client-package-name.md)]

***

[!INCLUDE[SignalR: Obsolete APIs](~/includes/core-changes/aspnetcore/3.0/signalr-obsolete-apis.md)]

***

[!INCLUDE[SPAs: SpaServices and NodeServices marked obsolete](~/includes/core-changes/aspnetcore/3.0/spas-spaservices-nodeservices-obsolete.md)]

***

[!INCLUDE[SPAs: SpaServices and NodeServices console logger fallback default change](~/includes/core-changes/aspnetcore/3.0/spas-spaservices-nodeservices-fallback.md)]

***

[!INCLUDE[Target framework: .NET Framework not supported](~/includes/core-changes/aspnetcore/3.0/targetfx-netfx-tfm-support.md)]

***
