---
title: C# 9.0 中的新增功能 - C# 指南
description: 简要介绍 C# 9.0 中提供的新功能。
ms.date: 09/04/2020
ms.openlocfilehash: 49170b123f612c06f22b70e44b29ad7be5f382ea
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876040"
---
# <a name="whats-new-in-c-90"></a>C# 9.0 中的新增功能

C# 9.0 向 C# 语言添加了以下功能和增强功能：

- [记录](#record-types)
- [仅限 Init 的资源库](#init-only-setters)
- [顶级语句](#top-level-statements)
- [模式匹配增强功能](#pattern-matching-enhancements)
- 本机大小的整数
- 函数指针
- 禁止发出 localsinit 标志
- 目标类型的新表达式
- 静态匿名函数
- 目标类型的条件表达式
- 协变返回类型
- 扩展 `GetEnumerator` 支持 `foreach` 循环
- Lambda 弃元参数
- 本地函数的属性
- 模块初始值设定项
- 分部方法的新功能

.NET 5 支持 C# 9.0。 有关详细信息，请参阅 [C# 语言版本控制](../language-reference/configure-language-version.md)。

可以从 [.NET 下载页](https://dotnet.microsoft.com/download)下载最新 .NET SDK。

## <a name="record-types"></a>记录类型

C# 9.0 引入了记录类型，这是一种引用类型，它提供合成方法来提供值语义，从而实现相等性。 默认情况下，记录是不可变的。

使用记录类型可在 .NET 中轻松创建不可变的引用类型。 以前，.NET 类型主要分为引用类型（包括类和匿名类型）和值类型（包括结构和元组）。 虽然建议使用不可变的值类型，但可变的值类型通常不会引入错误。 值类型变量可保存值，因此在将值类型传递给方法时，会对原始数据的副本进行更改。

不可变的引用类型也有许多优点。 这些优点在使用共享数据的并发程序中更为明显。 遗憾的是，C# 强制编写大量额外的代码来创建不可变的引用类型。 记录为不可变的引用类型提供类型声明，该引用类型使用值语义实现相等性。 如果用于实现相等性的合成方法的属性和哈希代码的属性都相等，则认为两条记录相等。 请考虑以下定义：

:::code language="csharp" source="snippets/whats-new-csharp9/RecordsExamples.cs" ID="RecordDefinition":::

记录定义会创建一个包含两个只读属性（`FirstName` 和 `LastName`）的 `Person` 类型。 `Person` 类型是引用类型。 如果查看 IL，它就是一个类。 它是不可变的，因为在创建它后，无法修改任何属性。 定义记录类型时，编译器会合成其他几种方法：

- 基于值的相等性比较方法
- 替代 <xref:System.Object.GetHashCode>
- 复制和克隆成员
- `PrintMembers` 和 <xref:System.Object.ToString>

记录支持继承。 可声明派生自 `Person` 的新记录，如下所示：

:::code language="csharp" source="snippets/whats-new-csharp9/RecordsExamples.cs" ID="InheritedRecord":::

还可密封记录以防止进一步派生：

:::code language="csharp" source="snippets/whats-new-csharp9/RecordsExamples.cs" ID="SealedRecord":::

编译器会合成上述方法的不同版本。 方法签名取决于记录类型是否密封以及直接基类是否为对象。 记录应具有以下功能：

- 相等性是基于值的，包括检查类型是否匹配。 例如，即使两条记录的名称相同，`Student` 也不能等于 `Person`。
- 记录具有为你生成的一致的字符串表示形式。
- 记录支持副本构造。 正确的副本构造必须包括继承层次结构和开发人员添加的属性。
- 可通过修改复制记录。 这些复制和修改操作支持非破坏性转变。

除了熟悉的 `Equals` 重载、`operator ==` 和 `operator !=` 外，编译器还会合成新的 `EqualityContract` 属性。 该属性返回与记录类型匹配的 `Type` 对象。 如果基类型为 `object`，则属性为 `virtual`。 如果基类型是其他记录类型，则属性为 `override`。 如果记录类型为 `sealed`，则属性为 `sealed`。 合成的 `GetHashCode` 使用基类型和记录类型中声明的所有属性和字段中的 `GetHashCode`。 这些合成方法在整个继承层次结构中强制执行基于值的相等性。 这意味着，绝不会将 `Student` 视为与同名的 `Person` 相等。 两条记录的类型必须匹配，而且记录类型之间共享的所有属性也必须相等。

记录还具有合成的构造函数和用于创建副本的“克隆”方法。 合成的构造函数有一个记录类型的参数。 该函数会为记录的所有属性生成具有相同值的新记录。 如果记录是密封的，则此构造函数是专用函数；否则它将受到保护。 合成的“克隆”方法支持用于记录层次结构的副本构造。 “克隆”一词用引号引起来，因为实际名称是编译器生成的。 无法在记录类型中创建名为 `Clone` 的方法。 合成的“克隆”方法返回使用虚拟调度复制的记录类型。 编译器根据 `record` 上的访问修饰符为“克隆”方法添加不同的修饰符：

- 如果记录类型为 `abstract`，则“克隆”方法也为 `abstract`。 如果基类型不是 `object`，则方法也是 `override`。
- 当基类型为 `object` 时，对于不是 `abstract` 的记录类型：
  - 如果记录为 `sealed`，则不向“克隆”方法添加其他修饰符（这意味着它不是 `virtual`）。
  - 如果记录不是 `sealed`，则“克隆”方法为 `virtual`。
- 当基类型不是 `object` 时，对于不是 `abstract` 的记录类型：
  - 如果记录是 `sealed`，则“克隆”方法也是 `sealed`。
  - 如果记录不是 `sealed`，则“克隆”方法为 `override`。

所有这些规则的结果都是，跨记录类型的任何层次结构一致地实现了相等性。 如果两条记录的属性相等且类型相同，则它们彼此相等，如下例所示：

:::code language="csharp" source="snippets/whats-new-csharp9/RecordsExamples.cs" ID="RecordsEquality":::

编译器合成了两种支持打印输出的方法：<xref:System.Object.ToString> 替代和 `PrintMembers`。 `PrintMembers` 采用 <xref:System.Text.StringBuilder?displayProperty=nameWithType> 作为其参数。 它对记录类型中的所有属性追加一个用逗号分隔的属性名称和值的列表。 `PrintMembers` 会调用派生自其他记录的任何记录的基本实现。 <xref:System.Object.ToString> 替代会返回由 `PrintMembers` 生成的字符串，并将其括在 `{` 和 `}` 内。 例如，`Student` 的 <xref:System.Object.ToString> 方法返回一个 `string`，类似于以下代码：

```csharp
"Student { LastName = Wagner, FirstName = Bill, Level = 11 }"
```

截至目前显示的示例都使用传统语法声明属性。 还有一种更简洁的格式，称为“位置记录”。  下面是先前定义为位置记录的 3 种记录类型：

:::code language="csharp" source="snippets/whats-new-csharp9/PositionalRecords.cs" ID="PositionalRecords":::

这些声明创建的功能与早期版本相同（以下部分介绍了几项额外的功能）。 这些声明以分号而不是方括号结尾，因为这些记录没有添加其他方法。 可添加正文，还可包括其他任何方法：

:::code language="csharp" source="snippets/whats-new-csharp9/PositionalRecords.cs" ID="RecordsWithMethods":::

编译器为位置记录生成 `Deconstruct` 方法。 `Deconstruct` 方法的参数与记录类型中所有公共属性的名称匹配。 `Deconstruct` 方法可用于将记录析构为其组件属性：

:::code language="csharp" source="snippets/whats-new-csharp9/PositionalRecords.cs" ID="DeconstructRecord":::

最后，记录支持 [`with` 表达式](../language-reference/operators/with-expression.md)。 `with` 表达式指示编译器创建一个记录的副本，但要修改指定的属性：

:::code language="csharp" source="snippets/whats-new-csharp9/PositionalRecords.cs" ID="Wither":::

上一行创建新的 `Person` 记录，其中 `LastName` 属性是 `person` 的副本，`FirstName` 为 `"Paul"`。 可在 `with` 表达式中设置任意数量的属性。 还可以使用 `with` 表达式来创建精确的副本。 为要修改的属性指定空集：

:::code language="csharp" source="snippets/whats-new-csharp9/PositionalRecords.cs" ID="WithCopy":::

你可编写除“克隆”方法以外的任何合成成员。 如果记录类型的方法与任何合成方法的签名匹配，则编译器不会合成该方法。 较早的 `Dog` 记录示例包含手动编码的 <xref:System.String.ToString> 方法作为示例。

了解有关此[浏览记录](../whats-new/tutorials/records.md)教程中记录类型的详细信息。

## <a name="init-only-setters"></a>仅限 Init 的资源库

仅限 init 的资源库提供一致的语法来初始化对象的成员。 属性初始值设定项可明确哪个值正在设置哪个属性。 缺点是这些属性必须是可设置的。 从 C# 9.0 开始，可为属性和索引器创建 `init` 访问器，而不是 `set` 访问器。 调用方可使用属性初始化表达式语法在创建表达式中设置这些值，但构造完成后，这些属性将变为只读。 仅限 init 的资源库提供了一个窗口用来更改状态。 构造阶段结束时，该窗口关闭。 在完成所有初始化（包括属性初始化表达式和 with 表达式）之后，构造阶段实际上就结束了。

可在编写的任何类型中声明仅限 `init` 的资源库。 例如，以下结构定义了天气观察结构：

:::code language="csharp" source="snippets/whats-new-csharp9/WeatherObservation.cs" ID="DeclareWeatherObservation":::

调用方可使用属性初始化表达式语法来设置值，同时仍保留不变性：

:::code language="csharp" source="snippets/whats-new-csharp9/WeatherObservation.cs" ID="UseWeatherObservation":::

但在初始化后更改观察值是错误的，它会在初始化之外分配给仅限 init 的属性：

```csharp
// Error! CS8852.
now.TemperatureInCelsius = 18;
```

对于从派生类设置基类属性，仅限 init 的资源库很有用。 它们还可通过基类中的帮助程序来设置派生属性。 位置记录使用仅限 init 的资源库声明属性。 这些设置器可在 with 表达式中使用。 可为定义的任何 `class` 或 `struct` 声明仅限 init 的资源库。

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

只有一行代码执行所有操作。 借助顶级语句，可使用 `using` 语句和执行操作的一行替换所有样本：

:::code language="csharp" source="snippets/whats-new-csharp9/Program.cs" ID="TopLevelStatements":::

如果需要单行程序，可删除 `using` 指令，并使用完全限定的类型名称：

```csharp
System.Console.WriteLine("Hello World!");
```

应用程序中只有一个文件可使用顶级语句。 如果编译器在多个源文件中找到顶级语句，则是错误的。 如果将顶级语句与声明的程序入口点方法（通常为 `Main` 方法）结合使用，也会出现错误。 从某种意义上讲，可认为一个文件包含通常位于 `Program` 类的 `Main` 方法中的语句。  

此功能最常见的用途之一是创建材料。 C# 初级开发人员可以用一两行代码 编写规范的“Hello World!”。 不需要额外的工作。 不过，经验丰富的开发人员还会发现此功能的许多用途。 顶级语句可提供类似脚本的试验体验，这与 Jupyter 笔记本提供的很类似。 顶级语句非常适合小型控制台程序和实用程序。 Azure Functions 是顶级语句的理想用例。

最重要的是，顶层语句不会限制应用程序的范围或复杂程度。 这些语句可访问或使用任何 .NET 类。 它们也不会限制你对命令行参数或返回值的使用。 顶级语句可访问名为 args 的字符串数组。 如果顶级语句返回整数值，则该值将成为来自合成 `Main` 方法的整数返回代码。 顶级语句可能包含异步表达式。 在这种情况下，合成入口点将返回 `Task` 或 `Task<int>`。

## <a name="pattern-matching-enhancements"></a>模式匹配增强功能

C# 9 包括新的模式匹配改进：

- 类型模式要求在变量是一种类型时匹配
- 带圆括号的模式强制或强调模式组合的优先级
- 联合 `and` 模式要求两个模式都匹配
- 析取 `or` 模式要求任一模式匹配
- 求反 `not` 模式要求模式不匹配
- 关系模式要求输入小于、大于、小于等于或大于等于给定常数。

这些模式丰富了模式的语法。 请考虑下列示例：

:::code language="csharp" source="snippets/whats-new-csharp9/PatternUtilities.cs" ID="IsLetterPattern":::

还可使用可选的括号来明确 `and` 的优先级高于 `or`：

:::code language="csharp" source="snippets/whats-new-csharp9/PatternUtilities.cs" ID="IsLetterOrSeparatorPattern":::

最常见的用途之一是用于 NULL 检查的新语法：

```csharp
if (e is not null)
{
    // ...
}
```

这些模式中的任何一种都可在允许使用模式的任何上下文中使用：`is` 模式表达式、`switch` 表达式、嵌套模式以及 `switch` 语句的 `case` 标签的模式。

## <a name="performance-and-interop"></a>性能和互操作性

3 项新功能改进了对需要高性能的本机互操作性和低级别库的支持：本机大小的整数、函数指针和省略 `localsinit` 标志。

本机大小的整数 `nint` 和 `nuint` 是整数类型。 它们由基础类型 <xref:System.IntPtr?displayProperty=nameWithType> 和 <xref:System.UIntPtr?displayProperty=nameWithType> 表示。 编译器将这些类型的其他转换和操作作为本机整数公开。 本机大小的整数定义 `MaxValue` 或 `MinValue` 的属性。 这些值不能表示为编译时编译时，因为它取决于目标计算机上整数的本机大小。 这些值在运行时是只读的。 可在以下范围内对 `nint` 使用常量值：[`int.MinValue` .. `int.MaxValue`]. 可在以下范围内对 `nuint` 使用常量值：[`uint.MinValue` .. `uint.MaxValue`]. 编译器使用 <xref:System.Int32?displayProperty=nameWithType> 和 <xref:System.UInt32?displayProperty=nameWithType> 类型为所有一元和二元运算符执行常量折叠。 如果结果不满足 32 位，操作将在运行时执行，且不会被视为常量。 在广泛使用整数数学且需要尽可能快的性能的情况下，本机大小的整数可提高性能。

函数指针提供了一种简单的语法来访问 IL 操作码 `ldftn` 和 `calli`。 可使用新的 `delegate*` 语法声明函数指针。 `delegate*` 类型是指针类型。 调用 `delegate*` 类型会使用 `calli`，而不是使用在 `Invoke()` 方法上采用 `callvirt` 的委托。 从语法上讲，调用是相同的。 函数指针调用使用 `managed` 调用约定。 在 `delegate*` 语法后面添加 `unmanaged` 关键字，以声明想要 `unmanaged` 调用约定。 可使用 `delegate*` 声明中的属性来指定其他调用约定。

最后，可添加 <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute?displayProperty=nameWithType> 来指示编译器不要发出 `localsinit` 标志。 此标志指示 CLR 对所有局部变量进行零初始化。 从 1.0 开始，`localsinit` 标志一直是 C# 的默认行为。 但在某些情况下，额外的零初始化可能会对性能产生可衡量的影响， 特别是在使用 `stackalloc` 时。 在这些情况下，可添加 <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute>。 可将它添加到单个方法或属性中，或者添加到 `class`、`struct`、`interface`，甚至是模块中。 此属性不会影响 `abstract` 方法，它会影响为实现生成的代码。

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

为代码生成器添加的两项功能是“分部方法语法”和“模块初始化表达式”的扩展。 首先是对分部方法的更改。 在 C# 9.0 之前，分部方法为 `private`，但不能指定访问修饰符、不能返回 `void`，也不能具有 `out` 参数。 这些限制意味着，如果未提供任何方法实现，编译器会删除对分部方法的所有调用。 C# 9.0 消除了这些限制，但要求分部方法声明必须具有实现。 代码生成器可提供这种实现。 为了避免引入中断性变更，编译器会考虑没有访问修饰符的任何分部方法，以遵循旧规则。 如果分部方法包括 `private` 访问修饰符，则由新规则控制该分部方法。

代码生成器的第二项新功能是模块初始化表达式。 模块初始化表达式是附加了 <xref:System.Runtime.CompilerServices.ModuleInitializerAttribute> 属性的方法。 在整个模块中进行任何其他字段访问或方法调用之前，运行时将调用这些方法。 模块初始化表达式方法：

- 必须是静态的
- 必须没有参数
- 必须返回 void
- 不能是泛型方法
- 不能包含在泛型类中
- 必须能够从包含模块访问

最后一个要点实际上意味着该方法及其包含类必须是内部的或公共的。 方法不能为本地函数。
