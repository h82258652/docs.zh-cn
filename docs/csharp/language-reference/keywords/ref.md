---
description: ref 关键字 - C# 参考
title: ref 关键字 - C# 参考
ms.date: 03/29/2021
f1_keywords:
- ref_CSharpKeyword
helpviewer_keywords:
- parameters [C#], ref
- ref keyword [C#]
ms.openlocfilehash: 3392e560eaf0bac39cf4e9707574fd2bb3d96057
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079435"
---
# <a name="ref-c-reference"></a>ref（C# 参考）

`ref` 关键字指示按引用传递值。 它用在四种不同的上下文中：

- 在方法签名和方法调用中，按引用将参数传递给方法。 有关详细信息，请参阅[按引用传递参数](#passing-an-argument-by-reference)。
- 在方法签名中，按引用将值返回给调用方。 有关详细信息，请参阅[引用返回值](#reference-return-values)。
- 在成员正文中，指示引用返回值是否作为调用方欲修改的引用被存储在本地。 或指示局部变量按引用访问另一个值。 有关详细信息，请参阅 [Ref 局部变量](#ref-locals)。
- 在 `struct` 声明中，声明 `ref struct` 或 `readonly ref struct`。 有关详细信息，请参阅[结构类型](../builtin-types/struct.md)一文中的 [`ref` 结构](../builtin-types/struct.md#ref-struct)一节。

## <a name="passing-an-argument-by-reference"></a>按引用传递参数

在方法的参数列表中使用 `ref` 关键字时，它指示参数按引用传递，而非按值传递。 `ref` 关键字让形参成为实参的别名，这必须是变量。 换而言之，对形参执行的任何操作都是对实参执行的。

例如，假设调用方传递了局部变量表达式或数组元素访问表达式。 然后，调用的方法可以替换 ref 参数引用的对象。 在这种情况下，调用方的局部变量或数组元素会在该方法返回时引用新的对象。

> [!NOTE]
> 不要混淆通过引用传递的概念与引用类型的概念。 这两种概念是不同的。 无论方法参数是值类型还是引用类型，均可由 `ref` 修改。 当通过引用传递时，不会对值类型装箱。  

若要使用 `ref` 参数，方法定义和调用方法均必须显式使用 `ref` 关键字，如下面的示例所示。 （除了在进行 COM 调用时，调用方法可忽略 `ref`。）

[!code-csharp-interactive[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#1)]

传递到 `ref` 或 `in` 形参的实参必须先经过初始化，然后才能传递。 该要求与 [out](out-parameter-modifier.md) 形参不同，在传递之前，不需要显式初始化该形参的实参。

类的成员不能具有仅在 `ref`、`in` 或 `out` 方面不同的签名。 如果类型的两个成员之间的唯一区别在于其中一个具有 `ref` 参数，而另一个具有 `out` 或 `in` 参数，则会发生编译器错误。 例如，以下代码将不会编译。  

```csharp
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded
    // methods that differ only on ref and out".
    public void SampleMethod(out int i) { }
    public void SampleMethod(ref int i) { }
}
```

但是，当一个方法具有 `ref`、`in` 或 `out` 参数，另一个方法具有值传递的参数时，则可以重载方法，如下面的示例所示。
  
[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#2)]
  
 在其他要求签名匹配的情况下（如隐藏或重写），`in`、`ref` 和 `out` 是签名的一部分，相互之间不匹配。  
  
 属性不是变量。 它们是方法，不能传递到 `ref` 参数。  
  
 不能将 `ref`、`in` 和 `out` 关键字用于以下几种方法：  
  
- 异步方法，通过使用 [async](async.md) 修饰符定义。  
- 迭代器方法，包括 [yield return](yield.md) 或 `yield break` 语句。

[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)还限制了对以下这些关键字的使用：

- 不能对扩展方法的第一个参数使用 `out` 关键字。
- 当参数不是结构或是不被约束为结构的泛型类型时，不能对扩展方法的第一个参数使用 `ref` 关键字。
- 除非第一个参数是结构，否则不能使用 `in` 关键字。 即使约束为结构，也不能对任何泛型类型使用 `in` 关键字。

## <a name="passing-an-argument-by-reference-an-example"></a>按引用传递参数：示例

前面的示例按引用传递值类型。 还可使用 `ref` 关键字按引用传递引用类型。 按引用传递引用类型使所调用方能够替换调用方中引用参数引用的对象。 对象的存储位置按引用参数的值传递到方法。 如果更改参数存储位置中的值（以指向新对象），你还可以将存储位置更改为调用方所引用的位置。 下面的示例将引用类型的实例作为 `ref` 参数传递。
  
[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#3)]

有关如何通过值和引用传递引用类型的详细信息，请参阅[传递引用类型参数](../../programming-guide/classes-and-structs/passing-reference-type-parameters.md)。
  
## <a name="reference-return-values"></a>引用返回值

引用返回值（或 ref 返回值）是由方法按引用向调用方返回的值。 即是说，调用方可以修改方法所返回的值，此更改反映在调用方法中的对象的状态中。

使用 `ref` 关键字来定义引用返回值：

- 在方法签名中。 例如，下列方法签名指示 `GetCurrentPrice` 方法按引用返回了 <xref:System.Decimal> 值。

```csharp
public ref decimal GetCurrentPrice()
```

- 在 `return` 标记和方法的 `return` 语句中返回的变量之间。 例如：

```csharp
return ref DecimalArray[0];
```

为方便调用方修改对象的状态，引用返回值必须存储在被显式定义为 [ref 局部变量](#ref-locals)的变量中。

下面是一个更完整的 ref 返回示例，同时显示方法签名和方法主体。

[!code-csharp[FindReturningRef](~/samples/snippets/csharp/new-in-7/MatrixSearch.cs#FindReturningRef "Find returning by reference")]

所调用方法还可能会将返回值声明为 `ref readonly` 以按引用返回值，并坚持调用代码无法修改返回的值。 调用方法可以通过将返回值存储在本地 [ref readonly](#ref-readonly-locals) 变量中来避免复制该值。

有关示例，请参阅 [ref 返回值和 ref 局部变量示例](#a-ref-returns-and-ref-locals-example)。

## <a name="ref-locals"></a>ref 局部变量

ref 局部变量用于指代使用 `return ref` 返回的值。 无法将 ref 局部变量初始化为非 ref 返回值。 也就是说，初始化的右侧必须为引用。 任何对 ref 本地变量值的修改都将反映在对象的状态中，该对象的方法按引用返回值。

可在以下两个位置使用 `ref` 关键字来定义 ref 局部变量：

- 在变量声明之前。
- 紧接在调用按引用返回值的方法之前。

例如，下列语句定义名为 `GetEstimatedValue` 的方法返回的 ref 局部变量值：

```csharp
ref decimal estValue = ref Building.GetEstimatedValue();
```

可通过相同方式按引用访问值。 在某些情况下，按引用访问值可避免潜在的高开销复制操作，从而提高性能。 例如，以下语句显示如何定义一个用于引用值的 ref 局部变量。

```csharp
ref VeryLargeStruct reflocal = ref veryLargeStruct;
```

在这两个示例中，必须在两个位置同时使用 `ref` 关键字，否则编译器将生成错误 CS8172：“无法使用值对按引用变量进行初始化”。

从 C# 7.3 开始，`foreach` 语句的迭代变量可以是 ref 局部变量，也可以是 ref readonly 局部变量。 有关详细信息，请参阅 [foreach 语句](foreach-in.md)一文。

此外，从 C# 7.3 开始，可以使用 [ref 赋值运算符](../operators/assignment-operator.md#ref-assignment-operator)重新分配 ref local 或 ref readonly local 变量。

## <a name="ref-readonly-locals"></a>Ref readonly 局部变量

Ref readonly 局部变量用于指代在其签名中具有 `ref readonly` 并使用 `return ref` 的方法或属性返回的值。 `ref readonly` 变量将 `ref` 局部变量的属性与 `readonly` 变量结合使用：它是所分配到的存储的别名，且无法修改。

## <a name="a-ref-returns-and-ref-locals-example"></a>ref 返回值和 ref 局部变量示例

下列示例定义一个具有两个 <xref:System.String> 字段（`Title` 和 `Author`）的 `Book` 类。 还定义包含 `Book` 对象的专用数组的 `BookCollection` 类。 通过调用 `GetBookByTitle` 方法，可按引用返回个别 book 对象。

[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#4)]

调用方将 `GetBookByTitle` 方法所返回的值存储为 ref 局部变量时，调用方对返回值所做的更改将反映在 `BookCollection` 对象中，如下例所示。

[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#5)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>请参阅

- [编写安全高效的代码](../../write-safe-efficient-code.md)
- [ref 返回结果和局部变量](../../programming-guide/classes-and-structs/ref-returns.md)
- [条件 ref 表达式](../operators/conditional-operator.md#conditional-ref-expression)
- [传递参数](../../programming-guide/classes-and-structs/passing-parameters.md)
- [方法参数](method-parameters.md)
- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
