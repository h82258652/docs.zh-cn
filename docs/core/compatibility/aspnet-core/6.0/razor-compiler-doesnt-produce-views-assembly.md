---
title: 中断性变更：Razor：编译器现在生成单个程序集
description: 了解 ASP.NET Core 6.0 中的中断性变更，其中，Razor 编译器不会再使用两步编译过程来生成两个单独的程序集。
no-loc:
- Razor
ms.date: 04/08/2021
ms.openlocfilehash: 8b6817563c4670aebfe681d5f24c8e309d2500ad
ms.sourcegitcommit: fdfa01f6cd3aa4c36b6e8a1830693ff22d35aeea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107299275"
---
# <a name="razor-compiler-no-longer-produces-a-views-assembly"></a>Razor：编译器不再生成 Views 程序集

Razor 编译器不再生成单独的 Views.dll 文件，该文件中包含在应用程序中定义的 CSHTML 视图。

## <a name="version-introduced"></a>引入的版本

ASP.NET Core 6.0 预览版 3

## <a name="old-behavior"></a>旧行为

在以前的版本中，Razor 编译器会利用两步编译过程，该过程将生成两个文件：

- 一个主 AppName.dll 程序集，其中包含应用程序类型。
- 一个 AppName.Views.dll 程序集，其中包含在应用中定义的生成的视图。 生成的视图类型为 `public`，位于 `AspNetCore` 命名空间下。

## <a name="new-behavior"></a>新行为

视图和应用程序类型都包含在一个 AppName.dll 程序集中。 默认情况下，视图类型具有可访问性修饰符 `internal` 和 `sealed`，并包含在 `AspNetCoreGeneratedDocument` 名称空间下。

## <a name="reason-for-change"></a>更改原因

删除该两步编译过程：

* 提高使用 Razor 视图的应用程序的生成性能。
* 允许 Razor 视图参与 Visual Studio 的“热重载”体验。

## <a name="recommended-action"></a>建议操作

无。

## <a name="affected-apis"></a>受影响的 API

无。

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis

-->
