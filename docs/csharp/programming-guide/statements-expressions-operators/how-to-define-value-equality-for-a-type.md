---
title: 如何为类或结构定义值相等性 - C# 编程指南
description: 了解如何为类或结构定义值相等性。 查看代码示例和可用资源。
ms.date: 03/26/2021
helpviewer_keywords:
- overriding Equals method [C#]
- object equivalence [C#]
- Equals method [C#], overriding
- value equality [C#]
- equivalence [C#]
ms.assetid: 4084581e-b931-498b-9534-cf7ef5b68690
ms.openlocfilehash: fd8e1e14650a836178534b44dc332215c0d0586a
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079448"
---
# <a name="how-to-define-value-equality-for-a-class-or-struct-c-programming-guide"></a>如何为类或结构定义值相等性（C# 编程指南）

[记录](../classes-and-structs/records.md)自动实现值相等性。 当你的类型为数据建模并应实现值相等性时，请考虑定义 `record` 而不是 `class`。

定义类或结构时，需确定为类型创建值相等性（或等效性）的自定义定义是否有意义。 通常，预期将类型的对象添加到集合时，或者这些对象主要用于存储一组字段或属性时，需实现值相等性。 可以基于类型中所有字段和属性的比较结果来定义值相等性，也可以基于子集进行定义。

在任何一种情况下，类和结构中的实现均应遵循 5 个等效性保证条件（对于以下规则，假设 `x`、`y` 和 `z` 都不为 null）：  
  
1. 自反属性：`x.Equals(x)` 将返回 `true`。
  
2. 对称属性：`x.Equals(y)` 返回与 `y.Equals(x)` 相同的值。
  
3. 可传递属性：如果 `(x.Equals(y) && y.Equals(z))` 返回 `true`，则 `x.Equals(z)` 将返回 `true`。
  
4. 只要未修改 x 和 y 引用的对象，`x.Equals(y)` 的连续调用就将返回相同的值。  
  
5. 任何非 null 值均不等于 null。 然而，当 `x` 为 null 时，`x.Equals(y)` 将引发异常。 这会违反规则 1 或 2，具体取决于 `Equals` 的参数。

定义的任何结构都已具有其从 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法的 <xref:System.ValueType?displayProperty=nameWithType> 替代中继承的值相等性的默认实现。 此实现使用反射来检查类型中的所有字段和属性。 尽管此实现可生成正确的结果，但与专门为类型编写的自定义实现相比，它的速度相对较慢。  
  
类和结构的值相等性的实现详细信息有所不同。 但是，类和结构都需要相同的基础步骤来实现相等性：  
  
1. 替代[虚拟](../../language-reference/keywords/virtual.md) <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法。 大多数情况下，`bool Equals( object obj )` 实现应只调入作为 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口的实现的类型特定 `Equals` 方法。 （请参阅步骤 2。）  
  
2. 通过提供类型特定的 `Equals` 方法实现 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口。 实际的等效性比较将在此接口中执行。 例如，可能决定通过仅比较类型中的一两个字段来定义相等性。 不会从 `Equals` 引发异常。 对于与继承相关的类：

   * 此方法应仅检查类中声明的字段。 它应调用 `base.Equals` 来检查基类中的字段。 （如果类型直接从 <xref:System.Object> 中继承，则不会调用 `base.Equals`，因为 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 的 <xref:System.Object> 实现会执行引用相等性检查。）

   * 仅当要比较的变量的运行时类型相同时，才应将两个变量视为相等。 此外，如果变量的运行时和编译时类型不同，请确保使用运行时类型的 `Equals` 方法的 `IEquatable` 实现。 确保始终正确比较运行时类型的一种策略是仅在 `sealed` 类中实现 `IEquatable`。 有关详细信息，请参阅本文后续部分的[类示例](#class-example)。
  
3. 可选但建议这样做：重载 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 和 [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 运算符。  
  
4. 替代 <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>，以便具有值相等性的两个对象生成相同的哈希代码。  
  
5. 可选：若要支持“大于”或“小于”定义，请为类型实现 <xref:System.IComparable%601> 接口，并同时重载 [<=](../../language-reference/operators/comparison-operators.md#less-than-or-equal-operator-) 和 [>=](../../language-reference/operators/comparison-operators.md#greater-than-or-equal-operator-) 运算符。  

> [!NOTE]
> 从 C# 9.0 开始，可以使用记录来获取值相等性语义，而不需要任何不必要的样板代码。

## <a name="class-example"></a>类示例

下面的示例演示如何在类（引用类型）中实现值相等性。

:::code language="csharp" source="snippets/how-to-define-value-equality-for-a-type/ValueEqualityClass/Program.cs":::

在类（引用类型）上，两种 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法的默认实现均执行引用相等性比较，而不是值相等性检查。 实施者替代虚方法时，目的是为其指定值相等性语义。

即使类不重载 `==` 和 `!=` 运算符，也可将这些运算符与类一起使用。 但是，默认行为是执行引用相等性检查。 在类中，如果重载 `Equals` 方法，则应重载 `==` 和 `!=` 运算符，但这并不是必需的。

> [!IMPORTANT]
> 前面的示例代码可能无法按照预期的方式处理每个继承方案。 考虑下列代码：
>
> ```csharp
> TwoDPoint p1 = new ThreeDPoint(1, 2, 3);
> TwoDPoint p2 = new ThreeDPoint(1, 2, 4);
> Console.WriteLine(p1.Equals(p2)); // output: True
> ```
>
> 根据此代码报告，尽管 `z` 值有所不同，但 `p1` 等于 `p2`。 由于编译器会根据编译时类型选取 `IEquatable` 的 `TwoDPoint` 实现，因而会忽略该差异。
>
> `record` 类型的内置值相等性可以正确处理这类场景。 如果 `TwoDPoint` 和 `ThreeDPoint` 是 `record` 类型，则 `p1.Equals(p2)` 的结果会是 `False`。 有关详细信息，请参阅 [`record` 类型继承层次结果中的相等性](../../language-reference/builtin-types/record.md#equality-in-inheritance-hierarchies)。

## <a name="struct-example"></a>结构示例

下面的示例演示如何在结构（值类型）中实现值相等性：

:::code language="csharp" source="snippets/how-to-define-value-equality-for-a-type/ValueEqualityStruct/Program.cs":::
  
对于结构，<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType>（<xref:System.ValueType?displayProperty=nameWithType> 中的替代版本）的默认实现通过使用反射来比较类型中每个字段的值，从而执行值相等性检查。 实施者替代结构中的 `Equals` 虚方法时，目的是提供更高效的方法来执行值相等性检查，并选择根据结构字段或属性的某个子集来进行比较。
  
除非结构显式重载了 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 和 [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 运算符，否则这些运算符无法对结构进行运算。

## <a name="see-also"></a>请参阅

- [相等性比较](equality-comparisons.md)
- [C# 编程指南](../index.md)
