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
# <a name="extensions-package-reference-changes-affecting-some-nuget-packages"></a>扩展：影响某些 NuGet 包的包引用更改

如 [aspnet/Announcements#411](https://github.com/aspnet/Announcements/issues/411) 中所述，通过将某些 `Microsoft.Extensions.*` NuGet 包从 [dotnet/extensions](https://github.com/dotnet/extensions) 存储库迁移到 [dotnet/runtime](https://github.com/dotnet/runtime)，可将打包更改应用于某些已迁移的包。 有关此问题的讨论，请参阅 [dotnet/aspnetcore#21033](https://github.com/dotnet/aspnetcore/issues/21033)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 4

## <a name="old-behavior"></a>旧行为

某些 `Microsoft.Extensions.*` 包包含应用依赖的 API 的包引用。

## <a name="new-behavior"></a>新行为

你的应用可能必须添加 `Microsoft.Extensions.*` 包依赖项。

## <a name="reason-for-change"></a>更改原因

打包策略已更新，以更好地与 *dotnet/runtime* 存储库保持一致。 如果采用新策略，在打包过程中，未使用的包引用将从 .nupkg 文件中删除  。

## <a name="recommended-action"></a>建议操作

如果使用删除的包依赖项中的 API，受影响的包的使用者应在项目中添加对删除的包依赖项的直接依赖项。 下表列出了受影响的包和相应的更改。

|包名称|更改描述|
|------------|------------------|
|[Microsoft.Extensions.Configuration.Binder](https://nuget.org/packages/Microsoft.Extensions.Configuration.Binder)|已删除对 `Microsoft.Extensions.Configuration` 的引用|
|[Microsoft.Extensions.Configuration.Json](https://nuget.org/packages/Microsoft.Extensions.Configuration.Json)    |已删除对 `System.Threading.Tasks.Extensions` 的引用|
|[Microsoft.Extensions.Hosting.Abstractions](https://nuget.org/packages/Microsoft.Extensions.Hosting.Abstractions)|已删除对 `Microsoft.Extensions.Logging.Abstractions` 的引用|
|[Microsoft.Extensions.Logging](https://nuget.org/packages/Microsoft.Extensions.Logging)                          |已删除对 `Microsoft.Extensions.Configuration.Binder` 的引用|
|[Microsoft.Extensions.Logging.Console](https://nuget.org/packages/Microsoft.Extensions.Logging.Console)          |已删除对 `Microsoft.Extensions.Configuration.Abstractions` 的引用|
|[Microsoft.Extensions.Logging.EventLog](https://nuget.org/packages/Microsoft.Extensions.Logging.EventLog)        |已删除对 .NET Framework 4.6.1 目标框架名字对象的 `System.Diagnostics.EventLog` 的引用|
|[Microsoft.Extensions.Logging.EventSource](https://nuget.org/packages/Microsoft.Extensions.Logging.EventSource)  |已删除对 `System.Threading.Tasks.Extensions` 的引用|
|[Microsoft.Extensions.Options](https://nuget.org/packages/Microsoft.Extensions.Options)                          |已删除对 `System.ComponentModel.Annotations` 的引用|

例如，对 `Microsoft.Extensions.Configuration` 的包引用已从 `Microsoft.Extensions.Configuration.Binder` 中删除。 包中未使用依赖项的 API。 依赖于 `Microsoft.Extensions.Configuration` 中 API 的 `Microsoft.Extensions.Configuration.Binder` 用户应该在项目中添加对它的直接引用。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
