---
title: 中断性变更：Blazor：更新的静态 Web 资产的验证逻辑
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：更新的静态 Web 资产的验证逻辑
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: b6fb8e9a10157a97119fa07a95fb3dbb8b207214
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498147"
---
# <a name="blazor-updated-validation-logic-for-static-web-assets"></a>Blazor：更新的静态 Web 资产的验证逻辑

在 ASP.NET Core 3.1 和 Blazor WebAssembly 3.2 中，静态 Web 资产的冲突验证存在问题。 问题：

* 阻碍了主机资产和 Razor 类库 (RCL) 及 Blazor WebAssembly 应用中资产之间的正常冲突检测。
* 通常会影响 Blazor WebAssembly 应用，因为默认情况下，RCL 中的静态 Web 资产在 `_content/$(PackageId)` 前缀下提供。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="old-behavior"></a>旧行为

在开发过程中，RCL 的静态 Web 资产可能会被同一主机路径上的主机项目资产以静默方式重写。 假设有一个 RCL 定义了要在 /folder/file.txt 提供的静态 Web 资产。 如果主机将文件放在 wwwroot/folder/file.txt，则服务器上的文件会以静默方式重写 RCL 或 Blazor WebAssembly 应用上的文件。

## <a name="new-behavior"></a>新行为

ASP.NET Core 正确检测何时发生此问题。 它会通知你（即用户）存在冲突，以便你可以采取适当的操作。

## <a name="reason-for-change"></a>更改原因

静态 Web 资产不应由项目的 wwwroot 主机上的文件替代。 允许重写这些文件可能会导致难以诊断的错误。 结果可能是已发布应用中未定义的行为更改。

## <a name="recommended-action"></a>建议操作

默认情况下，RCL 文件没有理由与主机上的文件冲突。 RCL 文件以 `_content/${PackageId}` 为前缀。 Blazor WebAssembly 文件位于主机 URL 空间的根目录中，这使得更容易出现冲突。 例如，Blazor WebAssembly 应用包含 favicon.ico 文件，主机可能包含在其 wwwroot 文件夹中 。

如果冲突的源是 RCL 文件，通常意味着代码将资产从库复制到项目的 wwwroot 文件夹中。 编写代码来复制文件会破坏静态 Web 资产的主要目标。 必须实现此目标，才能在更新内容时获取对浏览器的更新，而无需触发新的编译。

可以选择保留此行为并维护主机上的文件。 为此，请从具有自定义 MSBuild 目标的静态 Web 资产列表中删除该文件。

若要使用 RCL 的文件或 Blazor WebAssembly 应用的文件而不是主机项目的文件，请从主机项目中删除该文件。

## <a name="affected-apis"></a>受影响的 API

无

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
