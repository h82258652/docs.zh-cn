---
title: C# 9.0 中的新增功能 - C# 指南
description: 简要介绍 C# 9.0 中提供的新功能。
ms.date: 04/07/2021
ms.openlocfilehash: c2189d2db175a40c24d6a41d20f2ae2d9384513b
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255332"
---
# <a name="whats-new-in-c-90"></a>C# 9.0 中的新增功能

C# 9.0 向 C# 语言添加了以下功能和增强功能：

- [记录](#record-types)
- [仅限 Init 的资源库](#init-only-setters)
- [顶级语句](#top-level-statements)
- [模式匹配增强功能](#pattern-matching-enhancements)
- [性能和互操作性](#performance-and-interop)
  - 本机大小的整数
  - 函数指针
  - 禁止发出 localsinit 标志
- [调整和完成功能](#fit-and-finish-features)
  - 目标类型的 `new` 表达式
  - 静态匿名函数
  - 目标类型的条件表达式
  - 协变返回类型
  - 扩展 `GetEnumerator` 支持 `foreach` 循环
  - Lambda 弃元参数
  - 本地函数的属性
- [支持代码生成器](#support-for-code-generators)
  - 模块初始值设定项
  - 分部方法的新功能

.NET 5 支持 C# 9.0。 有关详细信息，请参阅 [C# 语言版本控制](../language-reference/configure-language-version.md)。

可以从 [.NET 下载页](https://dotnet.microsoft.com/download)下载最新 .NET SDK。

## <a name="record-types"></a>记录类型

C# 9.0 引入了记录类型。 可使用 `record` 关键字定义一个引用类型，用来提供用于封装数据的内置功能。 通过使用位置参数或标准属性语法，可以创建具有不可变属性的记录类型：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="PositionalRecord":::
:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="ImmutableRecord":::

此外，还可以创建具有可变属性和字段的记录类型：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="MutableRecord":::

虽然记录可以是可变的，但它们主要用于支持不可变的数据模型。 记录类型提供以下功能：

* [用于创建具有不可变属性的引用类型的简明语法](#positional-syntax-for-property-definition)
* 行为对于以数据为中心的引用类型非常有用：
  * [值相等性](#value-equality)
  * [非破坏性变化的简明语法](#nondestructive-mutation)
  * [用于显示的内置格式设置](#built-in-formatting-for-display)
* [支持继承层次结构](#inheritance)

可使用[结构类型](../language-reference/builtin-types/struct.md)来设计以数据为中心的类型，这些类型提供值相等性，并且很少或没有任何行为。 但对于相对较大的数据模型，结构类型有一些缺点：

* 它们不支持继承。
* 它们在确定值相等性时效率较低。 对于值类型，<xref:System.ValueType.Equals%2A?displayProperty=nameWithType> 方法使用反射来查找所有字段。 对于记录，编译器将生成 `Equals` 方法。 实际上，记录中的值相等性实现的速度明显更快。
* 在某些情况下，它们会占用更多内存，因为每个实例都有所有数据的完整副本。 记录类型是[引用类型](../language-reference/builtin-types/reference-types.md)，因此，记录实例只包含对数据的引用。

### <a name="positional-syntax-for-property-definition"></a>属性定义的位置语法

在创建实例时，可以使用位置参数来声明记录的属性，并初始化属性值：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="InstantiatePositional":::

当你为属性定义使用位置语法时，编译器将创建以下内容：

* 为记录声明中提供的每个位置参数提供一个公共的 init-only 自动实现的属性。 [init-only](../language-reference/keywords/init.md) 属性只能在构造函数中或使用属性初始值设定项来设置。
* 主构造函数，它的参数与记录声明上的位置参数匹配。
* 一个 `Deconstruct` 方法，对记录声明中提供的每个位置参数都有一个 `out` 参数。

有关详细信息，请参阅有关记录的 C# 语言参考文章中的[位置语法](../language-reference/builtin-types/record.md#positional-syntax-for-property-definition)。

### <a name="immutability"></a>不可变性

记录类型不一定是不可变的。 可以用 `set` 访问器和非 `readonly` 的字段来声明属性。 虽然记录可以是可变的，但它们使创建不可变的数据模型变得更容易。 使用位置语法创建的属性是不可变的。

如果希望以数据为中心的类型是线程安全的，或者哈希表中的哈希代码保持不变，那么不可变性很有用。 它可以防止在通过引用方法传递参数而该方法意外更改参数值时发生的 bug。

记录类型特有的功能是由编译器合成的方法实现的，这些方法都不会通过修改对象状态来影响不可变性。

### <a name="value-equality"></a>值相等性

值相等性是指如果记录类型的两个变量类型相匹配，且所有属性和字段值都一致，那么记录类型的两个变量是相等的。 对于其他引用类型，相等是指标识。 也就是说，如果一个引用类型的两个变量引用同一个对象，那么这两个变量是相等的。

下面的示例说明了记录类型的值相等性：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="Equality":::

在 `class` 类型中，可以手动替代相等性方法和运算符以实现值相等性，但开发和测试这种代码会非常耗时，而且容易出错。 内置此功能可防止在添加或更改属性或字段时忘记更新自定义替代代码导致的 bug。

有关详细信息，请参阅有关记录的 C# 语言参考文章中的[值相等性](../language-reference/builtin-types/record.md#value-equality)。

### <a name="nondestructive-mutation"></a>非破坏性变化

如果需要改变记录实例的不可变属性，可以使用 `with` 表达式来实现非破坏性变化。 `with` 表达式创建一个新的记录实例，该实例是现有记录实例的一个副本，修改了指定属性和字段。 使用[对象初始值设定项](../programming-guide/classes-and-structs/object-and-collection-initializers.md)语法来指定要更改的值，如以下示例中所示：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="WithExpressions":::

有关详细信息，请参阅有关记录的 C# 语言参考文章中的[非破坏性变化](../language-reference/builtin-types/record.md#nondestructive-mutation)。

### <a name="built-in-formatting-for-display"></a>用于显示的内置格式设置

记录类型具有编译器生成的 <xref:System.Object.ToString%2A> 方法，可显示公共属性和字段的名称和值。 `ToString` 方法返回一个格式如下的字符串：

> \<record type name> { \<property name> = \<value>, \<property name> = \<value>, ...}

对于引用类型，将显示属性所引用的对象的类型名称，而不是属性值。 在下面的示例中，数组是一个引用类型，因此显示的是 `System.String[]`，而不是实际的数组元素值：

```
Person { FirstName = Nancy, LastName = Davolio, ChildNames = System.String[] }
```

有关详细信息，请参阅有关记录的 C# 语言参考文章中的[内置格式](../language-reference/builtin-types/record.md#built-in-formatting-for-display)。

### <a name="inheritance"></a>继承

一条记录可以从另一条记录继承。 但是，记录不能从类继承，类也不能从记录继承。

下面的示例说明了具有位置属性语法的继承：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="PositionalInheritance":::

要使两个记录变量相等，运行时类型必须相等。 包含变量的类型可能不同。 下面的代码示例中说明了这一点：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="InheritanceEquality":::

在本示例中，所有实例都具有相同的属性和相同的属性值。 尽管两者都是 `Person` 类型变量，但 `student == teacher` 会返回 `False`。 尽管一个是 `Person` 变量，另一个是 `Student` 变量，但 `student == student2` 会返回 `True`。

派生类型和基类型的所有公共属性和字段都包含在 `ToString` 输出中，如以下示例所示：

:::code language="csharp" source="../language-reference/builtin-types/snippets/shared/RecordType.cs" id="ToStringInheritance":::

有关详细信息，请参阅有关记录的 C# 语言参考文章中的[继承](../language-reference/builtin-types/record.md#inheritance)。

## <a name="init-only-setters"></a>仅限 Init 的资源库

仅限 init 的资源库提供一致的语法来初始化对象的成员。 属性初始值设定项可明确哪个值正在设置哪个属性。 缺点是这些属性必须是可设置的。 从 C# 9.0 开始，可为属性和索引器创建 `init` 访问器，而不是 `set` 访问器。 调用方可使用属性初始化表达式语法在创建表达式中设置这些值，但构造完成后，这些属性将变为只读。 仅限 init 的资源库提供了一个窗口用来更改状态。 构造阶段结束时，该窗口关闭。 在完成所有初始化（包括属性初始化表达式和 with 表达式）之后，构造阶段实际上就结束了。

可在编写的任何类型中声明仅限 `init` 的资源库。 例如，以下结构定义了天气观察结构：

:::code language="csharp" source="snippets/whats-new-csharp9/WeatherObservation.cs" ID="DeclareWeatherObservation":::

调用方可使用属性初始化表达式语法来设置值，同时仍保留不变性：

:::code language="csharp" source="snippets/whats-new-csharp9/WeatherObservation.cs" ID="UseWeatherObservation":::

初始化后尝试更改观察值会导致编译器错误：

```csharp
// Error! CS8852.
now.TemperatureInCelsius = 18;
```

对于从派生类设置基类属性，仅限 init 的资源库很有用。 它们还可通过基类中的帮助程序来设置派生属性。 位置记录使用仅限 init 的资源库声明属性。 这些设置器可在 with 表达式中使用。 可为定义的任何 `class`、`struct` 或 `record` 声明仅限 init 的资源库。

有关详细信息，请查看 [init（C# 参考）](../language-reference/keywords/init.md)。

## <a name="top-level-statements"></a>顶级语句

顶级语句从许多应用程序中删除了不必要的流程。 请考虑规范的“Hello World!” 程序：

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

只有一行代码执行所有操作。 借助顶级语句，可使用 `using` 指令和执行操作的一行替换所有样本：

:::code language="csharp" source="snippets/whats-new-csharp9/Program.cs" ID="TopLevelStatements":::

如果需要单行程序，可删除 `using` 指令，并使用完全限定的类型名称：

```csharp
System.Console.WriteLine("Hello World!");
```

应用程序中只有一个文件可使用顶级语句。 如果编译器在多个源文件中找到顶级语句，则是错误的。 如果将顶级语句与声明的程序入口点方法（通常为 `Main` 方法）结合使用，也会出现错误。 从某种意义上讲，可认为一个文件包含通常位于 `Program` 类的 `Main` 方法中的语句。  

此功能最常见的用途之一是创建材料。 C# 初级开发人员可以用一两行代码 编写规范的“Hello World!”。 不需要额外的工作。 不过，经验丰富的开发人员还会发现此功能的许多用途。 顶级语句可提供类似脚本的试验体验，这与 Jupyter 笔记本提供的很类似。 顶级语句非常适合小型控制台程序和实用程序。 [Azure Functions](/azure/azure-functions/) 是顶级语句的理想用例。

最重要的是，顶层语句不会限制应用程序的范围或复杂程度。 这些语句可访问或使用任何 .NET 类。 它们也不会限制你对命令行参数或返回值的使用。 顶级语句可访问名为 `args` 的字符串数组。 如果顶级语句返回整数值，则该值将成为来自合成 `Main` 方法的整数返回代码。 顶级语句可能包含异步表达式。 在这种情况下，合成入口点将返回 `Task` 或 `Task<int>`。

有关详细信息，请参阅 C# 编程指南中的[顶级语句](../programming-guide/main-and-command-args/top-level-statements.md)。

## <a name="pattern-matching-enhancements"></a>模式匹配增强功能

C# 9 包括新的模式匹配改进：

- 类型模式要求在变量是一种类型时匹配
- 带圆括号的模式强制或强调模式组合的优先级
- 联合 `and` 模式要求两个模式都匹配
- 析取 `or` 模式要求任一模式匹配
- 否定 `not` 模式要求模式不匹配
- 关系模式要求输入小于、大于、小于等于或大于等于给定常数。

这些模式丰富了模式的语法。 请考虑下列示例：

:::code language="csharp" source="snippets/whats-new-csharp9/PatternUtilities.cs" ID="IsLetterPattern":::

使用可选的括号来明确 `and` 的优先级高于 `or`：

:::code language="csharp" source="snippets/whats-new-csharp9/PatternUtilities.cs" ID="IsLetterOrSeparatorPattern":::

最常见的用途之一是用于 NULL 检查的新语法：

```csharp
if (e is not null)
{
    // ...
}
```

后面模式中的任何一种都可在允许使用模式的任何上下文中使用：`is` 模式表达式、`switch` 表达式、嵌套模式以及 `switch` 语句的 `case` 标签的模式。

有关详细信息，请查看[模式（C# 参考）](../language-reference/operators/patterns.md)。

有关详细信息，请参阅[模式](../language-reference/operators/patterns.md)一文中的[关系模式](../language-reference/operators/patterns.md#relational-patterns)和[逻辑模式](../language-reference/operators/patterns.md#logical-patterns)部分。

## <a name="performance-and-interop"></a>性能和互操作性

3 项新功能改进了对需要高性能的本机互操作性和低级别库的支持：本机大小的整数、函数指针和省略 `localsinit` 标志。

本机大小的整数 `nint` 和 `nuint` 是整数类型。 它们由基础类型 <xref:System.IntPtr?displayProperty=nameWithType> 和 <xref:System.UIntPtr?displayProperty=nameWithType> 表示。 编译器将这些类型的其他转换和操作作为本机整数公开。 本机大小的整数定义 `MaxValue` 或 `MinValue` 的属性。 这些值不能表示为编译时编译时，因为它们取决于目标计算机上整数的本机大小。 这些值在运行时是只读的。 可在以下范围内对 `nint` 使用常量值：[`int.MinValue` .. `int.MaxValue`]. 可在以下范围内对 `nuint` 使用常量值：[`uint.MinValue` .. `uint.MaxValue`]. 编译器使用 <xref:System.Int32?displayProperty=nameWithType> 和 <xref:System.UInt32?displayProperty=nameWithType> 类型为所有一元和二元运算符执行常量折叠。 如果结果不满足 32 位，操作将在运行时执行，且不会被视为常量。 在广泛使用整数数学且需要尽可能快的性能的情况下，本机大小的整数可提高性能。 有关详细信息，请参阅 [`nint` 和 `nuint` 类型](../language-reference/builtin-types/nint-nuint.md)

函数指针提供了一种简单的语法来访问 IL 操作码 `ldftn` 和 `calli`。 可使用新的 `delegate*` 语法声明函数指针。 `delegate*` 类型是指针类型。 调用 `delegate*` 类型会使用 `calli`，而不是使用在 `Invoke()` 方法上采用 `callvirt` 的委托。 从语法上讲，调用是相同的。 函数指针调用使用 `managed` 调用约定。 在 `delegate*` 语法后面添加 `unmanaged` 关键字，以声明想要 `unmanaged` 调用约定。 可使用 `delegate*` 声明中的属性来指定其他调用约定。 有关详细信息，请参阅[不安全代码和指针类型](../language-reference/unsafe-code.md)。

最后，可添加 <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute?displayProperty=nameWithType> 来指示编译器不要发出 `localsinit` 标志。 此标志指示 CLR 对所有局部变量进行零初始化。 从 1.0 开始，`localsinit` 标志一直是 C# 的默认行为。 但在某些情况下，额外的零初始化可能会对性能产生可衡量的影响， 特别是在使用 `stackalloc` 时。 在这些情况下，可添加 <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute>。 可将它添加到单个方法或属性中，或者添加到 `class`、`struct`、`interface`，甚至是模块中。 此属性不会影响 `abstract` 方法，它会影响为实现生成的代码。 有关详细信息，请参阅 [`SkipLocalsInit` 属性](../language-reference/attributes/general.md#skiplocalsinit-attribute)。

这些功能在某些情况下可提高性能。 仅应在采用前后对这些功能进行仔细的基准测试之后使用它们。 涉及本机大小整数的代码必须在使用不同整数大小的多个目标平台上进行测试。 其他功能需要不安全的代码。

## <a name="fit-and-finish-features"></a>调整和完成功能

还有其他很多功能有助于更高效地编写代码。 在 C# 9.0 中，已知创建对象的类型时，可在 [`new`](../language-reference/operators/new-operator.md) 表达式中省略该类型。 最常见的用法是在字段声明中：

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="WeatherStationField":::

当需要创建新对象作为参数传递给方法时，也可使用目标类型 `new`。 请考虑使用以下签名的 `ForecastFor()` 方法：

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="ForecastSignature":::

可按如下所示调用该方法：

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="TargetTypeNewArgument":::

此功能还有一个不错的用途是，将其与仅限 init 的属性组合使用来初始化新对象：

:::code language="csharp" source="snippets/whats-new-csharp9/FitAndFinish.cs" ID="InitWeatherStation":::

可使用 `return new();` 语句返回由默认构造函数创建的实例。

类似的功能可改进[条件表达式](../language-reference/operators/conditional-operator.md)的目标类型解析。 进行此更改后，两个表达式无需从一个隐式转换到另一个，而是都可隐式转换为目标类型。 你可能不会注意到此更改。 你会注意到，某些以前需要强制转换或无法编译的条件表达式现在可以正常工作。

从 C# 9.0 开始，可将 `static` 修饰符添加到 [Lambda 表达式](../language-reference/operators/lambda-expressions.md)或[匿名方法](../language-reference/operators/delegate-operator.md)。 静态 Lambda 表达式类似于 `static` 局部函数：静态 Lambda 或匿名方法无法捕获局部变量或实例状态。 `static` 修饰符可防止意外捕获其他变量。

协变返回类型为[重写](../language-reference/keywords/override.md)方法的返回类型提供了灵活性。 重写方法可返回从重写基方法的返回类型派生的类型。 这对于记录和其他支持虚拟克隆或工厂方法的类型很有用。

此外，[`foreach` 循环](../language-reference/keywords/foreach-in.md)将识别并使用扩展方法 `GetEnumerator`，否则将满足 `foreach` 模式。 此更改意味着 `foreach` 与其他基于模式的构造（例如异步模式和基于模式的析构）一致。 实际上，此更改意味着可以为任何类型添加 `foreach` 支持。 在设计中，应将其限制为在枚举对象有意义时使用。

接下来，可使用弃元作为 Lambda 表达式的参数。 这样可免于为参数命名，并且编译器也可避免使用它。 可将 `_` 用于任何参数。 有关详细信息，请参阅 [Lambda 表达式](../language-reference/operators/lambda-expressions.md)一文中的 [Lambda 表达式的输入参数](../language-reference/operators/lambda-expressions.md#input-parameters-of-a-lambda-expression)一节。

最后，现在可将属性应用于[本地函数](../programming-guide/classes-and-structs/local-functions.md)。 例如，可将[可为空的属性注释](../language-reference/attributes/nullable-analysis.md)应用于本地函数。

## <a name="support-for-code-generators"></a>支持代码生成器

最后两项功能支持 C# 代码生成器。 C# 代码生成器是可编写的组件，类似于 roslyn 分析器或代码修补程序。 区别在于，代码生成器会在编译过程中分析代码并编写新的源代码文件。 典型的代码生成器会在代码中搜索属性或其他约定。

代码生成器使用 Roslyn 分析 API 读取属性或其他代码元素。 通过该信息，它将新代码添加到编译中。 源生成器只能添加代码，不能修改编译中的任何现有代码。

为代码生成器添加的两项功能是“分部方法语法”和“模块初始化表达式”的扩展。 首先是对分部方法的更改。 在 C# 9.0 之前，分部方法为 `private`，但不能指定访问修饰符、不能返回 `void`，也不能具有 `out` 参数。 这些限制意味着，如果未提供任何方法实现，编译器会删除对分部方法的所有调用。 C# 9.0 消除了这些限制，但要求分部方法声明必须具有实现。 代码生成器可提供这种实现。 为了避免引入中断性变更，编译器会考虑没有访问修饰符的任何分部方法，以遵循旧规则。 如果分部方法包括 `private` 访问修饰符，则由新规则控制该分部方法。 有关详细信息，请查看[分部方法（C# 参考）](../language-reference/keywords/partial-method.md)。

代码生成器的第二项新功能是模块初始化表达式。 模块初始化表达式是附加了 <xref:System.Runtime.CompilerServices.ModuleInitializerAttribute> 属性的方法。 在整个模块中进行任何其他字段访问或方法调用之前，运行时将调用这些方法。 模块初始化表达式方法：

- 必须是静态的
- 必须没有参数
- 必须返回 void
- 不能是泛型方法
- 不能包含在泛型类中
- 必须能够从包含模块访问

最后一个要点实际上意味着该方法及其包含类必须是内部的或公共的。 方法不能为本地函数。 有关详细信息，请参阅 [`ModuleInitializer` 属性](../language-reference/attributes/general.md#moduleinitializer-attribute)。
