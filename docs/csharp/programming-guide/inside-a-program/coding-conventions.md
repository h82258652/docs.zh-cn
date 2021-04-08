---
title: C# 编码约定 - C# 编程指南
description: 了解 C# 编码约定。 编码约定为代码创建一致的外观，并简化代码的复制、更改和维护过程。
ms.date: 03/31/2021
helpviewer_keywords:
- coding conventions, C#
- Visual C#, coding conventions
- C# language, coding conventions
ms.openlocfilehash: 019bf02ea3cdfec2c4ae0d73b5b375781c5fcd9a
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231318"
---
# <a name="c-coding-conventions-c-programming-guide"></a>C# 编码约定（C# 编程指南）

编码约定可实现以下目的：  
  
- 它们为代码创建一致的外观，以确保读取器专注于内容而非布局。  
  
- 它们使得读取器可以通过基于之前的经验进行的假设更快地理解代码。  
  
- 它们便于复制、更改和维护代码。  
  
- 它们展示 C# 最佳做法。  

Microsoft 根据本文中的准则来开发样本和文档。  
  
## <a name="naming-conventions"></a>命名约定  
  
- 在不包括 [using 指令](../../language-reference/keywords/using-directive.md)的短示例中，使用命名空间限定。 如果你知道命名空间默认导入项目中，则不必完全限定来自该命名空间的名称。 如果对于单行来说过长，则可以在点 (.) 后中断限定名称，如下面的示例所示。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet1":::

- 你不必更改使用 Visual Studio 设计器工具创建的对象的名称以使它们适合其他准则。  
  
## <a name="layout-conventions"></a>布局约定  

好的布局利用格式设置来强调代码的结构并使代码更便于阅读。 Microsoft 示例和样本符合以下约定：  
  
