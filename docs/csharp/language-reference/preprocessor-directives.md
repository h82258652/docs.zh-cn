---
description: 了解控制条件编译、警告、可为空分析等的不同 C# 预处理器指令
title: C# 预处理器指令
ms.date: 03/17/2021
f1_keywords:
- cs.preprocessor
- '#nullable'
- '#if'
- '#else'
- '#elif'
- '#endif'
- '#define'
- '#undef'
- '#warning'
- '#error'
- '#line'
- '#region'
- '#endregion'
- '#pragma'
- '#pragma warning'
- '#pragma checksum'
helpviewer_keywords:
- preprocessor directives [C#]
- keywords [C#], preprocessor directives
- '#nullable directive [C#]'
- '#if directive [C#]'
- '#else directive [C#]'
- '#elif directive [C#]'
- '#endif directive [C#]'
- '#define directive [C#]'
- '#undef directive [C#]'
- '#warning directive [C#]'
- '#error directive [C#]'
- '#line directive [C#]'
- '#region directive [C#]'
- '#endregion directive [C#]'
- '#pragma directive [C#]'
- '#pragma warning [C#]'
- '#pragma checksum [C#]'
ms.openlocfilehash: 373952282a684da25414af9853e18b7bc4874108
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637655"
---
# <a name="c-preprocessor-directives"></a>C# 预处理器指令

尽管编译器没有单独的预处理器，但本节中所述指令的处理方式与有预处理器时一样。 可使用这些指令来帮助条件编译。 不同于 C 和 C++ 指令，不能使用这些指令来创建宏。 预处理器指令必须是一行中唯一的说明。

## <a name="nullable-context"></a>可为空上下文

`#nullable` 预处理器指令将设置可为空注释上下文和可为空警告上下文 。 此指令控制是否可为空注释是否有效，以及是否给出为 Null 性警告。 每个上下文要么处于已禁用状态，要么处于已启用状态 。

可在项目级别（C# 源代码之外）指定这两个上下文。 `#nullable` 指令控制注释和警告上下文，并优先于项目级设置。 指令会设置其控制的上下文，直到另一个指令替代它，或直到源文件结束为止。

指令的效果如下所示：

- `#nullable disable`：将可为空注释和警告上下文设置为“已禁用”。
- `#nullable enable`：将可为空注释和警告上下文设置为“已启用”。
- `#nullable restore`：将可为空注释和警告上下文还原为项目设置。
- `#nullable disable annotations`：将可为空注释上下文设置为“已禁用”。
- `#nullable enable annotations`：将可为空注释上下文设置为“已启用”。
- `#nullable restore annotations`：将可为空注释上下文还原为项目设置。
- `#nullable disable warnings`：将可为空警告上下文设置为“已禁用”。
- `#nullable enable warnings`：将可为空警告上下文设置为“已启用”。
- `#nullable restore warnings`：将可为空警告上下文还原为项目设置。

## <a name="conditional-compilation"></a>条件编译

使用四个预处理器指令来控制条件编译：

- `#if`：打开条件编译，其中仅在定义了指定的符号时才会编译代码。
- `#elif`：关闭前面的条件编译，并基于是否定义了指定的符号打开一个新的条件编译。
- `#else`：关闭前面的条件编译，如果没有定义前面指定的符号，打开一个新的条件编译。
- `#endif`：关闭前面的条件编译。

如果 C# 编译器遇到 `#if` 指令，最后跟着一个 `#endif` 指令，则仅当定义指定的符号时，它才编译这些指令之间的代码。 与 C 和 C++ 不同，不能将数字值分配给符号。 C# 中的 `#if` 语句是布尔值，且仅测试是否已定义该符号。 例如：

```csharp
#if DEBUG
    Console.WriteLine("Debug version");
#endif
```

可以使用运算符 [`==`（相等）](operators/equality-operators.md#equality-operator-)和 [`!=`（不相等）](operators/equality-operators.md#inequality-operator-)来测试 [ `bool`](builtin-types/bool.md) 值是 `true` 还是 `false`。 `true` 表示定义该符号。 语句 `#if DEBUG` 具有与 `#if (DEBUG == true)` 相同的含义。 可以使用 [`&&` (and)](operators/boolean-logical-operators.md#conditional-logical-and-operator-)、[`||` (or)](operators/boolean-logical-operators.md#conditional-logical-or-operator-) 和 [`!`(not)](operators/boolean-logical-operators.md#logical-negation-operator-) 运算符来计算是否已定义多个符号。 还可以用括号对符号和运算符进行分组。

`#if` 以及 `#else`、`#elif`、`#endif`、`#define` 和 `#undef` 指令，允许基于是否存在一个或多个符号包括或排除代码。 条件编译在编译调试版本的代码或编译特定配置的代码时会很有用。

以 `#if` 指令开头的条件指令必须以 `#endif` 指令显式终止。 `#define` 允许你定义一个符号。 通过将该符号用作传递给 `#if` 指令的表达式，该表达式的计算结果为 `true`。 还可以通过 [DefineConstants](compiler-options/language.md#defineconstants) 编译器选项来定义符号。 可以通过 `#undef` 取消定义符号。 使用 `#define` 创建的符号的作用域是在其中定义它的文件。 使用 DefineConstants 或 `#define` 定义的符号与具有相同名称的变量不冲突。 也就是说，变量名称不应传递给预处理器指令，且符号仅能由预处理器指令评估。

`#elif` 可以创建复合条件指令。 如果之前的 `#if` 和任何之前的可选 `#elif` 指令表达式的值都不为 `true`，则计算 `#elif` 表达式。 如果 `#elif` 表达式计算结果为 `true`，编译器将计算 `#elif` 和下一条件指令间的所有代码。 例如：

```csharp
#define VC7
//...
#if debug
    Console.WriteLine("Debug build");
#elif VC7
    Console.WriteLine("Visual Studio 7");
#endif
```

`#else` 允许创建复合条件指令，因此，如果先前 `#if` 或（可选）`#elif` 指令中的任何表达式的计算结果都不是 `true`，则编译器将对介于 `#else` 和下一个 `#endif` 之间的所有代码进行求值。 `#endif`(#endif) 必须是 `#else` 之后的下一个预处理器指令。

`#endif` 指定条件指令的末尾，以 `#if` 指令开头。

此外，生成系统还会感知表示 SDK 样式项目中不同[目标框架](../../standard/frameworks.md)的预定义预处理器符号。 在创建可以面向多个 .NET 版本的应用程序时，这些符号会很有用。

[!INCLUDE [Preprocessor symbols](~/includes/preprocessor-symbols.md)]

> [!NOTE]
> 对于传统的非 SDK 样式的项目，必须通过项目的属性页面在 Visual Studio 中为不同目标框架手动配置条件编译符号。

其他预定义符号包括 `DEBUG` 和 `TRACE` 常数。 你可以使用 `#define` 替代项目的值集。 例如，会根据生成配置属性（“调试”或者“发布”模式）自动设置 DEBUG 符号。

下例显示如何在文件上定义 `MYTEST` 符号，然后测试 `MYTEST` 和 `DEBUG` 符号的值。 此示例的输出取决于是在“调试”还是“发布”配置模式下生成项目 。

```csharp
#define MYTEST
using System;
public class MyClass
{
    static void Main()
    {
#if (DEBUG && !MYTEST)
        Console.WriteLine("DEBUG is defined");
#elif (!DEBUG && MYTEST)
        Console.WriteLine("MYTEST is defined");
#elif (DEBUG && MYTEST)
        Console.WriteLine("DEBUG and MYTEST are defined");  
#else
        Console.WriteLine("DEBUG and MYTEST are not defined");
#endif
    }
}
```

下例显示如何针对不同的目标框架进行测试，以便在可能时使用较新的 API：

```csharp
public class MyClass
{
    static void Main()
    {
#if NET40
        WebClient _client = new WebClient();
#else
        HttpClient _client = new HttpClient();
#endif
    }
    //...
}
```

## <a name="defining-symbols"></a>定义符号

使用以下两个预处理器指令来定义或取消定义条件编译的符号：

- `#define`：定义符号。
- `#undef`：取消定义符号。

使用 `#define` 来定义符号。 将符号用作传递给 `#if` 指令的表达式时，该表达式的计算结果为 `true`，如以下示例所示：

 ```csharp
 #define VERBOSE

#if VERBOSE
    Console.WriteLine("Verbose output version");
#endif

 ```

> [!NOTE]
> `#define` 指令不能用于声明常量值，这与 C 和 C++ 中的通常做法一样。 C# 中的常量最好定义为类或结构的静态成员。 如果具有多个此类常量，请考虑创建一个单独的“常量”类来容纳它们。

符号可用于指定编译的条件。 可通过 `#if` 或 `#elif` 测试符号。 还可以使用 <xref:System.Diagnostics.ConditionalAttribute> 来执行条件编译。 可以定义符号，但不能为符号分配值。 文件中必须先出现 `#define` 指令，才能使用并非同时也是预处理器指令的任何指示。 还可以通过 [DefineConstants](compiler-options/language.md#defineconstants) 编译器选项来定义符号。 可以通过 `#undef` 取消定义符号。

## <a name="defining-regions"></a>定义区域

可以使用以下两个预处理器指令来定义可在大纲中折叠的代码区域：

- `#region`：启动区域。
- `#endregion`：结束区域

利用 `#region`，可以指定在使用代码编辑器的[大纲](/visualstudio/ide/outlining)功能时可展开或折叠的代码块。 在较长的代码文件中，折叠或隐藏一个或多个区域十分便利，这样，可将精力集中于当前处理的文件部分。 下面的示例演示如何定义区域：

```csharp
#region MyClass definition
public class MyClass
{
    static void Main()
    {
    }
}
#endregion
```

`#region` 块必须通过 `#endregion` 指令终止。 `#region` 块不能与 `#if` 块重叠。 但是，可以将 `#region` 块嵌套在 `#if` 块内，或将 `#if` 块嵌套在 `#region` 块内。

## <a name="error-and-warning-information"></a>错误和警告信息

使用以下指令指示编译器生成用户定义的编译器错误和警告，并控制行信息：

- `#error`：使用指定的消息生成编译器错误。
- `#warning`：使用指定的消息生成编译器警告。
- `#line`：更改用编译器消息输出的行号。

`#error` 可从代码中的特定位置生成 [CS1029](compiler-messages/cs1029.md) 用户定义的错误。 例如：

```csharp
#error Deprecated code in this method.
```

> [!NOTE]
> 编译器以特殊的方式处理 `#error version` 并报告编译器错误 CS8304，消息中包含使用的编译器和语言版本。

`#warning` 允许你从代码中的特定位置生成 [ CS1030](../misc/cs1030.md) 第一级编译器警告。 例如：

```csharp
#warning Deprecated code in this method.
```

借助 `#line`，可修改编译器的行号及（可选）用于错误和警告的文件名输出。

以下示例演示如何报告与行号相关联的两个警告。 `#line 200` 指令将下一行的行号强制设为 200（尽管默认值为 #6）；在执行下一个 `#line` 指令前，文件名都会报告为“特殊”。 `#line default` 指令将行号恢复至默认行号，这会对上一指令重新编号的行进行计数。

```csharp
class MainClass
{
    static void Main()
    {
#line 200 "Special"
        int i;
        int j;
#line default
        char c;
        float f;
#line hidden // numbering not affected
        string s;
        double d;
    }
}
```

编译产生以下输出：

```console
Special(200,13): warning CS0168: The variable 'i' is declared but never used
Special(201,13): warning CS0168: The variable 'j' is declared but never used
MainClass.cs(9,14): warning CS0168: The variable 'c' is declared but never used
MainClass.cs(10,15): warning CS0168: The variable 'f' is declared but never used
MainClass.cs(12,16): warning CS0168: The variable 's' is declared but never used
MainClass.cs(13,16): warning CS0168: The variable 'd' is declared but never used
```

可在生成过程的自动、中间步骤中使用 `#line` 指令。 例如，如果已从原始源代码文件中删除行，但仍希望编译器基于文件中的原始行号生成输出，可在删除行后，使用 `#line` 来模拟原始行号。

`#line hidden` 指令能对调试程序隐藏连续行，当开发者逐行执行代码时，介于 `#line hidden` 和下一 `#line` 指令（假设它不是其他 `#line hidden` 指令）间的任何行都将被跳过。 还可通过此选项允许 ASP.NET 区分用户定义和计算机生成的代码。 虽然此功能主要用于 ASP.NET，但可能更多的源生成器会利用此功能。

`#line hidden` 指令不影响错误报告中的文件名或行号。 也就是说，如果编译器在隐藏块中发现错误，编译器将报告错误的当前文件名和行号。

`#line filename` 指令可指定要在编译器输出中显示的文件名。 默认情况下，将使用源代码文件的实际名称。 该文件名必须以双引号 ("") 引起来，且必须位于行号之后。

下列示例演示调试程序如何忽略代码中的隐藏行。 运行示例时，它将显示三行文本。 但是，如果按照示例所示设置断点、并按 F10 逐行执行代码，调试程序将忽略隐藏行。 即使在隐藏行设置断点，调试程序仍将忽略它。

```csharp
// preprocessor_linehidden.cs
using System;
class MainClass
{
    static void Main()
    {
        Console.WriteLine("Normal line #1."); // Set break point here.
#line hidden
        Console.WriteLine("Hidden line.");
#line default
        Console.WriteLine("Normal line #2.");
    }
}
```

## <a name="pragmas"></a>杂注

`#pragma` 为编译器给出特殊指令以编译它所在的文件。 这些指令必须受编译器支持。 换句话说，不能使用 `#pragma` 创建自定义的预处理指令。
  
- [`#pragma warning`](#pragma-warning)：启用或禁用警告。
- [`#pragma checksum`](#pragma-checksum)：生成校验和。

```csharp
#pragma pragma-name pragma-arguments
```

其中 `pragma-name` 是可识别 pragma 的名称，`pragma-arguments` 是特定于 pragma 的参数。

### <a name="pragma-warning"></a>#pragma warning

`#pragma warning` 可以启用或禁用特定警告。

```csharp
#pragma warning disable warning-list
#pragma warning restore warning-list
```

其中 `warning-list` 是以逗号分隔的警告编号的列表。 “CS”前缀是可选的。 未指定警告编号时，`disable` 会禁用所有警告，`restore` 会启用所有警告。

> [!NOTE]
> 若要在 Visual Studio 中查找警告编号，请生成项目，然后在“输出”窗口中查找警告编号。

`disable` 从源文件的下一行开始生效。 警告会在后面的 `restore` 行上还原。 如果文件中没有 `restore`，则在同一编译中任何之后文件的第一行，警告将还原为其默认状态。

```csharp
// pragma_warning.cs
using System;

#pragma warning disable 414, CS3021
[CLSCompliant(false)]
public class C
{
    int i = 1;
    static void Main()
    {
    }
}
#pragma warning restore CS3021
[CLSCompliant(false)]  // CS3021
public class D
{
    int i = 1;
    public static void F()
    {
    }
}
```

### <a name="pragma-checksum"></a>#pragma checksum

生成源文件的校验和以帮助调试 ASP.NET 页面。

```csharp
#pragma checksum "filename" "{guid}" "checksum bytes"
```

其中，`"filename"` 是需要监视更改或更新的文件的名称，`"{guid}"` 是哈希算法的全局唯一标识符 (GUID)，`"checksum_bytes"` 是表示校验和字节的十六进制数字的字符串。 必须是偶数个十六进制数字。 奇数个数字会导致编译时警告出现，且指令遭忽略。

Visual Studio 调试器使用校验和确保它可始终找到正确的源。 编译器为源文件计算校验和，然后将输出发出到程序数据库 (PDB) 文件。 调试器随后使用 PDB 针对它为源文件计算的校验和进行比较。

此解决方案不适用于 ASP.NET 项目，因为计算出的校验和是针对生成的源文件，而不是 .aspx 文件。 为解决此问题，`#pragma checksum` 为 ASP.NET 页面提供校验和支持。

在 Visual C# 中创建 ASP.NET 项目时，生成的源文件包含 .aspx 文件（从该文件生成源）的校验和。 编译器随后将此信息写入 PDB 文件中。

如果编译器在文件中没有找到 `#pragma checksum` 指令，它将计算校验和，并将该值写入 PDB 文件。

```csharp
class TestClass
{
    static int Main()
    {
        #pragma checksum "file.cs" "{406EA660-64CF-4C82-B6F0-42D48172A799}" "ab007f1d23d9" // New checksum
    }
}
```
