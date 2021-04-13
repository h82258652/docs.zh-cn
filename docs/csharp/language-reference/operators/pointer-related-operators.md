---
title: 指针相关运算符 - C# 参考
description: 了解在使用指针时可以使用的 C# 运算符。
ms.date: 05/20/2019
author: pkulikov
f1_keywords:
- ->_CSharpKeyword
helpviewer_keywords:
- pointer related operators [C#]
- address-of operator [C#]
- '& operator [C#]'
- pointer indirection operator [C#]
- dereference operator [C#]
- '* operator [C#]'
- pointer member access operator [C#]
- -> operator [C#]
- pointer element access [C#]
- '[] operator [C#]'
- pointer arithmetic [C#]
- pointer increment [C#]
- pointer decrement [C#]
- pointer comparison [C#]
ms.openlocfilehash: 22cd87a62c92dbfacd0b8ebbdd5cbafb92da73ca
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497354"
---
# <a name="pointer-related-operators-c-reference"></a>指针相关运算符（C# 参考）

可以使用以下运算符来使用指针：

- 一元 [`&` (address-of) ](#address-of-operator-)运算符：用于获取变量的地址
- 一元 [`*`（指针间接）](#pointer-indirection-operator-)运算符：用于获取指针指向的变量
- [`->`（成员访问）](#pointer-member-access-operator--)和 [`[]`（元素访问）](#pointer-element-access-operator-)运算符
- 算术运算符 [`+`、`-`、`++` 和 `--`](#pointer-arithmetic-operators)
- 比较运算符 [`==`、`!=`、`<`、`>`、`<=` 和 `>=`](#pointer-comparison-operators)

有关指针类型的信息，请参阅[指针类型](../unsafe-code.md#pointer-types)。

> [!NOTE]
> 任何带指针的运算都需要使用 [unsafe](../keywords/unsafe.md) 上下文。 必须使用 [AllowUnsafeBlocks](../compiler-options/language.md#allowunsafeblocks) 编译器选项来编译包含不安全块的代码。

## <a name="address-of-operator-amp"></a><a name="address-of-operator-"></a> Address-of 运算符 &amp;

一元 `&` 运算符返回其操作数的地址：

[!code-csharp[address of local](snippets/shared/PointerOperators.cs#AddressOf)]

`&` 运算符的操作数必须是固定变量。 固定变量是驻留在不受[垃圾回收器](../../../standard/garbage-collection/index.md)操作影响的存储位置的变量  。 在上述示例中，局部变量 `number` 是固定变量，因为它驻留在堆栈上。 驻留在可能受垃圾回收器影响的存储位置的变量（如重定位）称为可移动变量  。 对象字段和数组元素是可移动变量的示例。 如果使用 [`fixed` 语句](../keywords/fixed-statement.md)“固定”，则可以获取可移动变量的地址。 获取的地址仅在 `fixed` 语句块中有效。 以下示例显示如何使用 `fixed` 语句和 `&` 运算符：

[!code-csharp[address of fixed](snippets/shared/PointerOperators.cs#AddressOfFixed)]

你也无法获取常量或值的地址。

有关固定变量和可移动变量的详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[固定变量和可移动变量](~/_csharplang/spec/unsafe-code.md#fixed-and-moveable-variables)部分。

二进制 `&` 运算符计算其布尔操作数的[逻辑 AND](boolean-logical-operators.md#logical-and-operator-) 或其整型操作数的[位逻辑 AND](bitwise-and-shift-operators.md#logical-and-operator-)。

## <a name="pointer-indirection-operator-"></a>指针间接运算符 *

一元指针间接运算符 `*` 获取其操作数指向的变量。 它也称为取消引用运算符。 `*` 运算符的操作数必须是指针类型。

[!code-csharp[pointer indirection](snippets/shared/PointerOperators.cs#PointerIndirection)]

不能将 `*` 运算符应用于类型 `void*` 的表达式。

二进制 `*` 运算符计算其数值操作数的[乘积](arithmetic-operators.md#multiplication-operator-)。

## <a name="pointer-member-access-operator--"></a>指针成员访问运算符 ->

`->` 运算符将[指针间接](#pointer-indirection-operator-)和[成员访问](member-access-operators.md#member-access-expression-)合并。 也就是说，如果 `x` 是类型为 `T*` 的指针且 `y` 是类型 `T` 的可访问成员，则形式的表达式

```csharp
x->y
```

等效于

```csharp
(*x).y
```

下面的示例演示 `->` 运算符的用法：

[!code-csharp[pointer member access](snippets/shared/PointerOperators.cs#MemberAccess)]

不能将 `->` 运算符应用于类型 `void*` 的表达式。

## <a name="pointer-element-access-operator-"></a>指针元素访问运算符 []

对于指针类型的表达式 `p`，`p[n]` 形式的指针元素访问计算方式为 `*(p + n)`，其中 `n` 必须是可隐式转换为 `int`、`uint`、`long` 或 `ulong` 的类型。 有关带有指针的 `+` 运算符的行为的信息，请参阅[向指针中增加或从指针中减少整数值](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer)部分。

以下示例演示如何使用指针和 `[]` 运算符访问数组元素：

[!code-csharp[pointer element access](snippets/shared/PointerOperators.cs#ElementAccess)]

在前面的示例中，[`stackalloc` 表达式](stackalloc.md) 在堆栈上分配内存块。

> [!NOTE]
> 指针元素访问运算符不检查越界错误。

不能将 `[]` 用于带有类型为 `void*` 的表达式的指针元素访问。

还可以将 `[]` 运算符用于[数组元素或索引器访问](member-access-operators.md#indexer-operator-)。

## <a name="pointer-arithmetic-operators"></a>指针算术运算符

可以使用指针执行以下算术运算：

- 向指针增加或从指针中减少一个整数值
- 减少两个指针
- 增量或减量指针

无法使用类型为 `void*` 的指针执行这些操作。

有关带数值类型的受支持的算术运算的信息，请参阅[算术运算符](arithmetic-operators.md)。

### <a name="addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer"></a>向指针增加或从指针中减少整数值

对于类型为 `T*` 的指针`p`和可隐式转换为 `int`、`uint`、`long` 或 `ulong` 的类型的表达式 `n`，加法和减法定义如下：

- `p + n` 和 `n + p` 表达式都生成 `T*` 类型的指针，该指针是将 `n * sizeof(T)` 添加到 `p` 给出的地址得到的。
- `p - n` 表达式生成类型为 `T*` 的指针，该指针是从 `p` 给出的地址中减去 `n * sizeof(T)` 得到的。

[`sizeof` 运算符](sizeof.md)获取类型的大小（以字节为单位）。

以下示例演示了 `+` 运算符与指针的用法：

[!code-csharp[pointer addition](snippets/shared/PointerOperators.cs#AddNumber)]

### <a name="pointer-subtraction"></a>指针减法

对于类型为 `T*` 的两个指针 `p1` 和 `p2`，表达式 `p1 - p2` 生成 `p1` 和 `p2` 给出的地址之间的差除以 `sizeof(T)` 的值。 结果的类型为 `long`。 即 `p1 - p2` 计算公式为 `((long)(p1) - (long)(p2)) / sizeof(T)`。

以下示例演示了指针减法：

[!code-csharp[pointer subtraction](snippets/shared/PointerOperators.cs#SubtractPointers)]

### <a name="pointer-increment-and-decrement"></a>指针增量和减量

`++` 增量运算符将 1 [添加](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer)到其指针操作数。 `--` 减量运算符从其指针操作数中[减去](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer) 1。

两种运算符都支持两种形式：后缀（`p++` 和 `p--`）和前缀（`++p` 和 `--p`）。 `p++` 和 `p--` 的结果是该运算之前 `p` 的值  。 `++p` 和 `--p` 的结果是该运算之后 `p` 的值  。

以下示例演示了后缀和前缀增量运算符的行为：

[!code-csharp[pointer increment](snippets/shared/PointerOperators.cs#Increment)]

## <a name="pointer-comparison-operators"></a>指针比较运算符

可以使用 `==`、`!=`、`<`、`>`、`<=` 和 `>=` 运算符来比较任何指针类型（包括 `void*`）的操作数。 这些运算符将两个操作数给出的地址进行比较，就好像它们是无符号整数一样。

有关这些运算符对其他类型操作数的行为的信息，请参阅[等式运算符](equality-operators.md)和[比较运算符](comparison-operators.md)一文。

## <a name="operator-precedence"></a>运算符优先级

以下列表按优先级从高到低的顺序对指针相关运算符进行排序：

- 后缀增量 `x++` 和减量 `x--` 运算符以及 `->` 和 `[]` 运算符
- 前缀增量 `++x` 和减量 `--x` 运算符以及 `&` 和 `*` 运算符
- 加法 `+` 和 `-` 运算符
- 比较 `<`、`>`、`<=` 和 `>=` 运算符
- 等式 `==` 和 `!=` 运算符

使用括号 `()` 更改运算符优先级所施加的计算顺序。

如需了解按优先级排序的完整 C# 运算符列表，请参阅 [C# 运算符](index.md#operator-precedence)一文中的[运算符优先级](index.md)部分。

## <a name="operator-overloadability"></a>运算符可重载性

用户定义的类型不能重载与指针相关的运算符 `&`、`*`、`->` 和 `[]`。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [固定和可移动变量](~/_csharplang/spec/unsafe-code.md#fixed-and-moveable-variables)
- [address-of 运算符](~/_csharplang/spec/unsafe-code.md#the-address-of-operator)
- [指针间接](~/_csharplang/spec/unsafe-code.md#pointer-indirection)
- [指针成员访问](~/_csharplang/spec/unsafe-code.md#pointer-member-access)
- [指针元素访问](~/_csharplang/spec/unsafe-code.md#pointer-element-access)
- [指针算术](~/_csharplang/spec/unsafe-code.md#pointer-arithmetic)
- [指针增量和减量](~/_csharplang/spec/unsafe-code.md#pointer-increment-and-decrement)
- [指针比较](~/_csharplang/spec/unsafe-code.md#pointer-comparison)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [指针类型](../unsafe-code.md#pointer-types)
- [unsafe 关键字](../keywords/unsafe.md)
- [fixed 关键字](../keywords/fixed-statement.md)
- [stackalloc](stackalloc.md)
- [sizeof 运算符](sizeof.md)
