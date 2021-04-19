---
title: C# 7.0 中的新增功能 - C# 指南
description: 大致了解 C# 语言的版本 7.0 中的新增功能。
ms.date: 10/02/2020
ms.assetid: fd41596d-d0c2-4816-b94d-c4d00a5d0243
ms.openlocfilehash: 0c57a030497d13b783960152612de27d5fc73c3c
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255020"
---
# <a name="whats-new-in-c-70-through-c-73"></a>C# 7.0 - C# 7.3 中的新增功能

C# 7.0 - C# 7.3 为 C# 开发体验带来了大量功能和增量改进。 本文概述了新的语言功能和编译器选项。 说明中描述了 C# 7.3 的行为，C# 7.3 是基于 .NET Framework 的应用程序支持的最新版本。

C# 7.1 中添加了[语言版本选择](../language-reference/configure-language-version.md)配置元素，因此你可以在项目文件中指定编译器语言版本。

C# 7.0-7.3 将这些功能和主题添加到了 C# 语言中：

- [元组和弃元](#tuples-and-discards)
  - 可以创建包含多个公共字段的轻量级未命名类型。 编译器和 IDE 工具可理解这些类型的语义。
  - 弃元是指在不关心所赋予的值时，赋值中使用的临时只写变量。 在对元组和用户定义类型进行解构，以及在使用 `out` 参数调用方法时，它们的作用最大。
- [模式匹配](#pattern-matching)
  - 可以基于任意类型和这些类型的成员的值创建分支逻辑。
- [`async` `Main` 方法](#async-main)
  - 应用程序的入口点可以含有 `async` 修饰符。
- [本地函数](#local-functions)
  - 可以将函数嵌套在其他函数内，以限制其范围和可见性。
- [更多的 expression-bodied 成员](#more-expression-bodied-members)
  - 可使用表达式创作的成员列表有所增长。
- [`throw` 表达式](#throw-expressions)
  - 可以在之前因为 `throw` 是语句而不被允许的代码构造中引发异常。
- [`default` 文本表达式](#default-literal-expressions)
  - 在可以推断目标类型的情况下，可在默认值表达式中使用默认文本表达式。
- [数字文本语法改进](#numeric-literal-syntax-improvements)
  - 新令牌可提高数值常量的可读性。
- [`out` 变量](#out-variables)
  - 可以将 `out` 值内联作为参数声明到使用这些参数的方法中。
- [非尾随命名参数](#non-trailing-named-arguments)
  - 命名的参数可后接位置参数。
- [`private protected` 访问修饰符](#private-protected-access-modifier)
  - `private protected` 访问修饰符允许访问同一程序集中的派生类。
- [改进了重载解析](#improved-overload-candidates)
  - 用于解决重载解析歧义的新规则。
- [编写安全高效代码的技巧](#enabling-more-efficient-safe-code)
  - 结合了多项语法改进，可使用引用语义处理值类型。

最后，编译器中添加了新的选项：

- 控制[引用程序集生成](#reference-assembly-generation)的 `-refout` 和 `-refonly`。
- `-publicsign`，用于启用程序集的开放源代码软件 (OSS) 签名。
- `-pathmap`用于提供源目录的映射。

本文的其余部分概述了每个功能。 你将了解每项功能背后的原理和语法。 可以使用 `dotnet try` 全局工具在环境中浏览这些功能：

1. 安装 [dotnet-try](https://github.com/dotnet/try/blob/main/DotNetTryLocal.md) 全局工具。
1. 克隆 [dotnet/try-samples](https://github.com/dotnet/try-samples) 存储库。
1. 将当前目录设置为 try-samples 存储库的 csharp7 子目录 。
1. 运行 `dotnet try`。

## <a name="tuples-and-discards"></a>元组和弃元

C# 为用于说明设计意图的类和结构提供了丰富的语法。 但是，这种丰富的语法有时会需要额外的工作，但益处却很少。 你可能经常编写需要包含多个数据元素的简单结构的方法。 为了支持这些方案，已将元组添加到了 C#。 元组是包含多个字段以表示数据成员的轻量级数据结构。 这些字段没有经过验证，并且你无法定义自己的方法。 C# 元组类型支持 `==` 和 `!=`。 有关详细信息，

> [!NOTE]
> 低于 C# 7.0 的版本中也提供元组，但它们效率低下且不具有语言支持。 这意味着元组元素只能作为 `Item1` 和 `Item2` 等引用。 C# 7.0 引入了对元组的语言支持，可利用更有效的新元组类型向元组字段赋予语义名称。

可以通过为每个成员赋值来创建元组，并可选择为元组的每个成员提供语义名称：

[!code-csharp[NamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#NamedTuple "Named tuple")]

`namedLetters` 元组包含称为 `Alpha` 和 `Beta` 的字段。 这些名称仅存在于编译时且不保留，例如在运行时使用反射来检查元组时。

在进行元组赋值时，还可以指定赋值右侧的字段的名称：

[!code-csharp[ImplicitNamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#ImplicitNamedTuple "Implicitly named tuple")]

在某些时候，你可能想要解包从方法返回的元组的成员。  可通过为元组中的每个值声明单独的变量来实现此目的。 这种解包操作称为解构元组：

[!code-csharp[CallingWithDeconstructor](~/samples/snippets/csharp/new-in-7/program.cs#CallingWithDeconstructor "Deconstructing a tuple")]

还可以为 .NET 中的任何类型提供类似的析构。 编写 `Deconstruct` 方法，用作类的成员。 `Deconstruct` 方法为你要提取的每个属性提供一组 `out` 参数。 考虑提供析构函数方法的此 `Point` 类，该方法提取 `X` 和 `Y` 坐标：

[!code-csharp[PointWithDeconstruction](~/samples/snippets/csharp/new-in-7/point.cs#PointWithDeconstruction "Point with deconstruction method")]

可以通过向元组分配 `Point` 来提取各个字段：

[!code-csharp[DeconstructPoint](~/samples/snippets/csharp/new-in-7/program.cs#DeconstructPoint "Deconstruct a point")]

在初始化元组时，许多时候，赋值操作右侧的变量名与用于元组元素的名称相同：元组元素的名称可通过用于初始化元组的变量进行推断：

```csharp
int count = 5;
string label = "Colors used in the map";
var pair = (count, label); // element names are "count" and "label"
```

若要详细了解此功能，可以参阅[元组类型](../language-reference/builtin-types/value-tuples.md)一文。

通常，在进行元组解构或使用 `out` 参数调用方法时，必须定义一个其值无关紧要且你不打算使用的变量。 为处理此情况，C# 增添了对弃元的支持。 弃元是一个名为 `_`（下划线字符）的只写变量，可向单个变量赋予要放弃的所有值。 弃元类似于未赋值的变量；不可在代码中使用弃元（赋值语句除外）。

在以下方案中支持弃元：

- 在对元组或用户定义的类型进行解构时。
- 在使用 [out](../language-reference/keywords/out-parameter-modifier.md) 参数调用方法时。
- 在使用 [is](../language-reference/keywords/is.md) 和 [switch](../language-reference/keywords/switch.md) 语句匹配操作的模式中。
- 在要将某赋值的值显式标识为弃元时用作独立标识符。

以下示例定义了 `QueryCityDataForYears` 方法，它返回一个包含两个不同年份的城市数据的六元组。 本例中，方法调用仅与此方法返回的两个人口值相关，因此在进行元组解构时，将元组中的其余值视为弃元。

[!code-csharp[Tuple-discard](~/samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

有关详细信息，请参阅[弃元](../discards.md)。

## <a name="pattern-matching"></a>模式匹配

模式匹配是一组功能，利用这些功能，你可以通过新的方式在代码中表示控制流。 你可以测试变量的类型、值或其属性的值。 这些方法可创建可读性更佳的代码流。

模式匹配支持 `is` 表达式和 `switch` 表达式。 每个表达式都允许检查对象及其属性以确定该对象是否满足所寻求的模式。 使用 `when` 关键字来指定模式的其他规则。

`is` 模式表达式扩展了常用 [`is` 运算符](../language-reference/keywords/is.md#pattern-matching-with-is)以查询关于其类型的对象，并在一条指令分配结果。 以下代码检查变量是否为 `int`，如果是，则将其添加到当前总和：

```csharp
if (input is int count)
    sum += count;
```

前面的小型示例演示了 `is` 表达式的增强功能。 可以针对值类型和引用类型进行测试，并且可以将成功结果分配给类型正确的新变量。

switch 匹配表达式具有常见的语法，它基于已包含在 C# 语言中的 `switch` 语句。 更新后的 switch 语句有几个新构造：

- `switch` 表达式的控制类型不再局限于整数类型、`Enum` 类型、`string` 或与这些类型之一对应的可为 null 的类型。 可能会使用任何类型。
- 可以在每个 `case` 标签中测试 `switch` 表达式的类型。 与 `is` 表达式一样，可以为该类型指定一个新变量。
- 可以添加 `when` 子句以进一步测试该变量的条件。
- `case` 标签的顺序现在很重要。 执行匹配的第一个分支；其他将跳过。

以下代码演示了这些新功能：

```csharp
public static int SumPositiveNumbers(IEnumerable<object> sequence)
{
    int sum = 0;
    foreach (var i in sequence)
    {
        switch (i)
        {
            case 0:
                break;
            case IEnumerable<int> childSequence:
            {
                foreach(var item in childSequence)
                    sum += (item > 0) ? item : 0;
                break;
            }
            case int n when n > 0:
                sum += n;
                break;
            case null:
                throw new NullReferenceException("Null found in sequence");
            default:
                throw new InvalidOperationException("Unrecognized type");
        }
    }
    return sum;
}
```

- `case 0:` 是[常量模式](../language-reference/operators/patterns.md#constant-pattern)。
- `case IEnumerable<int> childSequence:` 是[声明模式](../language-reference/operators/patterns.md#declaration-and-type-patterns)。
- `case int n when n > 0:` 是具有附加 `when` 条件的声明模式。
- `case null:` 是 `null` 常量模式。
- `default:` 是常见的默认事例。

自 C# 7.1 起，`is` 和 `switch` 类型模式的模式表达式的类型可能为泛型类型参数。 这可能在检查 `struct` 或 `class` 类型且要避免装箱时最有用。

可以在 [C# 中的模式匹配](../pattern-matching.md)中了解有关模式匹配的更多信息。

## <a name="async-main"></a>异步 `main` 方法

*异步 Main* 方法使你能够在 `Main` 方法中使用 `await` 关键字。 在过去，需要编写：

```csharp
static int Main()
{
    return DoAsyncWork().GetAwaiter().GetResult();
}
```

现在，您可以编写：

```csharp
static async Task<int> Main()
{
    // This could also be replaced with the body
    // DoAsyncWork, including its await expressions:
    return await DoAsyncWork();
}
```

如果程序不返回退出代码，可以声明返回 <xref:System.Threading.Tasks.Task> 的 `Main` 方法:

```csharp
static async Task Main()
{
    await SomeAsyncMethod();
}
```

如需了解更多详情，可以阅读编程指南中的[异步 Main](../programming-guide/main-and-command-args/index.md) 一文。

## <a name="local-functions"></a>本地函数

许多类的设计都包括仅从一个位置调用的方法。 这些额外的私有方法使每个方法保持小且集中。 本地函数使你能够在另一个方法的上下文内声明方法。 本地函数使得类的阅读者更容易看到本地方法仅从声明它的上下文中调用。

对于本地函数有两个常见的用例：公共迭代器方法和公共异步方法。 这两种类型的方法都生成报告错误的时间晚于程序员期望时间的代码。 在迭代器方法中，只有在调用枚举返回的序列的代码时才会观察到任何异常。 在异步方法中，只有当返回的 `Task` 处于等待状态时才会观察到任何异常。 以下示例演示如何使用本地函数将参数验证与迭代器实现分离：

[!code-csharp[22_IteratorMethodLocal](~/samples/snippets/csharp/new-in-7/Iterator.cs#IteratorMethodLocal "Iterator method with local function")]

可以对 `async` 方法采用相同的技术，以确保在异步工作开始之前引发由参数验证引起的异常：

[!code-csharp[TaskExample](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#TaskExample "Task returning method with local function")]

现在支持此语法：

```csharp
[field: SomeThingAboutFieldAttribute]
public int SomeProperty { get; set; }
```

属性 `SomeThingAboutFieldAttribute` 应用于编译器生成的 `SomeProperty` 的支持字段。 有关详细信息，请参阅 C# 编程指南中的[属性](../programming-guide/concepts/attributes/index.md)。

> [!NOTE]
> 本地函数支持的某些设计也可以使用 lambda 表达式来完成。 有关详细信息，请参阅[本地函数与 Lambda 表达式](../programming-guide/classes-and-structs/local-functions.md#local-functions-vs-lambda-expressions)。

## <a name="more-expression-bodied-members"></a>更多的 expression-bodied 成员

C# 6 为成员函数和只读属性引入了 expression-bodied 成员。 C# 7.0 扩展了可作为表达式实现的允许的成员。 在 C# 7.0 中，你可以在属性和索引器上实现构造函数、终结器以及 `get` 和 `set` 访问器。 以下代码演示了每种情况的示例：

[!code-csharp[ExpressionBodiedMembers](~/samples/snippets/csharp/new-in-7/expressionmembers.cs#ExpressionBodiedEverything "new expression-bodied members")]

> [!NOTE]
> 本示例不需要终结器，但显示它是为了演示语法。 不应在类中实现终结器，除非有必要发布非托管资源。 还应考虑使用 <xref:System.Runtime.InteropServices.SafeHandle> 类，而不是直接管理非托管资源。

这些 expression-bodied 成员的新位置代表了 C# 语言的一个重要里程碑：这些功能由致力于开发开放源代码 [Roslyn](https://github.com/dotnet/Roslyn) 项目的社区成员实现。

将方法更改为 expression bodied 成员是[二进制兼容的更改](version-update-considerations.md#binary-compatible-changes)。

## <a name="throw-expressions"></a>引发表达式

在 C# 中，`throw` 始终是一个语句。 因为 `throw` 是一个语句而非表达式，所以在某些 C# 构造中无法使用它。 它们包括条件表达式、null 合并表达式和一些 lambda 表达式。 添加 expression-bodied 成员将添加更多位置，在这些位置中，`throw` 表达式会很有用。 为了可以编写这些构造，C# 7.0 引入了 [*throw 表达式*](../language-reference/keywords/throw.md#the-throw-expression)。

这使得编写更多基于表达式的代码变得更容易。 不需要其他语句来进行错误检查。

## <a name="default-literal-expressions"></a>默认文本表达式

默认文本表达式是针对默认值表达式的一项增强功能。 这些表达式将变量初始化为默认值。 过去会这么编写：

```csharp
Func<string, bool> whereClause = default(Func<string, bool>);
```

现在，可以省略掉初始化右侧的类型：

```csharp
Func<string, bool> whereClause = default;
```

有关详细信息，请参阅 [default 运算符](../language-reference/operators/default.md)一文中的 [default 文本](../language-reference/operators/default.md#default-literal)部分。

## <a name="numeric-literal-syntax-improvements"></a>数字文本语法改进

误读的数值常量可能使第一次阅读代码时更难理解。 位掩码或其他符号值容易产生误解。 C# 7.0 包括两项新功能，可用于以最可读的方式写入数字来用于预期用途：二进制文本和数字分隔符 。

在创建位掩码时，或每当数字的二进制表示形式使代码最具可读性时，以二进制形式写入该数字：

[!code-csharp[ThousandSeparators](~/samples/snippets/csharp/new-in-7/Program.cs#ThousandSeparators "Thousands separators")]

常量开头的 `0b` 表示该数字以二进制数形式写入。 二进制数可能会很长，因此通过引入 `_` 作为数字分隔符通常更易于查看位模式，如前面示例中的二进制常量所示。 数字分隔符可以出现在常量的任何位置。 对于十进制数字，通常将其用作千位分隔符。 十六进制和二进制文本可采用 `_` 开头：

[!code-csharp[LargeIntegers](~/samples/snippets/csharp/new-in-7/Program.cs#LargeIntegers "Large integer")]

数字分隔符也可以与 `decimal`、`float` 和 `double` 类型一起使用：

[!code-csharp[OtherConstants](~/samples/snippets/csharp/new-in-7/Program.cs#OtherConstants "non-integral constants")]

综观来说，你可以声明可读性更强的数值常量。

## <a name="out-variables"></a>`out` 变量

支持 `out` 参数的现有语法已在 C# 7 中得到改进。 现在可以在方法调用的参数列表中声明 `out` 变量，而不是编写单独的声明语句：

[!code-csharp[OutVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVariableDeclarations "Out variable declarations")]

为清楚起见，我们建议你指定 `out` 变量的类型，如前面的示例所示。 但是，该语言支持使用隐式类型的局部变量：

[!code-csharp[OutVarVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVarVariableDeclarations "Implicitly typed Out variable")]

- 代码更易于阅读。
  - 在使用 out 变量的地方声明 out 变量，而不是在上面的代码行中声明。
- 无需分配初始值。
  - 通过在方法调用中使用 `out` 变量的位置声明该变量，使得在分配它之前不可能意外使用它。

已对在 C# 7.0 中添加的允许 `out` 变量声明的语法进行了扩展，以包含字段初始值设定项、属性初始值设定项、构造函数初始值设定项和查询子句。 它允许使用如以下示例中所示的代码：

```csharp
public class B
{
   public B(int i, out int j)
   {
      j = i;
   }
}

public class D : B
{
   public D(int i) : base(i, out var j)
   {
      Console.WriteLine($"The value of 'j' is {j}");
   }
}
```

## <a name="non-trailing-named-arguments"></a>非尾随命名参数

方法调用现可使用位于位置参数前面的命名参数（若这些命名参数的位置正确）。 如需了解详情，请参阅[命名参数和可选参数](../programming-guide/classes-and-structs/named-and-optional-arguments.md)。

## <a name="private-protected-access-modifier"></a>*private protected* 访问修饰符

新的复合访问修饰符：`private protected` 指示可通过包含同一程序集中声明的类或派生类来访问成员。 虽然 `protected internal` 允许通过同一程序集中的类或派生类进行访问，但 `private protected` 限制对同一程序集中声明的派生类的访问。

如需了解详情，请参阅语言参考中的[访问修饰符](../language-reference/keywords/access-modifiers.md)。

## <a name="improved-overload-candidates"></a>改进了重载候选项

在每个版本中，对重载解析规则进行了更新，以解决多义方法调用具有“明显”选择的情况。 此版本添加了三个新规则，以帮助编译器选取明显的选择：

1. 当方法组同时包含实例和静态成员时，如果方法在不含实例接收器或上下文的情况下被调用，则编译器将丢弃实例成员。 如果方法在含有实例接收器的情况下被调用，则编译器将丢弃静态成员。 在没有接收器时，编译器将仅添加静态上下文中的静态成员，否则，将同时添加静态成员和实例成员。 当接收器是不明确的实例或类型时，编译器将同时添加两者。 静态上下文（其中隐式 `this` 实例接收器无法使用）包含未定义 `this` 的成员的正文（例如，静态成员），以及不能使用 `this` 的位置（例如，字段初始值设定项和构造函数初始值设定项）。
1. 当一个方法组包含类型参数不满足其约束的某些泛型方法时，这些成员将从候选集中移除。
1. 对于方法组转换，返回类型与委托的返回类型不匹配的候选方法将从集中移除。

你将注意到此更改，因为当你确定哪个方法更好时，你将发现多义方法重载具有更少的编译器错误。

## <a name="enabling-more-efficient-safe-code"></a>启用更高效的安全代码

你应能够安全地编写性能与不安全代码一样好的 C# 代码。 安全代码可避免错误类，例如缓冲区溢出、杂散指针和其他内存访问错误。 这些新功能扩展了可验证安全代码的功能。 努力使用安全结构编写更多代码。 这些功能使其更容易实现。

以下新增功能支持使安全代码获得更好的性能的主题：

- 无需固定即可访问固定的字段。
- 可以重新分配 `ref` 本地变量。
- 可以使用 `stackalloc` 数组上的初始值设定项。
- 可以对支持模式的任何类型使用 `fixed` 语句。
- 可以使用其他泛型约束。
- 针对实参的 `in` 修饰符，指定形参通过引用传递，但不通过调用方法修改。 将 `in` 修饰符添加到参数是[源兼容的更改](version-update-considerations.md#source-compatible-changes)。
- 针对方法返回的 `ref readonly` 修饰符，指示方法通过引用返回其值，但不允许写入该对象。 如果向某个值赋予返回值，则添加 `ref readonly` 修饰符是[源兼容的更改](version-update-considerations.md#source-compatible-changes)。 将 `readonly` 修饰符添加到现有的 `ref` 返回语句是[不兼容的更改](version-update-considerations.md#incompatible-changes)。 它要求调用方更新 `ref` 本地变量的声明以包含 `readonly` 修饰符。
- `readonly struct` 声明，指示结构不可变，且应作为 `in` 参数传递到其成员方法。 将 `readonly` 修饰符添加到现有的结构声明是[二进制兼容的更改](version-update-considerations.md#binary-compatible-changes)。
- `ref struct` 声明，指示结构类型直接访问托管的内存，且必须始终分配有堆栈。 将 `ref` 修饰符添加到现有 `struct` 声明是[不兼容的更改](version-update-considerations.md#incompatible-changes)。 `ref struct` 不能是类的成员，也不能用于可能在堆上分配的其他位置。

可以在[编写安全高效的代码](../write-safe-efficient-code.md)中详细了解所有这些更改。

### <a name="ref-locals-and-returns"></a>Ref 局部变量和返回结果

此功能允许使用并返回对变量的引用的算法，这些变量在其他位置定义。 一个示例是使用大型矩阵并查找具有某些特征的单个位置。 下面的方法在矩阵中向该存储返回“引用”：

[!code-csharp[FindReturningRef](~/samples/snippets/csharp/new-in-7/MatrixSearch.cs#FindReturningRef "Find returning by reference")]

可以将返回值声明为 `ref` 并在矩阵中修改该值，如以下代码所示：

[!code-csharp[AssignRefReturn](~/samples/snippets/csharp/new-in-7/Program.cs#AssignRefReturn "Assign ref return")]

C# 语言还有多个规则，可保护你免于误用 `ref` 局部变量和返回结果：

- 必须将 `ref` 关键字添加到方法签名和方法中的所有 `return` 语句中。
  - 这清楚地表明，该方法在整个方法中通过引用返回。
- 可以将 `ref return` 分配给值变量或 `ref` 变量。
  - 调用方控制是否复制返回值。 在分配返回值时省略 `ref` 修饰符表示调用方需要该值的副本，而不是对存储的引用。
- 不可向 `ref` 本地变量赋予标准方法返回值。
  - 因为那将禁止类似 `ref int i = sequence.Count();` 这样的语句
- 不能将 `ref` 返回给其生存期不超出方法执行的变量。
  - 这意味着不可返回对本地变量或对类似作用域变量的引用。
- `ref` 局部变量和返回结果不可用于异步方法。
  - 编译器无法知道异步方法返回时，引用的变量是否已设置为其最终值。

添加 ref 局部变量和 ref 返回结果可通过避免复制值或多次执行取消引用操作，允许更为高效的算法。

向返回值添加 `ref` 是[源兼容的更改](version-update-considerations.md#source-compatible-changes)。 现有代码会进行编译，但在分配时复制 ref 返回值。 调用方必须将存储的返回值更新为 `ref` 局部变量，从而将返回值存储为引用。

现在，在对 `ref` 局部变量进行初始化后，可能会对其重新分配，以引用不同的实例。 以下代码现在编译：

```csharp
ref VeryLargeStruct refLocal = ref veryLargeStruct; // initialization
refLocal = ref anotherVeryLargeStruct; // reassigned, refLocal refers to different storage.
```

有关详细信息，请参阅有关 [`ref` 返回和 `ref` 局部变量](../programming-guide/classes-and-structs/ref-returns.md)以及 [`foreach`](../language-reference/keywords/foreach-in.md) 的文章。

有关详细信息，请参阅 [ref 关键字](../language-reference/keywords/ref.md)一文。

### <a name="conditional-ref-expressions"></a>条件 `ref` 表达式

最后，条件表达式可能生成 ref 结果而不是值。 例如，你将编写以下内容以检索对两个数组之一中第一个元素的引用：

```csharp
ref var r = ref (arr != null ? ref arr[0] : ref otherArr[0]);
```

变量 `r` 是对 `arr` 或 `otherArr` 中第一个值的引用。

有关详细信息，请参阅语言参考中的[条件运算符 (?:)](../language-reference/operators/conditional-operator.md)。

### <a name="in-parameter-modifier"></a>`in` 参数修饰符

`in` 关键字补充了现有的 ref 和 out 关键字，以按引用传递参数。 `in` 关键字指定按引用传递参数，但调用的方法不修改值。

你可以声明按值或只读引用传递的重载，如以下代码所示：

```csharp
static void M(S arg);
static void M(in S arg);
```

按值（前面示例中的第一个）传递的重载比按只读引用传递的重载更好。 若要使用只读引用参数调用版本，必须在调用方法前添加 `in` 修饰符。

有关详细信息，请参阅有关 [`in` 参数修饰符](../language-reference/keywords/in-parameter-modifier.md)的文章。

### <a name="more-types-support-the-fixed-statement"></a>更多类型支持 `fixed` 语句

`fixed` 语句支持有限的一组类型。 从 C# 7.3 开始，任何包含返回 `ref T` 或 `ref readonly T` 的 `GetPinnableReference()` 方法的类型均有可能为 `fixed`。 添加此功能意味着 `fixed` 可与 <xref:System.Span%601?displayProperty=nameWithType> 和相关类型配合使用。

有关详细信息，请参阅语言参考中的 [`fixed` 语句](../language-reference/keywords/fixed-statement.md)一文。

### <a name="indexing-fixed-fields-does-not-require-pinning"></a>索引 `fixed` 字段不需要进行固定

请考虑此结构：

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}
```

在早期版本的 C# 中，需要固定变量才能访问属于 `myFixedField` 的整数之一。 现在，以下代码进行编译，而不将变量 `p` 固定到单独的 `fixed` 语句中：

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        int p = s.myFixedField[5];
    }
}
```

变量 `p` 访问 `myFixedField` 中的一个元素。 无需声明单独的 `int*` 变量。 仍需要 `unsafe` 上下文。 在早期版本的 C# 中，需要声明第二个固定的指针：

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        fixed (int* ptr = s.myFixedField)
        {
            int p = ptr[5];
        }
    }
}
```

有关详细信息，请参阅有关 [`fixed` 语句](../language-reference/keywords/fixed-statement.md)的文章。

### <a name="stackalloc-arrays-support-initializers"></a>`stackalloc` 数组支持初始值设定项

当你对数组中的元素的值进行初始值设定时，你已能够指定该值：

```csharp
var arr = new int[3] {1, 2, 3};
var arr2 = new int[] {1, 2, 3};
```

现在，可向使用 `stackalloc` 进行声明的数组应用同一语法：

```csharp
int* pArr = stackalloc int[3] {1, 2, 3};
int* pArr2 = stackalloc int[] {1, 2, 3};
Span<int> arr = stackalloc [] {1, 2, 3};
```

有关详细信息，请参阅[`stackalloc`运算符](../language-reference/operators/stackalloc.md)一文。

### <a name="enhanced-generic-constraints"></a>增强的泛型约束

现在，可以将类型 <xref:System.Enum?displayProperty=nameWithType> 或 <xref:System.Delegate?displayProperty=nameWithType> 指定为类型参数的基类约束。

现在也可以使用新的 `unmanaged` 约束来指定类型参数必须是不可为 null 的[“非托管类型”](../language-reference/builtin-types/unmanaged-types.md)。

有关详细信息，请参阅有关 [`where` 泛型约束](../language-reference/keywords/where-generic-type-constraint.md)和[类型参数的约束](../programming-guide/generics/constraints-on-type-parameters.md)的文章。

将这些约束添加到现有类型是[不兼容的更改](version-update-considerations.md#incompatible-changes)。 封闭式泛型类型可能不再满足这些新约束的要求。

### <a name="generalized-async-return-types"></a>通用的异步返回类型

从异步方法返回 `Task` 对象可能在某些路径中导致性能瓶颈。 `Task` 是引用类型，因此使用它意味着分配对象。 如果使用 `async` 修饰符声明的方法返回缓存结果或以同步方式完成，那么额外的分配在代码的性能关键部分可能要耗费相当长的时间。 如果这些分配发生在紧凑循环中，则成本会变高。

新语言功能意味着异步方法返回类型不限于 `Task`、`Task<T>` 和 `void`。 返回类型必须仍满足异步模式，这意味着 `GetAwaiter` 方法必须是可访问的。 作为一个具体示例，已将 `ValueTask` 类型添加到 .NET 中，以使用这一新语言功能：

[!code-csharp[UsingValueTask](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#UsingValueTask "Using ValueTask")]

> [!NOTE]
> 你需要添加 NuGet 包 [`System.Threading.Tasks.Extensions`](https://www.nuget.org/packages/System.Threading.Tasks.Extensions/) 才能使用 <xref:System.Threading.Tasks.ValueTask%601> 类型。

此增强功能对于库作者最有用，可避免在性能关键型代码中分配 `Task`。

## <a name="new-compiler-options"></a>新的编译器选项

新的编译器选项支持 C# 程序的新版本和 DevOps 方案。

### <a name="reference-assembly-generation"></a>引用程序集生成

有两种新的编译器选项可生成仅引用程序集：[ProduceReferenceAssembly](../language-reference/compiler-options/output.md#producereferenceassembly) 和 [ProduceOnlyReferenceAssembly](../language-reference/compiler-options/code-generation.md#produceonlyreferenceassembly)。
链接的文章详细介绍了这些选项和引用程序集。

### <a name="public-or-open-source-signing"></a>公共或开放源代码签名

PublicSign 编译器选项指示编译器使用公钥对程序集进行签名。 程序集被标记为已签名，但签名取自公钥。 此选项使你能够使用公钥在开放源代码项目中构建签名的程序集。

有关详细信息，请参阅 [PublicSign 编译器选项](../language-reference/compiler-options/security.md#publicsign)一文。

### <a name="pathmap"></a>pathmap

PathMap 编译器选项指示编译器将生成环境中的源路径替换为映射的源路径。 PathMap 选项控制由编译器编写入 PDB 文件或为 <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> 编写的源路径。

有关详细信息，请参阅 [PathMap 编译器选项](../language-reference/compiler-options/advanced.md#pathmap)一文。
