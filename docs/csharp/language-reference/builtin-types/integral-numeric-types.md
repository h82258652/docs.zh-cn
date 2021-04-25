---
title: 整型数值类型 - C# 参考
description: 了解每种整型数值类型的范围、存储大小和用途。
ms.date: 04/10/2021
f1_keywords:
- byte_CSharpKeyword
- sbyte_CSharpKeyword
- short_CSharpKeyword
- ushort_CSharpKeyword
- int_CSharpKeyword
- uint_CSharpKeyword
- long_CSharpKeyword
- ulong_CSharpKeyword
helpviewer_keywords:
- integral types, C#
- Visual C#, integral types
- types [C#], integral types
- ranges of integral types [C#]
- byte keyword [C#]
- sbyte keyword [C#]
- short keyword [C#]
- ushort keyword [C#]
- int keyword [C#]
- uint keyword [C#]
- long keyword [C#]
- ulong keyword [C#]
ms.openlocfilehash: 21e6595e477fd48d0e5f39f5b4f1f7c5893a8840
ms.sourcegitcommit: bbc724b72fb6c978905ac715e4033efa291f84dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107369577"
---
# <a name="integral-numeric-types--c-reference"></a>整型数值类型（C# 参考）

整型数值类型  表示整数。 所有的整型数值类型均为[值类型](value-types.md)。 它们还是[简单类型](value-types.md#built-in-value-types)，可以使用[文本](#integer-literals)进行初始化。 所有整型数值类型都支持[算术](../operators/arithmetic-operators.md)、[位逻辑](../operators/bitwise-and-shift-operators.md)、[比较](../operators/comparison-operators.md)和[相等](../operators/equality-operators.md)运算符。

## <a name="characteristics-of-the-integral-types"></a>整型类型的特征

C# 支持以下预定义整型类型：

|C# 类型/关键字|范围|大小|.NET 类型|
|----------|-----------|----------|-------------|
|`sbyte`|-128 到 127|8 位带符号整数|<xref:System.SByte?displayProperty=nameWithType>|
|`byte`|0 到 255|无符号的 8 位整数|<xref:System.Byte?displayProperty=nameWithType>|
|`short`|-32,768 到 32,767|有符号 16 位整数|<xref:System.Int16?displayProperty=nameWithType>|
|`ushort`|0 到 65,535|无符号 16 位整数|<xref:System.UInt16?displayProperty=nameWithType>|
|`int`|-2,147,483,648 到 2,147,483,647|带符号的 32 位整数|<xref:System.Int32?displayProperty=nameWithType>|
|`uint`|0 到 4,294,967,295|无符号的 32 位整数|<xref:System.UInt32?displayProperty=nameWithType>|
|`long`|-9,223,372,036,854,775,808 到 9,223,372,036,854,775,807|64 位带符号整数|<xref:System.Int64?displayProperty=nameWithType>|
|`ulong`|0 到 18,446,744,073,709,551,615|无符号 64 位整数|<xref:System.UInt64?displayProperty=nameWithType>|
|`nint`|取决于平台|带符号的 32 位或 64 位整数|<xref:System.IntPtr?displayProperty=nameWithType>|
|`nuint`|取决于平台|无符号的 32 位或 64 位整数|<xref:System.UIntPtr?displayProperty=nameWithType>|

在除最后两行之外的所有表行中，最左侧列中的每个 C# 类型关键字都是相应 .NET 类型的别名。 关键字和 .NET 类型名称是可互换的。 例如，以下声明声明了相同类型的变量：

```csharp
int a = 123;
System.Int32 b = 123;
```

表的最后两行中的 `nint` 和 `nuint` 类型是本机大小的整数。 在内部它们由所指示的 .NET 类型表示，但在任意情况下关键字和 .NET 类型都是不可互换的。 编译器为 `nint` 和 `nuint` 的整数类型提供操作和转换，而不为指针类型 `System.IntPtr` 和 `System.UIntPtr` 提供。 有关详细信息，请参阅 [`nint` 和 `nuint` 类型](nint-nuint.md)。

每个整型类型的默认值都为零 `0`。 除本机大小的类型外，每个整型类型都有 `MinValue` 和 `MaxValue` 常量，提供该类型的最小值和最大值。

<xref:System.Numerics.BigInteger?displayProperty=nameWithType> 结构用于表示没有上限或下限的带符号整数。

## <a name="integer-literals"></a>整数文本

整数文本可以是

-  十进制：不使用任何前缀
-  十六进制：使用 `0x` 或 `0X` 前缀
-  二进制：使用 `0b` 或 `0B` 前缀（在 C# 7.0 和更高版本中可用）

下面的代码演示每种类型的示例：

```csharp
var decimalLiteral = 42;
var hexLiteral = 0x2A;
var binaryLiteral = 0b_0010_1010;
```

前面的示例还演示了如何将 `_` 用作数字分隔符（从 C# 7.0 开始提供支持）。 可以将数字分隔符用于所有类型的数字文本。

整数文本的类型由其后缀确定，如下所示：

- 如果文本没有后缀，则其类型为以下类型中可表示其值的第一个类型：`int`、`uint`、`long`、`ulong`。

  > [!NOTE]
  > 文本解释为正值。 例如，文本 `0xFF_FF_FF_FF` 表示 `uint` 类型的数字 `4294967295`，但其位表现形式与 `int` 类型的数字 `-1` 相同。 如果需要特定类型的值，请将文本强制转换为该类型。 如果文本值无法以目标类型表示，请使用运算符 `unchecked`。 示例：`unchecked((int)0xFF_FF_FF_FF)` 生成 `-1`。

- 如果文本以 `U` 或 `u` 为后缀，则其类型为以下类型中可表示其值的第一个类型：`uint`、`ulong`。
- 如果文本以 `L` 或 `l` 为后缀，则其类型为以下类型中可表示其值的第一个类型：`long`、`ulong`。

  > [!NOTE]
  > 可以使用小写字母 `l` 作为后缀。 但是，这会生成一个编译器警告，因为字母 `l` 可能与数字 `1` 混淆。 为清楚起见，请使用 `L`。

- 如果文本的后缀为 `UL`、`Ul`、`uL`、`ul`、`LU`、`Lu`、`lU` 或 `lu`，则其类型为 `ulong`。

如果由整数字面量所表示的值超出了 <xref:System.UInt64.MaxValue?displayProperty=nameWithType>，则将出现编译器错误 [CS1021](../../misc/cs1021.md)。

如果确定的整数文本的类型为 `int`，且文本所表示的值位于目标类型的范围内，则该值可以隐式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`、`nint` 或 `nuint`：

```csharp
byte a = 17;
byte b = 300;   // CS0031: Constant value '300' cannot be converted to a 'byte'
```

如前面的示例所示，如果文本的值不在目标类型的范围内，则发生编译器错误 [CS0031](../../misc/cs0031.md)。

还可以使用强制转换将整数文本所表示的值转换为除确定的文本类型之外的类型：

```csharp
var signedByte = (sbyte)42;
var longVariable = (long)42;
```

## <a name="conversions"></a>转换

可以将任何整型数值类型转换为其他整数数值类型。 如果目标类型可以存储源类型的所有值，则转换是隐式的。 否则，需要使用[强制转换表达式](../operators/type-testing-and-cast.md#cast-expression)来执行显式转换。 有关详细信息，请参阅[内置数值转换](numeric-conversions.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [整型类型](~/_csharplang/spec/types.md#integral-types)
- [整数文本](~/_csharplang/spec/lexical-structure.md#integer-literals)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [值类型](value-types.md)
- [浮点类型](floating-point-numeric-types.md)
- [标准数字格式字符串](../../../standard/base-types/standard-numeric-format-strings.md)
- [.NET 中的数字](../../../standard/numerics.md)
