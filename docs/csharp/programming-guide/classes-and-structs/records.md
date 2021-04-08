---
title: 记录 - C# 编程指南
description: 了解记录类型及其创建方式
ms.date: 02/26/2021
helpviewer_keywords:
- records [C#]
- C# language, records
ms.openlocfilehash: 99370e398d5e607def58334b33ae20aa59e21962
ms.sourcegitcommit: 872ca41d1c26f39d0aef57cc365d09503bac780d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106288025"
---
# <a name="records-c-programming-guide"></a>记录（C# 编程指南）

[记录](../../language-reference/builtin-types/record.md)是一个[类](../../language-reference/keywords/class.md)，它为使用数据模型提供特定的语法和行为。 有关类的详细信息，请查看[类（C# 编程指南）](classes.md)。

## <a name="when-to-use-records"></a>何时使用记录

在下列情况下，请考虑使用记录而不是类：

* 你想要定义对象不可变的引用类型。
* 你想要定义依赖[值相等性](../statements-expressions-operators/equality-comparisons.md#value-equality)的数据模型。

### <a name="immutability"></a>不可变性

不可变类型会阻止你在对象实例化后更改该对象的任何属性或字段值。 如果你需要一个类型是线程安全的，或者需要哈希代码在哈希表中国能保持不变，那么不可变性很有用。 记录为创建和使用不可变类型提供了简洁的语法。

不可变性并不适用于所有数据方案。 例如，[Entity Framework Core](/ef/core/) 不支持通过不可变实体类型进行更新。

### <a name="value-equality"></a>值相等性

对记录来说，值相等性表示当类型匹配且所有属性和字段值都匹配时，记录类型的两个变量相等。 对于其他引用类型（例如类），相等性是指[引用相等性](../statements-expressions-operators/equality-comparisons.md#reference-equality)。 也就是说，如果类的两个变量引用同一个对象，则这两个变量是相等的。 确定两个记录实例的相等性的方法和运算符使用值相等性。

并非所有数据模型都适合使用值相等性。 例如，[Entity Framework Core](/ef/core/) 依赖引用相等性，来确保它对概念上是一个实体的实体类型只使用一个实例。 因此，记录类型不适合用作 Entity Framework Core 中的实体类型。

## <a name="how-records-differ-from-classes"></a>记录与类的区别

[声明](classes.md#declaring-classes)和[实例化](classes.md#creating-objects)类时使用的语法与操作记录时的相同。 只需将 `record` 关键字替换为 `class` 关键字即可。 同样地，记录支持相同的表示继承关系的语法。 记录与类的区别如下所示：

* 可使用[位置参数](../../language-reference/builtin-types/record.md#positional-syntax-for-property-definition)创建和实例化具有不可变属性的类型。
* 在类中指示引用相等性或不相等的方法和运算符（例如 <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> 和 `==`）在记录中指示[值相等性或不相等](../../language-reference/builtin-types/record.md#value-equality)。
* 可使用 [`with` 表达式](../../language-reference/builtin-types/record.md#nondestructive-mutation)对不可变对象创建在所选属性中具有新值的副本。
* 记录的 `ToString` 方法会创建一个格式字符串，它显示对象的类型名称及其所有公共属性的名称和值。
* 记录可[从另一个记录继承](../../language-reference/builtin-types/record.md#inheritance)。 但记录不可从类继承，类也不可从记录继承。

## <a name="examples"></a>示例

下面的示例定义了一个公共记录，它使用位置参数来声明和实例化记录。 然后，它会输出类型名称和属性值：

:::code language="csharp" source="../../language-reference/builtin-types/snippets/shared/RecordType.cs" id="InstantiatePositional":::

下面的示例演示了记录中的值相等性：

:::code language="csharp" source="../../language-reference/builtin-types/snippets/shared/RecordType.cs" id="Equality":::

下面的示例演示如何使用 `with` 表达式来复制不可变对象和更改其中的一个属性：

:::code language="csharp" source="../../language-reference/builtin-types/snippets/shared/RecordType.cs" id="WithExpressions":::

有关详细信息，请查看[记录（C# 参考）](../../language-reference/builtin-types/record.md)。
  
## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>请参阅

- [类（C# 编程指南）](classes.md)
- [记录（C# 参考）](../../language-reference/builtin-types/record.md)
- [C# 编程指南](../index.md)
- [面向对象的编程](../../tutorials/intro-to-csharp/object-oriented-programming.md)
- [多态性](polymorphism.md)
- [标识符名称](../inside-a-program/identifier-names.md)
- [成员](members.md)
- [方法](methods.md)
- [构造函数](constructors.md)
- [终结器](destructors.md)
- [对象](objects.md)
