---
title: switch 表达式 - C# 参考
description: 详细了解根据模式匹配提供类似 switch 的语义的 C# switch 表达式
ms.date: 04/16/2021
f1_keywords:
- switch_CSharpKeyword
helpviewer_keywords:
- switch expression [C#]
- pattern matching [C#]
ms.openlocfilehash: ce892598f364e4934613a797d9290fa385abd4c8
ms.sourcegitcommit: 23e2bc267872ce6ea22db4bf6478fe57bcba4bc8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2021
ms.locfileid: "107592261"
---
# <a name="switch-expression-c-reference"></a>switch 表达式（C# 参考）

从 C# 8.0 开始，可以使用 `switch` 表达式，根据与输入表达式匹配的模式，对候选表达式列表中的单个表达式进行求值。 有关在语句上下文中支持 `switch` 类语义的 `switch` 语句的信息，请参阅 [`switch` 语句](../keywords/switch.md)一文。

下面的示例演示了一个 `switch` 表达式，该表达式将在线地图中表示视觉方向的 [`enum`](../builtin-types/enum.md) 中的值转换为相应的基本方位：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetBasicStructure":::

上述示例展示了 `switch` 表达式的基本元素：

- 后跟 `switch` 关键字的表达式。 在上述示例中，这是 `direction` 方法参数。
- `switch` expression arm，用逗号分隔。 每个 `switch` expression arm 都包含一个模式、一个可选的 [case guard](#case-guards)、`=>` 标记和一个表达式  。

在上述示例中，`switch` 表达式使用以下模式：

- [常数模式](patterns.md#constant-pattern)，用于处理 `Direction` 枚举的定义值。
- [弃元模式](patterns.md#discard-pattern)用于处理没有相应的 `Direction` 枚举成员的任何整数值（例如 `(Direction)10`）。 这会使 `switch` 表达式[详尽](#non-exhaustive-switch-expressions)。

有关 `switch` 表达式支持的模式的信息，请参阅[模式](patterns.md)。

`switch` 表达式的结果是第一个 `switch` expression arm 的表达式的值，该 switch expression arm 的模式与范围表达式匹配，并且它的 case guard（如果存在）求值为 `true`。 `switch` expression arm 按文本顺序求值。

如果无法选择较低的 `switch` expression arm，编译器会发出错误，因为较高的 `switch` expression arm 匹配其所有值。

## <a name="case-guards"></a>Case guard

模式或许表现力不够，无法指定用于计算 arm 的表达式的条件。 在这种情况下，可以使用 case guard。 这是一个附加条件，必须与匹配模式同时满足。 可以在模式后面的 `when` 关键字之后指定一个 case guard，如以下示例所示：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="CaseGuardExample":::

上述示例使用带有嵌套 [var 模式](patterns.md#var-pattern)的[属性模式](patterns.md#property-pattern)。

## <a name="non-exhaustive-switch-expressions"></a>非详尽的 switch 表达式

如果 `switch` 表达式的模式均未捕获输入值，则运行时将引发异常。 在 .NET Core 3.0 及更高版本中，异常是 <xref:System.Runtime.CompilerServices.SwitchExpressionException?displayProperty=nameWithType>。 在 .NET Framework 中，异常是 <xref:System.InvalidOperationException>。 如果 `switch` 表达式未处理所有可能的输入值，则编译器会生成警告。

> [!TIP]
> 为了保证 `switch` 表达式处理所有可能的输入值，请为 `switch` expression arm 提供[弃元模式](patterns.md#discard-pattern)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-8.0/patterns.md)的 [`switch` 表达式](~/_csharplang/proposals/csharp-8.0/patterns.md#switch-expression)部分。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [模式](patterns.md)
- [教程：使用模式匹配来构建类型驱动和数据驱动的算法](../../tutorials/pattern-matching.md)
- [`switch` 语句](../keywords/switch.md)
