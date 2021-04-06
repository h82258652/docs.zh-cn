---
description: throw - C# 参考
title: throw - C# 参考
ms.date: 03/02/2015
f1_keywords:
- throw
- throw_CSharpKeyword
helpviewer_keywords:
- throw statement [C#]
- throw expression [C#]
- throw keyword [C#]
ms.assetid: 5ac4feef-4b1a-4c61-aeb4-61d549e5dd42
ms.openlocfilehash: e081bd86e57d69c85ea018d371b3e489145e958d
ms.sourcegitcommit: 872ca41d1c26f39d0aef57cc365d09503bac780d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106288038"
---
# <a name="throw-c-reference"></a>throw（C# 参考）

发出程序执行期间出现异常的信号。  
  
## <a name="remarks"></a>备注

`throw` 的语法为：

```csharp
throw [e];
```

`e` 是一个派生自 <xref:System.Exception?displayProperty=nameWithType> 的类的实例。 下例使用 `throw` 语句在传递给名为 `GetNumber` 的方法的参数与内部数组的有效索引不对应时引发  <xref:System.IndexOutOfRangeException> 。

[!code-csharp[csrefKeyword#1](~/samples/snippets/csharp/language-reference/keywords/throw/throw1/throw-1.cs#1)]

然后方法调用方使用 `try-catch` 或 `try-catch-finally` 块来处理引发的异常。 下例处理由 `GetNumber` 方法引发的异常。

[!code-csharp[csrefKeyword#2](~/samples/snippets/csharp/language-reference/keywords/throw/throw1/throw-1.cs#2)]

## <a name="re-throwing-an-exception"></a>重新引发异常

`throw` 也可以用于 `catch` 块，以重新引发在 `catch` 块中处理的异常。  在这种情况下，`throw` 不采用异常操作数。 当方法将参数从调用方传递给其他库方法时，这是最有用的，库方法引发的异常必须传递给调用方。 例如，以下示例重新引发在尝试检索未初始化字符串的第一个字符时引发的 <xref:System.NullReferenceException>。

[!code-csharp[csrefKeyword#3](~/samples/snippets/csharp/language-reference/keywords/throw/throw2/throw-2.cs#3)]

> [!IMPORTANT]
> 还可以使用 `catch` 块中的 `throw e` 语法来实例化传递给调用方的新异常。 在这种情况下，将不会保留可从 <xref:System.Exception.StackTrace> 属性获得的原始异常的堆栈跟踪。

## <a name="the-throw-expression"></a>`throw` 表达式

从 C# 7.0 开始，`throw` 可以用作表达式和语句。 这允许在以前不支持的上下文中引发异常。 这些方法包括：

- [条件运算符](../operators/conditional-operator.md)。 下例使用 `throw` 表达式在向方法传递空字符串数组时引发 <xref:System.ArgumentException>。 在 C# 7.0 之前，此逻辑将需要显示在 `if`/`else` 语句中。

   [!code-csharp[csrefKeyword#4](~/samples/snippets/csharp/language-reference/keywords/throw/conditional/conditional.cs#1)]

- [null 合并运算符](../operators/null-coalescing-operator.md)。 在以下示例中，如果分配给 `Name` 属性的字符串为 `null`，则将 `throw` 表达式与 null 合并运算符结合使用以引发异常。

   [!code-csharp[csrefKeyword#5](~/samples/snippets/csharp/language-reference/keywords/throw/coalescing/coalescing.cs#1)]

- expression-bodied [lambda](../operators/lambda-expressions.md) 或方法。 下例说明了 expression-bodied 方法，由于不支持对 <xref:System.DateTime> 值的转换，该方法引发 <xref:System.InvalidCastException>。

   [!code-csharp[csrefKeyword#6](~/samples/snippets/csharp/language-reference/keywords/throw/exp-bodied/exp-bodied.cs#1)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [try-catch](try-catch.md)
- [C# 关键字](index.md)
- [如何：显式抛出异常](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
