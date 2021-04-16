---
title: 可维护性规则（代码分析）
description: 了解代码分析可维护性规则。
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.maintainabilityrules
helpviewer_keywords:
- rules, maintainability
- managed code analysis rules, maintainability rules
- maintainability rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: fc2ef2b42eeba241b7af66b579a60365ad2b88c7
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590172"
---
# <a name="maintainability-rules"></a>可维护性规则

可维护性规则支持库和应用程序维护。

## <a name="in-this-section"></a>在本节中

| 规则 | 描述 |
|-----------|-----------------------------------|
| [CA1501:避免过度继承](ca1501.md) | 类型在继承层次结构中的深度超过四级。 深度嵌套的类型层次结构可能很难遵循、理解和维护。 |
| [CA1502:避免过度复杂](ca1502.md) | 此规则通过方法来测量线性独立的路径的数量，该数量是由条件分支的数量和复杂度决定的。 |
| [CA1505:避免使用无法维护的代码](ca1505.md) | 类型或方法具有较低的可维护性索引值。 如果可维护性指数较低，则表示类型或方法可能难以维护，最好重新进行设计。 |
| [CA1506:避免过度类耦合度](ca1506.md) | 此规则通过计算类型或方法包含的唯一类型引用的个数来衡量类耦合。 |
| [CA1507：使用 nameof 代替字符串](ca1507.md) | 字符串字面量用作参数，可在其中使用 `nameof` 表达式。 |
| [CA1508：避免死条件代码](ca1508.md) | 方法具有在运行时始终计算为 `true` 或 `false` 的条件代码。 这会导致条件的 `false` 分支中出现死代码。 |
| [CA1509：代码度量配置文件中的条目无效](ca1509.md) | 代码度量规则（如 [CA1501](ca1501.md)、[CA1502](ca1502.md)、[CA1505](ca1505.md) 和 [CA1506](ca1506.md)）提供了具有无效条目的名为 `CodeMetricsConfig.txt` 的配置文件。 |

## <a name="see-also"></a>另请参阅

- [测量托管代码的复杂性和可维护性](/visualstudio/code-quality/code-metrics-values)
