---
title: 记录 - C# 参考
description: 了解 C# 中的记录类型
ms.date: 02/25/2021
f1_keywords:
- record_CSharpKeyword
helpviewer_keywords:
- record keyword [C#]
- record type [C#]
ms.openlocfilehash: 067046a66c6eb1f314c123de8bde2d023f1f374a
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255111"
---
# <a name="records-c-reference"></a>记录（C# 参考）

从 C# 9 开始，可以使用 `record` 关键字定义一个[引用类型](reference-types.md)，用来提供用于封装数据的内置功能。 通过使用位置参数或标准属性语法，可以创建具有不可变属性的记录类型：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PositionalRecord":::
:::code language="csharp" source="snippets/shared/RecordType.cs" id="ImmutableRecord":::

此外，还可以创建具有可变属性和字段的记录类型：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="MutableRecord":::

虽然记录可以是可变的，但它们主要用于支持不可变的数据模型。 记录类型提供以下功能：

* [用于创建具有不可变属性的引用类型的简明语法](#positional-syntax-for-property-definition)
* 内置行为对于以数据为中心的引用类型非常有用：
  * [值相等性](#value-equality)
  * [非破坏性变化的简明语法](#nondestructive-mutation)
  * [用于显示的内置格式设置](#built-in-formatting-for-display)
* [支持继承层次结构](#inheritance)

你还可以使用[结构类型](struct.md)来设计以数据为中心的类型，这些类型提供值相等性，并且很少或没有任何行为。 但对于相对较大的数据模型，结构类型有一些缺点：

* 它们不支持继承。
* 它们在确定值相等性时效率较低。 对于值类型，<xref:System.ValueType.Equals%2A?displayProperty=nameWithType> 方法使用反射来查找所有字段。 对于记录，编译器将生成 `Equals` 方法。 实际上，记录中的值相等性实现的速度明显更快。
* 在某些情况下，它们会占用更多内存，因为每个实例都有所有数据的完整副本。 记录类型是[引用类型](reference-types.md)，因此，记录实例只包含对数据的引用。

## <a name="positional-syntax-for-property-definition"></a>属性定义的位置语法

在创建实例时，可以使用位置参数来声明记录的属性，并初始化属性值：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="InstantiatePositional":::

当你为属性定义使用位置语法时，编译器将创建以下内容：

* 为记录声明中提供的每个位置参数提供一个公共的 init-only 自动实现的属性。 [init-only](../keywords/init.md) 属性只能在构造函数中或使用属性初始值设定项来设置。
* 主构造函数，它的参数与记录声明上的位置参数匹配。
* 一个 `Deconstruct` 方法，对记录声明中提供的每个位置参数都有一个 `out` 参数。 只有当有两个或更多的位置参数时，才会提供这个方法。 此方法解构了使用位置语法定义的属性；它忽略了使用标准属性语法定义的属性。

如果生成的自动实现的属性定义并不是你所需要的，你可以自行定义同名的属性。 如果这样做，生成的构造函数和解构函数将使用你的属性定义。 例如，下面的示例定义了 `FirstName` 位置属性 `internal` 而不是 `public`。

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PositionalWithManualProperty":::

记录类型不需要声明任何位置属性。 你可以在没有任何位置属性的情况下声明一个记录，也可以声明其他字段和属性，如以下示例中所示：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="MixedSyntax":::

如果使用标准属性语法来定义属性，但省略了访问修饰符，这些属性将隐式地 `public`。
<!-- Todo -- Explain issues surrounding use of attributes on positional properties. -->

## <a name="immutability"></a>不可变性

记录类型不一定是不可变的。 可以用 `set` 访问器和非 `readonly` 的字段来声明属性。 虽然记录可以是可变的，但它们使创建不可变的数据模型变得更容易。

如果你需要一个以数据为中心的类型是线程安全的，或者需要使哈希表中的哈希代码保持不变，那么不可变性很有用。 但不可变性并不是适用于所有的数据场景。 例如，[Entity Framework Core](/ef/core/) 就不支持通过不可变实体类型进行更新。

init-only 属性无论是通过位置参数创建的，还是通过指定 `init` 访问器创建的，都具有浅的不可变性。 初始化后，将不能更改值型属性的值或引用型属性的引用。 不过，引用型属性引用的数据是可以更改的。 下面的示例展示了引用型不可变属性的内容（本例中是数组）是可变的：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="ShallowImmutability":::

记录类型特有的功能是由编译器合成的方法实现的，这些方法都不会通过修改对象状态来影响不可变性。

## <a name="value-equality"></a>值相等性

值相等性是指如果记录类型的两个变量类型相匹配，且所有属性和字段值都一致，那么记录类型的两个变量是相等的。 对于其他引用类型，相等是指标识。 也就是说，如果一个引用类型的两个变量引用同一个对象，那么这两个变量是相等的。

一些数据模型需要引用相等性。 例如，[Entity Framework Core](/ef/core/) 依赖于引用相等性来确保它对概念上是一个实体的实体类型只使用一个实例。 因此，记录类型不适合用作 Entity Framework Core 中的实体类型。

下面的示例说明了记录类型的值相等性：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="Equality":::

为了实现值相等性，编译器合成了以下方法：

* <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> 的替代。

  当两个参数都为非 NULL 时，此方法被用作 <xref:System.Object.Equals(System.Object,System.Object)?displayProperty=nameWithType> 静态方法的基础。

* 一个虚拟的 `Equals` 方法，其参数为记录类型。 此方法实现 <xref:System.IEquatable%601>。

* <xref:System.Object.GetHashCode?displayProperty=nameWithType> 的替代。

* 运算符 `==` 和 `!=` 的替代。

在 `class` 类型中，可以手动替代相等性方法和运算符以实现值相等性，但开发和测试这种代码会非常耗时，而且容易出错。 内置此功能可防止在添加或更改属性或字段时忘记更新自定义替代代码导致的 bug。

你可以编写自己的实现来替换这些合成的方法。 如果记录类型的方法与任何合成方法的签名匹配，则编译器不会合成该方法。

如果在记录类型中提供自己的 `Equals` 实现，则还应提供 `GetHashCode` 的实现。

## <a name="nondestructive-mutation"></a>非破坏性变化

如果需要改变记录实例的不可变属性，可以使用 `with` 表达式来实现非破坏性变化。 `with` 表达式创建一个新的记录实例，该实例是现有记录实例的一个副本，修改了指定属性和字段。 使用[对象初始值设定项](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)语法来指定要更改的值，如以下示例中所示：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="WithExpressions":::

`with` 表达式可以设置位置属性或使用标准属性语法创建的属性。 非位置属性必须有一个 `init` 或 `set` 访问器才能在 `with` 表达式中进行更改。

`with` 表达式的结果是一个浅的副本，这意味着对于引用属性，只复制对实例的引用。 原始记录和副本最终都具有对同一实例的引用。

为了实现此功能，编译器合成了一个克隆方法和一个复制构造函数。 构造函数接收一个要复制的记录的实例，然后调用克隆方法。 当你使用 `with` 表达式时，编译器将创建调用复制构造函数的代码，然后设置在 `with` 表达式中指定的属性。

如果你需要不同的复制行为，可以编写自己的复制构造函数。 如果你这样做，编译器将不会合成复制构造函数。 如果记录是 `sealed`，则使构造函数为 `private`，否则使其为 `protected`。

你不能替代克隆方法，也不能创建名为 `Clone` 的成员。 克隆方法的实际名称是由编译器生成的。

## <a name="built-in-formatting-for-display"></a>用于显示的内置格式设置

记录类型具有编译器生成的 <xref:System.Object.ToString%2A> 方法，可显示公共属性和字段的名称和值。 `ToString` 方法返回一个格式如下的字符串：

> \<record type name> { \<property name> = \<value>, \<property name> = \<value>, ...}

对于引用类型，将显示属性所引用的对象的类型名称，而不是属性值。 在下面的示例中，数组是一个引用类型，因此显示的是 `System.String[]`，而不是实际的数组元素值：

```
Person { FirstName = Nancy, LastName = Davolio, ChildNames = System.String[] }
```

为了实现此功能，编译器合成了一个虚拟 `PrintMembers` 方法和一个 <xref:System.Object.ToString%2A> 替代。
`ToString` 替代创建了一个 <xref:System.Text.StringBuilder> 对象，它的类型名称后跟一个左括号。 它调用 `PrintMembers` 以添加属性名称和值，然后添加右括号。 下面的示例展示了类似于合成替代中包含的代码：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="ToStringOverrideDefault":::

你可以提供自己的 `PrintMembers` 实现或 `ToString` 替代。 本文后面的[派生记录中的 `PrintMembers` 格式设置](#printmembers-formatting-in-derived-records)一节中提供了示例。

## <a name="inheritance"></a>继承

一条记录可以从另一条记录继承。 但是，记录不能从类继承，类也不能从记录继承。

### <a name="positional-parameters-in-derived-record-types"></a>派生记录类型中的位置参数

派生记录为基本记录主构造函数中的所有参数声明位置参数。 基本记录声明并初始化这些属性。 派生记录不会隐藏它们，而只会创建和初始化未在其基本记录中声明的参数的属性。

下面的示例说明了具有位置属性语法的继承：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PositionalInheritance":::

### <a name="equality-in-inheritance-hierarchies"></a>继承层次结构中的相等性

要使两个记录变量相等，运行时类型必须相等。 包含变量的类型可能不同。 下面的代码示例中说明了这一点：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="InheritanceEquality":::

在本示例中，所有实例都具有相同的属性和相同的属性值。 但 `student == teacher` 返回 `False`（尽管这两个都是 `Person` 型变量），`student == student2` 返回 `True`（尽管一个是 `Person` 变量，而另一个是 `Student` 变量）。

为实现此行为，编译器合成了一个 `EqualityContract` 属性，该属性返回一个与记录类型匹配的 <xref:System.Type> 对象。 这样，相等性方法在检查相等性时，就可以比较对象的运行时类型。 如果记录的基类型为 `object`，则此属性为 `virtual`。 如果基类型是其他记录类型，则此属性是一个替代。 如果记录类型为 `sealed`，则此属性为 `sealed`。

在比较派生类型的两个实例时，合成的相等性方法会检查基类型和派生类型的所有属性是否相等。 合成的 `GetHashCode` 方法从基类型和派生的记录类型中声明的所有属性和字段中使用 `GetHashCode` 方法。

### <a name="with-expressions-in-derived-records"></a>派生记录中的 `with` 表达式

由于合成的克隆方法使用[协变返回类型](~/_csharplang/proposals/csharp-9.0/covariant-returns.md)，因此 `with` 表达式的结果与表达式的操作数具有相同的运行时类型。 运行时类型的所有属性都会被复制，但你只能设置编译时类型的属性，如下面的示例所示：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="WithExpressionInheritance":::

### <a name="printmembers-formatting-in-derived-records"></a>派生记录中的 `PrintMembers` 格式设置

派生记录类型的合成 `PrintMembers` 方法调用基实现。 结果是派生类型和基类型的所有公共属性和字段都包含在 `ToString` 输出中，如以下示例中所示：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="ToStringInheritance":::

你可以提供自己的 `PrintMembers` 方法的实现。 如果这样做，请使用以下签名：

* 对于派生自 `object` 的 `sealed` 记录（不声明基本记录）：使用 `private bool PrintMembers(StringBuilder builder)`；
* 对于派生自其他记录的 `sealed` 记录：使用 `protected sealed override bool PrintMembers(StringBuilder builder)`；
* 对于非 `sealed` 且派生自对象的记录：使用 `protected virtual bool PrintMembers(StringBuilder builder);`
* 对于非 `sealed` 且派生自其他记录的记录：使用 `protected override bool PrintMembers(StringBuilder builder);`

下面的代码示例替换了合成 `PrintMembers` 方法，一个是派生自对象的记录类型，另一个是派生自另一条记录的记录类型：

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PrintMembersImplementation":::

### <a name="deconstructor-behavior-in-derived-records"></a>派生记录中的解构函数行为

派生记录的 `Deconstruct` 方法返回编译时类型所有位置属性的值。 如果变量类型为基本记录，则只解构基本记录属性，除非该对象强制转换为派生类型。 下面的示例演示了如何对派生记录调用解构函数。

:::code language="csharp" source="snippets/shared/RecordType.cs" id="DeconstructorInheritance":::

## <a name="generic-constraints"></a>泛型约束

没有任何泛型约束要求类型是记录。 记录满足 `class` 约束条件。 要对记录类型的特定层次结构进行约束，请像对基类一样对基本记录进行约束。 有关详细信息，请参阅[类型参数的约束](../../programming-guide/generics/constraints-on-type-parameters.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[类](~/_csharplang/spec/classes.md)部分。

若要详细了解 C# 9 及更高版本中引入的功能，请参阅以下功能建议说明：

- [记录](~/_csharplang/proposals/csharp-9.0/records.md)
- [init-only 资源库](~/_csharplang/proposals/csharp-9.0/init.md)
- [协变返回结果](~/_csharplang/proposals/csharp-9.0/covariant-returns.md)

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [设计指南 - 在类和结构之间选择](../../../standard/design-guidelines/choosing-between-class-and-struct.md)
- [设计指南 - 结构设计](../../../standard/design-guidelines/struct.md)
- [类和结构](../../programming-guide/classes-and-structs/index.md)
- [`with`表达式](../operators/with-expression.md)
