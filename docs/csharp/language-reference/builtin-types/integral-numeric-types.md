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
# <a name="integral-numeric-types--c-reference"></a><span data-ttu-id="69242-103">整型数值类型（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="69242-103">Integral numeric types  (C# reference)</span></span>

<span data-ttu-id="69242-104">整型数值类型  表示整数。</span><span class="sxs-lookup"><span data-stu-id="69242-104">The *integral numeric types* represent integer numbers.</span></span> <span data-ttu-id="69242-105">所有的整型数值类型均为[值类型](value-types.md)。</span><span class="sxs-lookup"><span data-stu-id="69242-105">All integral numeric types are [value types](value-types.md).</span></span> <span data-ttu-id="69242-106">它们还是[简单类型](value-types.md#built-in-value-types)，可以使用[文本](#integer-literals)进行初始化。</span><span class="sxs-lookup"><span data-stu-id="69242-106">They are also [simple types](value-types.md#built-in-value-types) and can be initialized with [literals](#integer-literals).</span></span> <span data-ttu-id="69242-107">所有整型数值类型都支持[算术](../operators/arithmetic-operators.md)、[位逻辑](../operators/bitwise-and-shift-operators.md)、[比较](../operators/comparison-operators.md)和[相等](../operators/equality-operators.md)运算符。</span><span class="sxs-lookup"><span data-stu-id="69242-107">All integral numeric types support [arithmetic](../operators/arithmetic-operators.md), [bitwise logical](../operators/bitwise-and-shift-operators.md), [comparison](../operators/comparison-operators.md), and [equality](../operators/equality-operators.md) operators.</span></span>

## <a name="characteristics-of-the-integral-types"></a><span data-ttu-id="69242-108">整型类型的特征</span><span class="sxs-lookup"><span data-stu-id="69242-108">Characteristics of the integral types</span></span>

<span data-ttu-id="69242-109">C# 支持以下预定义整型类型：</span><span class="sxs-lookup"><span data-stu-id="69242-109">C# supports the following predefined integral types:</span></span>

|<span data-ttu-id="69242-110">C# 类型/关键字</span><span class="sxs-lookup"><span data-stu-id="69242-110">C# type/keyword</span></span>|<span data-ttu-id="69242-111">范围</span><span class="sxs-lookup"><span data-stu-id="69242-111">Range</span></span>|<span data-ttu-id="69242-112">大小</span><span class="sxs-lookup"><span data-stu-id="69242-112">Size</span></span>|<span data-ttu-id="69242-113">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="69242-113">.NET type</span></span>|
|----------|-----------|----------|-------------|
|`sbyte`|<span data-ttu-id="69242-114">-128 到 127</span><span class="sxs-lookup"><span data-stu-id="69242-114">-128 to 127</span></span>|<span data-ttu-id="69242-115">8 位带符号整数</span><span class="sxs-lookup"><span data-stu-id="69242-115">Signed 8-bit integer</span></span>|<xref:System.SByte?displayProperty=nameWithType>|
|`byte`|<span data-ttu-id="69242-116">0 到 255</span><span class="sxs-lookup"><span data-stu-id="69242-116">0 to 255</span></span>|<span data-ttu-id="69242-117">无符号的 8 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-117">Unsigned 8-bit integer</span></span>|<xref:System.Byte?displayProperty=nameWithType>|
|`short`|<span data-ttu-id="69242-118">-32,768 到 32,767</span><span class="sxs-lookup"><span data-stu-id="69242-118">-32,768 to 32,767</span></span>|<span data-ttu-id="69242-119">有符号 16 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-119">Signed 16-bit integer</span></span>|<xref:System.Int16?displayProperty=nameWithType>|
|`ushort`|<span data-ttu-id="69242-120">0 到 65,535</span><span class="sxs-lookup"><span data-stu-id="69242-120">0 to 65,535</span></span>|<span data-ttu-id="69242-121">无符号 16 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-121">Unsigned 16-bit integer</span></span>|<xref:System.UInt16?displayProperty=nameWithType>|
|`int`|<span data-ttu-id="69242-122">-2,147,483,648 到 2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="69242-122">-2,147,483,648 to 2,147,483,647</span></span>|<span data-ttu-id="69242-123">带符号的 32 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-123">Signed 32-bit integer</span></span>|<xref:System.Int32?displayProperty=nameWithType>|
|`uint`|<span data-ttu-id="69242-124">0 到 4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="69242-124">0 to 4,294,967,295</span></span>|<span data-ttu-id="69242-125">无符号的 32 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-125">Unsigned 32-bit integer</span></span>|<xref:System.UInt32?displayProperty=nameWithType>|
|`long`|<span data-ttu-id="69242-126">-9,223,372,036,854,775,808 到 9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="69242-126">-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807</span></span>|<span data-ttu-id="69242-127">64 位带符号整数</span><span class="sxs-lookup"><span data-stu-id="69242-127">Signed 64-bit integer</span></span>|<xref:System.Int64?displayProperty=nameWithType>|
|`ulong`|<span data-ttu-id="69242-128">0 到 18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="69242-128">0 to 18,446,744,073,709,551,615</span></span>|<span data-ttu-id="69242-129">无符号 64 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-129">Unsigned 64-bit integer</span></span>|<xref:System.UInt64?displayProperty=nameWithType>|
|`nint`|<span data-ttu-id="69242-130">取决于平台</span><span class="sxs-lookup"><span data-stu-id="69242-130">Depends on platform</span></span>|<span data-ttu-id="69242-131">带符号的 32 位或 64 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-131">Signed 32-bit or 64-bit integer</span></span>|<xref:System.IntPtr?displayProperty=nameWithType>|
|`nuint`|<span data-ttu-id="69242-132">取决于平台</span><span class="sxs-lookup"><span data-stu-id="69242-132">Depends on platform</span></span>|<span data-ttu-id="69242-133">无符号的 32 位或 64 位整数</span><span class="sxs-lookup"><span data-stu-id="69242-133">Unsigned 32-bit or 64-bit integer</span></span>|<xref:System.UIntPtr?displayProperty=nameWithType>|

<span data-ttu-id="69242-134">在除最后两行之外的所有表行中，最左侧列中的每个 C# 类型关键字都是相应 .NET 类型的别名。</span><span class="sxs-lookup"><span data-stu-id="69242-134">In all of the table rows except the last two, each C# type keyword from the leftmost column is an alias for the corresponding .NET type.</span></span> <span data-ttu-id="69242-135">关键字和 .NET 类型名称是可互换的。</span><span class="sxs-lookup"><span data-stu-id="69242-135">The keyword and .NET type name are interchangeable.</span></span> <span data-ttu-id="69242-136">例如，以下声明声明了相同类型的变量：</span><span class="sxs-lookup"><span data-stu-id="69242-136">For example, the following declarations declare variables of the same type:</span></span>

```csharp
int a = 123;
System.Int32 b = 123;
```

<span data-ttu-id="69242-137">表的最后两行中的 `nint` 和 `nuint` 类型是本机大小的整数。</span><span class="sxs-lookup"><span data-stu-id="69242-137">The `nint` and `nuint` types in the last two rows of the table are native-sized integers.</span></span> <span data-ttu-id="69242-138">在内部它们由所指示的 .NET 类型表示，但在任意情况下关键字和 .NET 类型都是不可互换的。</span><span class="sxs-lookup"><span data-stu-id="69242-138">They are represented internally by the indicated .NET types, but in each case the keyword and the .NET type are not interchangeable.</span></span> <span data-ttu-id="69242-139">编译器为 `nint` 和 `nuint` 的整数类型提供操作和转换，而不为指针类型 `System.IntPtr` 和 `System.UIntPtr` 提供。</span><span class="sxs-lookup"><span data-stu-id="69242-139">The compiler provides operations and conversions for `nint` and `nuint` as integer types that it doesn't provide for the pointer types `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="69242-140">有关详细信息，请参阅 [`nint` 和 `nuint` 类型](nint-nuint.md)。</span><span class="sxs-lookup"><span data-stu-id="69242-140">For more information, see [`nint` and `nuint` types](nint-nuint.md).</span></span>

<span data-ttu-id="69242-141">每个整型类型的默认值都为零 `0`。</span><span class="sxs-lookup"><span data-stu-id="69242-141">The default value of each integral type is zero, `0`.</span></span> <span data-ttu-id="69242-142">除本机大小的类型外，每个整型类型都有 `MinValue` 和 `MaxValue` 常量，提供该类型的最小值和最大值。</span><span class="sxs-lookup"><span data-stu-id="69242-142">Each of the integral types except the native-sized types has `MinValue` and `MaxValue` constants that provide the minimum and maximum value of that type.</span></span>

<span data-ttu-id="69242-143"><xref:System.Numerics.BigInteger?displayProperty=nameWithType> 结构用于表示没有上限或下限的带符号整数。</span><span class="sxs-lookup"><span data-stu-id="69242-143">Use the <xref:System.Numerics.BigInteger?displayProperty=nameWithType> structure to represent a signed integer with no upper or lower bounds.</span></span>

## <a name="integer-literals"></a><span data-ttu-id="69242-144">整数文本</span><span class="sxs-lookup"><span data-stu-id="69242-144">Integer literals</span></span>

<span data-ttu-id="69242-145">整数文本可以是</span><span class="sxs-lookup"><span data-stu-id="69242-145">Integer literals can be</span></span>

- <span data-ttu-id="69242-146"> 十进制：不使用任何前缀</span><span class="sxs-lookup"><span data-stu-id="69242-146">*decimal*: without any prefix</span></span>
- <span data-ttu-id="69242-147"> 十六进制：使用 `0x` 或 `0X` 前缀</span><span class="sxs-lookup"><span data-stu-id="69242-147">*hexadecimal*: with the `0x` or `0X` prefix</span></span>
- <span data-ttu-id="69242-148"> 二进制：使用 `0b` 或 `0B` 前缀（在 C# 7.0 和更高版本中可用）</span><span class="sxs-lookup"><span data-stu-id="69242-148">*binary*: with the `0b` or `0B` prefix (available in C# 7.0 and later)</span></span>

<span data-ttu-id="69242-149">下面的代码演示每种类型的示例：</span><span class="sxs-lookup"><span data-stu-id="69242-149">The following code demonstrates an example of each:</span></span>

```csharp
var decimalLiteral = 42;
var hexLiteral = 0x2A;
var binaryLiteral = 0b_0010_1010;
```

<span data-ttu-id="69242-150">前面的示例还演示了如何将 `_` 用作数字分隔符（从 C# 7.0 开始提供支持）。</span><span class="sxs-lookup"><span data-stu-id="69242-150">The preceding example also shows the use of `_` as a *digit separator*, which is supported starting with C# 7.0.</span></span> <span data-ttu-id="69242-151">可以将数字分隔符用于所有类型的数字文本。</span><span class="sxs-lookup"><span data-stu-id="69242-151">You can use the digit separator with all kinds of numeric literals.</span></span>

<span data-ttu-id="69242-152">整数文本的类型由其后缀确定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="69242-152">The type of an integer literal is determined by its suffix as follows:</span></span>

- <span data-ttu-id="69242-153">如果文本没有后缀，则其类型为以下类型中可表示其值的第一个类型：`int`、`uint`、`long`、`ulong`。</span><span class="sxs-lookup"><span data-stu-id="69242-153">If the literal has no suffix, its type is the first of the following types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>

  > [!NOTE]
  > <span data-ttu-id="69242-154">文本解释为正值。</span><span class="sxs-lookup"><span data-stu-id="69242-154">Literals are interpreted as positive values.</span></span> <span data-ttu-id="69242-155">例如，文本 `0xFF_FF_FF_FF` 表示 `uint` 类型的数字 `4294967295`，但其位表现形式与 `int` 类型的数字 `-1` 相同。</span><span class="sxs-lookup"><span data-stu-id="69242-155">For example, the literal `0xFF_FF_FF_FF` represents the number `4294967295` of the `uint` type, though it has the same bit representation as the number `-1` of the `int` type.</span></span> <span data-ttu-id="69242-156">如果需要特定类型的值，请将文本强制转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="69242-156">If you need a value of a certain type, cast a literal to that type.</span></span> <span data-ttu-id="69242-157">如果文本值无法以目标类型表示，请使用运算符 `unchecked`。</span><span class="sxs-lookup"><span data-stu-id="69242-157">Use the `unchecked` operator, if a literal value cannot be represented in the target type.</span></span> <span data-ttu-id="69242-158">示例：`unchecked((int)0xFF_FF_FF_FF)` 生成 `-1`。</span><span class="sxs-lookup"><span data-stu-id="69242-158">For example, `unchecked((int)0xFF_FF_FF_FF)` produces `-1`.</span></span>

- <span data-ttu-id="69242-159">如果文本以 `U` 或 `u` 为后缀，则其类型为以下类型中可表示其值的第一个类型：`uint`、`ulong`。</span><span class="sxs-lookup"><span data-stu-id="69242-159">If the literal is suffixed by `U` or `u`, its type is the first of the following types in which its value can be represented: `uint`, `ulong`.</span></span>
- <span data-ttu-id="69242-160">如果文本以 `L` 或 `l` 为后缀，则其类型为以下类型中可表示其值的第一个类型：`long`、`ulong`。</span><span class="sxs-lookup"><span data-stu-id="69242-160">If the literal is suffixed by `L` or `l`, its type is the first of the following types in which its value can be represented: `long`, `ulong`.</span></span>

  > [!NOTE]
  > <span data-ttu-id="69242-161">可以使用小写字母 `l` 作为后缀。</span><span class="sxs-lookup"><span data-stu-id="69242-161">You can use the lowercase letter `l` as a suffix.</span></span> <span data-ttu-id="69242-162">但是，这会生成一个编译器警告，因为字母 `l` 可能与数字 `1` 混淆。</span><span class="sxs-lookup"><span data-stu-id="69242-162">However, this generates a compiler warning because the letter `l` can be confused with the digit `1`.</span></span> <span data-ttu-id="69242-163">为清楚起见，请使用 `L`。</span><span class="sxs-lookup"><span data-stu-id="69242-163">Use `L` for clarity.</span></span>

- <span data-ttu-id="69242-164">如果文本的后缀为 `UL`、`Ul`、`uL`、`ul`、`LU`、`Lu`、`lU` 或 `lu`，则其类型为 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="69242-164">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, its type is `ulong`.</span></span>

<span data-ttu-id="69242-165">如果由整数字面量所表示的值超出了 <xref:System.UInt64.MaxValue?displayProperty=nameWithType>，则将出现编译器错误 [CS1021](../../misc/cs1021.md)。</span><span class="sxs-lookup"><span data-stu-id="69242-165">If the value represented by an integer literal exceeds <xref:System.UInt64.MaxValue?displayProperty=nameWithType>, a compiler error [CS1021](../../misc/cs1021.md) occurs.</span></span>

<span data-ttu-id="69242-166">如果确定的整数文本的类型为 `int`，且文本所表示的值位于目标类型的范围内，则该值可以隐式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`、`nint` 或 `nuint`：</span><span class="sxs-lookup"><span data-stu-id="69242-166">If the determined type of an integer literal is `int` and the value represented by the literal is within the range of the destination type, the value can be implicitly converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, `nint` or `nuint`:</span></span>

```csharp
byte a = 17;
byte b = 300;   // CS0031: Constant value '300' cannot be converted to a 'byte'
```

<span data-ttu-id="69242-167">如前面的示例所示，如果文本的值不在目标类型的范围内，则发生编译器错误 [CS0031](../../misc/cs0031.md)。</span><span class="sxs-lookup"><span data-stu-id="69242-167">As the preceding example shows, if the literal's value is not within the range of the destination type, a compiler error [CS0031](../../misc/cs0031.md) occurs.</span></span>

<span data-ttu-id="69242-168">还可以使用强制转换将整数文本所表示的值转换为除确定的文本类型之外的类型：</span><span class="sxs-lookup"><span data-stu-id="69242-168">You can also use a cast to convert the value represented by an integer literal to the type other than the determined type of the literal:</span></span>

```csharp
var signedByte = (sbyte)42;
var longVariable = (long)42;
```

## <a name="conversions"></a><span data-ttu-id="69242-169">转换</span><span class="sxs-lookup"><span data-stu-id="69242-169">Conversions</span></span>

<span data-ttu-id="69242-170">可以将任何整型数值类型转换为其他整数数值类型。</span><span class="sxs-lookup"><span data-stu-id="69242-170">You can convert any integral numeric type to any other integral numeric type.</span></span> <span data-ttu-id="69242-171">如果目标类型可以存储源类型的所有值，则转换是隐式的。</span><span class="sxs-lookup"><span data-stu-id="69242-171">If the destination type can store all values of the source type, the conversion is implicit.</span></span> <span data-ttu-id="69242-172">否则，需要使用[强制转换表达式](../operators/type-testing-and-cast.md#cast-expression)来执行显式转换。</span><span class="sxs-lookup"><span data-stu-id="69242-172">Otherwise, you need to use a [cast expression](../operators/type-testing-and-cast.md#cast-expression) to perform an explicit conversion.</span></span> <span data-ttu-id="69242-173">有关详细信息，请参阅[内置数值转换](numeric-conversions.md)。</span><span class="sxs-lookup"><span data-stu-id="69242-173">For more information, see [Built-in numeric conversions](numeric-conversions.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="69242-174">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="69242-174">C# language specification</span></span>

<span data-ttu-id="69242-175">有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：</span><span class="sxs-lookup"><span data-stu-id="69242-175">For more information, see the following sections of the [C# language specification](~/_csharplang/spec/introduction.md):</span></span>

- [<span data-ttu-id="69242-176">整型类型</span><span class="sxs-lookup"><span data-stu-id="69242-176">Integral types</span></span>](~/_csharplang/spec/types.md#integral-types)
- [<span data-ttu-id="69242-177">整数文本</span><span class="sxs-lookup"><span data-stu-id="69242-177">Integer literals</span></span>](~/_csharplang/spec/lexical-structure.md#integer-literals)

## <a name="see-also"></a><span data-ttu-id="69242-178">请参阅</span><span class="sxs-lookup"><span data-stu-id="69242-178">See also</span></span>

- [<span data-ttu-id="69242-179">C# 参考</span><span class="sxs-lookup"><span data-stu-id="69242-179">C# reference</span></span>](../index.md)
- [<span data-ttu-id="69242-180">值类型</span><span class="sxs-lookup"><span data-stu-id="69242-180">Value types</span></span>](value-types.md)
- [<span data-ttu-id="69242-181">浮点类型</span><span class="sxs-lookup"><span data-stu-id="69242-181">Floating-point types</span></span>](floating-point-numeric-types.md)
- [<span data-ttu-id="69242-182">标准数字格式字符串</span><span class="sxs-lookup"><span data-stu-id="69242-182">Standard numeric format strings</span></span>](../../../standard/base-types/standard-numeric-format-strings.md)
- [<span data-ttu-id="69242-183">.NET 中的数字</span><span class="sxs-lookup"><span data-stu-id="69242-183">Numerics in .NET</span></span>](../../../standard/numerics.md)