- 使用默认的代码编辑器设置（智能缩进、4 字符缩进、制表符保存为空格）。 有关详细信息，请参阅[选项、文本编辑器、C#、格式设置](/visualstudio/ide/reference/options-text-editor-csharp-formatting)。  
  
- 每行只写一条语句。  
  
- 每行只写一个声明。  
  
- 如果连续行未自动缩进，请将它们缩进一个制表符位（四个空格）。  
  
- 在方法定义与属性定义之间添加至少一个空白行。  
  
- 使用括号突出表达式中的子句，如下面的代码所示。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet2":::

## <a name="commenting-conventions"></a>注释约定  
  
- 将注释放在单独的行上，而非代码行的末尾。  
  
- 以大写字母开始注释文本。  
  
- 以句点结束注释文本。  
  
- 在注释分隔符 (//) 与注释文本之间插入一个空格，如下面的示例所示。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet3":::

- 请勿在注释周围创建格式化的星号块。  
  
## <a name="language-guidelines"></a>语言准则  

以下各节介绍 C# 遵循以准备代码示例和样本的做法。  
  
### <a name="string-data-type"></a>字符串数据类型  
  
- 使用[字符串内插](../../language-reference/tokens/interpolated.md)来连接短字符串，如下面的代码所示。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet6":::

- 若要在循环中追加字符串，尤其是在使用大量文本时，请使用 <xref:System.Text.StringBuilder> 对象。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet7":::

### <a name="implicitly-typed-local-variables"></a>隐式类型本地变量  
  
- 当变量类型明显来自赋值的右侧时，或者当精度类型不重要时，请对本地变量进行[隐式类型化](../classes-and-structs/implicitly-typed-local-variables.md)。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet8":::
  
- 当类型并非明显来自赋值的右侧时，请勿使用 [var](../../language-reference/keywords/var.md)。 请勿假设类型明显来自方法名称。 如果变量类型为 `new` 运算符或显式强制转换，则将其视为明显来自方法名称。
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet9":::

- 请勿依靠变量名称来指定变量的类型。 它可能不正确。 在以下示例中，变量名称 `inputInt` 会产生误导性。 它是字符串。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet10":::

- 避免使用 `var` 来代替 [dynamic](../../language-reference/builtin-types/reference-types.md)。 如果想要进行运行时类型推理，请使用 `dynamic`。 有关详细信息，请参阅[使用类型 dynamic（C# 编程指南）](../types/using-type-dynamic.md)。
  
- 使用隐式类型化来确定 [for](../../language-reference/keywords/for.md) 循环中循环变量的类型。  
  
  下面的示例在 `for` 语句中使用隐式类型化。  

    :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet7":::

- 请勿使用隐式类型化来确定 [foreach](../../language-reference/keywords/foreach-in.md) 循环中循环变量的类型。

  下面的示例在 `foreach` 语句中使用显式类型化。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet12":::

  > [!NOTE]
  > 注意不要意外更改可迭代集合的元素类型。 例如，在 `foreach` 语句中从 <xref:System.Linq.IQueryable?displayProperty=nameWithType> 切换到 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 很容易，这会更改查询的执行。

### <a name="unsigned-data-types"></a>无符号数据类型  
  
通常，使用 `int` 而非无符号类型。 `int` 的使用在整个 C# 中都很常见，并且当你使用 `int` 时，更易于与其他库交互。  
  
### <a name="arrays"></a>数组  
  
当在声明行上初始化数组时，请使用简洁的语法。 在以下示例中，请注意不能使用 `var` 替代 `string[]`。  
  
:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet13a":::

如果使用显式实例化，则可以使用 `var`。

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet13b":::

如果指定数组大小，只能一次初始化一个元素。
  
:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet13c":::
  
### <a name="delegates"></a>委托  
  
使用 [`Func<>` 和 `Action<>`](../../../standard/delegates-lambdas.md)，而不是定义委托类型。 在类中，定义委托方法。  

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet14a":::

使用 `Func<>` 或 `Action<>` 委托定义的签名来调用方法。

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet15a":::

如果创建委托类型的实例，请使用简洁的语法。 在类中，定义委托类型和具有匹配签名的方法。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet14b":::

创建委托类型的实例，然后调用该实例。 以下声明显示了紧缩的语法。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet15b":::

以下声明使用了完整的语法。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet15c":::

### <a name="try-catch-and-using-statements-in-exception-handling"></a>`try`-`catch` 和 `using` 语句正在异常处理中  
  
- 对大多数异常处理使用 [try-catch](../../language-reference/keywords/try-catch.md) 语句。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet16":::

- 通过使用 C# [using 语句](../../language-reference/keywords/using-statement.md)简化你的代码。 如果具有 [try-finally](../../language-reference/keywords/try-finally.md) 语句（该语句中 `finally` 块的唯一代码是对 <xref:System.IDisposable.Dispose%2A> 方法的调用），请使用 `using` 语句代替。

  在以下示例中，`try`-`finally` 语句仅在 `finally` 块中调用 `Dispose`。
  
   :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet17a":::

  可以使用 `using` 语句执行相同的操作。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet17b":::

  在 C# 8 及更高版本中，使用无需大括号的新的 [`using` 语法](../../language-reference/keywords/using-statement.md)：

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet17c":::

### <a name="-and--operators"></a>`&&` 和 `||` 运算符  
  
若要通过跳过不必要的比较来避免异常并提高性能，请在执行比较时使用 [`&&`](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-)（而不是 [`&`](../../language-reference/operators/boolean-logical-operators.md#logical-and-operator-)）和 [`||`](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-)（而不是 [`|`](../../language-reference/operators/boolean-logical-operators.md#logical-or-operator-)），如下面的示例所示。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet18":::

如果除数为 0，则 `if` 语句中的第二个子句将导致运行时错误。 但是，当第一个表达式为 false 时，&& 运算符将发生短路。 也就是说，它并不评估第二个表达式。 如果 `divisor` 为 0，则 & 运算符将同时计算这两个表达式，从而导致运行时错误。
  
### <a name="new-operator"></a>`new` 运算符  
  
- 使用对象实例化的简洁形式之一，如以下声明中所示。 第二个示例显示了从 C# 9 开始可用的语法。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet19":::
  
  ```csharp
  ExampleClass instance2 = new();
  ```
  
  前面的声明等效于下面的声明。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet20":::

- 使用对象初始值设定项简化对象创建，如以下示例中所示。

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet21a":::

  下面的示例设置了与前面的示例相同的属性，但未使用初始值设定项。
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet21b":::

### <a name="event-handling"></a>事件处理  
  
如果你正在定义一个稍后不需要删除的事件处理程序，请使用 lambda 表达式。  
  
:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet22":::

Lambda 表达式缩短了以下传统定义。

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet23":::

### <a name="static-members"></a>静态成员  
  
使用类名调用 [static](../../language-reference/keywords/static.md) 成员：ClassName.StaticMember。 这种做法通过明确静态访问使代码更易于阅读。  请勿使用派生类的名称来限定基类中定义的静态成员。  编译该代码时，代码可读性具有误导性，如果向派生类添加具有相同名称的静态成员，代码可能会被破坏。  
  
### <a name="linq-queries"></a>LINQ 查询  
  
- 对查询变量使用有意义的名称。 下面的示例为位于西雅图的客户使用 `seattleCustomers`。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet25":::

- 使用别名确保匿名类型的属性名称都使用 Pascal 大小写格式正确大写。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet26":::

- 如果结果中的属性名称模棱两可，请对属性重命名。 例如，如果你的查询返回客户名称和分销商 ID，而不是在结果中将它们保留为 `Name` 和 `ID`，请对它们进行重命名以明确 `Name` 是客户的名称，`ID` 是分销商的 ID。  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet27":::

- 在查询变量和范围变量的声明中使用隐式类型化。  

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet25":::

- 对齐 [from](../../language-reference/keywords/from-clause.md) 子句下的查询子句，如上面的示例所示。  

- 在其他查询子句之前使用 [where](../../language-reference/keywords/where-clause.md) 子句，以确保后面的查询子句作用于经过减少和筛选的数据集。  

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet29":::

- 使用多行 `from` 子句代替 [join](../../language-reference/keywords/join-clause.md) 子句以访问内部集合。 例如，`Student` 对象的集合可能包含测验分数的集合。 当执行以下查询时，它返回高于 90 的分数，并返回得到该分数的学生的姓氏。  

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet30":::

## <a name="security"></a>安全性  

请遵循[安全编码准则](../../../standard/security/secure-coding-guidelines.md)中的准则。  
  
## <a name="see-also"></a>请参阅

- [.NET 运行时编码准则](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md)
- [Visual Basic 编码约定](../../../visual-basic/programming-guide/program-structure/coding-conventions.md)
- [安全编码准则](../../../standard/security/secure-coding-guidelines.md)
