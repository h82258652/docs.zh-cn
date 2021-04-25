---
title: 预定义的配置文件（代码分析）
description: 了解如何面向特定代码分析类型使用预定义的 EditorConfig 和规则集文件。
ms.date: 09/24/2020
ms.topic: conceptual
ms.openlocfilehash: 748ab8a9ddfcfcadeb33da877769cedac901655a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876586"
---
# <a name="predefined-configuration-files"></a>预定义的配置文件

使用预定义的 EditorConfig 和[规则集](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules)文件，可以快速轻松地启用某一类别的代码质量规则，如安全性或设计规则。 通过启用特定类别的规则，可以确定目标问题和特定情况。 若要访问这些预定义的文件，请安装 [Microsoft.CodeAnalysis.NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) NuGet 分析器包。

[Microsoft.CodeAnalysis.NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) 包括用于以下规则类别的预定义 EditorConfig 文件和规则集：

- 所有规则
- 数据流
- 设计
- 文档
- 全球化
- 互操作性
- 可维护性
- 命名
- 性能
- 从 FxCop 移植
- 可靠性
- 安全性
- 使用情况

每类规则都有一个 EditorConfig 或规则集文件，用于：

- 启用相应类别中的所有规则（并禁用所有其他规则）
- 使用每个规则由默认设置启用的默认严重性（并禁用所有其他规则）

> [!TIP]
> “所有规则”类别具有一个额外的 EditorConfig 或规则集文件，用于禁用所有规则。 可使用此文件快速清除项目中的任何分析器警告或错误。

## <a name="predefined-editorconfig-files"></a>预定义的 EditorConfig 文件

Microsoft.CodeAnalysis.NetAnalyzers 分析器包的预定义 EditorConfig 文件位于 NuGet 包安装位置的“editorconfig”子目录中。 例如，用于启用所有安全规则的 EditorConfig 文件位于 editorconfig/SecurityRulesEnabled/.editorconfig。

请将所选的 .editorconfig 文件复制到项目的根目录中。

## <a name="predefined-rule-sets"></a>预定义规则集

Microsoft.CodeAnalysis.NetAnalyzers 分析器包的预定义规则集文件位于 NuGet 包安装位置的“rulesets”子目录中。 例如，用于启用所有安全规则的规则集文件位于 rulesets/SecurityRulesEnabled.ruleset。 请复制一个或多个规则集，并将其粘贴到包含你的项目的目录中。

## <a name="see-also"></a>请参阅

- [分析器配置](https://github.com/dotnet/roslyn-analyzers/blob/main/docs/Analyzer%20Configuration.md)
- [EditorConfig 的 .NET 代码样式规则选项](code-style-rule-options.md)
