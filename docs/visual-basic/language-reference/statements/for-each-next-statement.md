---
description: '了解有关以下各项的详细信息： .。。下一条语句 (Visual Basic) '
title: For Each...Next 语句
ms.date: 07/20/2015
f1_keywords:
- vb.ForEach
- vb.ForEachNext
- vb.Each
- ForEachNext
helpviewer_keywords:
- infinite loops
- Next statement [Visual Basic], For Each...Next
- endless loops
- loop structures [Visual Basic], For Each...Next
- loops, endless
- Each keyword [Visual Basic]
- instructions, repeating
- For Each statement [Visual Basic]
- collections, instruction repetition
- loops, infinite
- For Each...Next statements
- For keyword [Visual Basic], For Each...Next statements
- Exit statement [Visual Basic], For Each...Next statements
- iteration
ms.assetid: ebce3120-95c3-42b1-b70b-fa7da40c75e2
ms.openlocfilehash: 85fb426fdc2d7961603a3ddcc882f9fdce0c70a2
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079617"
---
# <a name="for-eachnext-statement-visual-basic"></a>For Each...Next 语句 (Visual Basic)

为集合中的每个元素重复一组语句。

## <a name="syntax"></a>语法

```vb
For Each element [ As datatype ] In group
    [ statements ]
    [ Continue For ]
    [ statements ]
    [ Exit For ]
    [ statements ]
Next [ element ]
```

## <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`element`|语句中必需 `For Each` 。 在语句中是可选的 `Next` 。 变量。 用于循环访问集合中的元素。|
|`datatype`|如果 [`Option Infer`](option-infer-statement.md) (为默认) 或已声明，则为可选 `element` ; 如果 `Option Infer` 为 off，并且尚未声明，则为必需 `element` 。 `element` 的数据类型。|
|`group`|必需。 类型为集合类型或对象的变量。 引用要在其上重复的集合 `statements` 。|
|`statements`|可选。 在和之间运行的一个或多个语句 `For Each` `Next` `group` 。|
|`Continue For`|可选。 将控制转移到循环的起始处 `For Each` 。|
|`Exit For`|可选。 将控制转移到 `For Each` 循环外。|
|`Next`|必需。 终止循环的定义 `For Each` 。|

## <a name="simple-example"></a>简单示例

`For Each` `Next` 若要为集合或数组的每个元素重复一组语句，请使用 ... 循环。

> [!TIP]
> [用于 .。。](for-next-statement.md)如果可以将循环的每次迭代与控件变量相关联，并确定该变量的初始值，则下一语句会很好地运行。 但是，在处理集合时，初始值和 final 值的概念没有意义，并且您不一定知道该集合具有多少元素。 在这种情况下，" `For Each` ..." `Next` 循环通常是更好的选择。

在 `For Each` 下面的示例中，"..."`Next` 语句循环访问列表集合的所有元素。

:::code language="vb" source="./snippets/for-each-next-statement/vb/Examples.vb" id="init":::

有关更多示例，请参阅 [集合](../../../standard/collections/index.md) 和 [数组](../../programming-guide/language-features/arrays/index.md)。

## <a name="nested-loops"></a>Nested Loops

可以 `For Each` 通过在另一个循环中放置循环来嵌套循环。

下面的示例演示了嵌套 `For Each` .。。`Next` 构造.

:::code language="vb" source="./snippets/for-each-next-statement/vb/Examples.vb" id="nested":::

嵌套循环时，每个循环必须具有唯一 `element` 变量。

您还可以在彼此之间嵌套不同种类的控制结构。 有关详细信息，请参阅 [嵌套控制结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)。

## <a name="exit-for-and-continue-for"></a>退出并继续进行

[Exit For](exit-statement.md)语句导致执行退出 `For` .。。`Next` 循环并将控制转移到语句后面的语句 `Next` 。

`Continue For`语句将控制立即传输到循环的下一次迭代。 有关详细信息，请参阅 [Continue 语句](continue-statement.md)。

下面的示例演示如何使用 `Continue For` 和 `Exit For` 语句。

:::code language="vb" source="./snippets/for-each-next-statement/vb/Examples.vb" id="exitcontinue":::

可以将任意数量的语句放入 `Exit For` `For Each` 循环中。 在嵌套循环内使用时 `For Each` ， `Exit For` 使执行退出最内层的循环，并将控制转移到下一个更高的嵌套级别。

`Exit For` 通常在对某些条件进行计算后使用，例如，在 `If` ... `Then`...`Else` 构造. 你可能想要将 `Exit For` 用于以下情况：

- 不需要继续循环访问。 这可能是由错误值或终止请求引起的。

- 在 `Try` ... `Catch` 中捕获到异常...`Finally`.可以 `Exit For` 在块的末尾使用 `Finally` 。

- 有一个无限循环，该循环是一种循环，它可以运行很大甚至无限次。 如果检测到这种情况，则可以使用 `Exit For` 来转义循环。 有关详细信息，请参阅 [Do .。。循环语句](do-loop-statement.md)。

## <a name="iterators"></a>迭代器

使用 *迭代器* 对集合执行自定义迭代。 迭代器可以是函数或 `Get` 访问器。 它使用 `Yield` 语句一次返回集合中的每个元素。

使用语句调用迭代器 `For Each...Next` 。 `For Each` 循环的每次迭代都会调用迭代器。 `Yield`在迭代器中到达语句时，将返回语句中的表达式 `Yield` ，并保留当前在代码中的位置。 下次调用迭代器时，将从该位置重新开始执行。

下面的示例使用迭代器函数。 迭代器函数包含的 `Yield` 语句位于 [For .。。下一个](for-next-statement.md) 循环。 在 `ListEvenNumbers` 方法中，语句体的每次迭代 `For Each` 都会创建对迭代器函数的调用，该函数将继续执行下一 `Yield` 条语句。

:::code language="vb" source="./snippets/for-each-next-statement/vb/Examples.vb" id="iterator":::

有关详细信息，请参阅 [迭代](../../programming-guide/concepts/iterators.md)器、 [Yield 语句](yield-statement.md)和 [迭代器](../modifiers/iterator.md)。

## <a name="technical-implementation"></a>技术实现

当 `For Each` .。。`Next` 语句运行时，Visual Basic 在循环开始之前仅计算一次集合。 如果语句块发生更改 `element` `group` ，则这些更改不会影响循环的迭代。

如果集合中的所有元素连续分配到 `element` ，则 `For Each` 循环停止并控制传递到语句后面的语句 `Next` 。

如果 [选项推断](option-infer-statement.md) (其默认设置) ，则 Visual Basic 编译器可以推断的数据类型 `element` 。 如果它处于关闭状态且未 `element` 在循环外声明，则必须在语句中声明它 `For Each` 。 若要显式声明的数据类型 `element` ，请使用 `As` 子句。 除非在 ... 构造之外定义元素的数据类型 `For Each` `Next` ，否则其作用域为循环的主体。 请注意，不能在 `element` 循环的外部和内部声明。

您可以选择 `element` 在语句中指定 `Next` 。 这会提高程序的可读性，尤其是在有嵌套循环的情况下 `For Each` 。 您必须指定与相应语句中显示的变量相同的变量 `For Each` 。

你可能希望避免在循环内更改的值 `element` 。 这样做可以更难读取和调试代码。 更改的值 `group` 不会影响集合或其元素，这是在第一次进入循环时确定的。

嵌套循环时，如果在 `Next` 内部级别之前遇到外部嵌套级别的语句 `Next` ，则编译器会发出错误消息。 但是，只有在 `element` 每个语句中指定时，编译器才能检测到此重叠错误 `Next` 。

如果你的代码依赖于遍历特定顺序的集合，则 `For Each` `Next` 不是最佳选择，除非你知道该集合公开的枚举器对象的特征。 遍历的顺序并不由 Visual Basic 确定，而是由 <xref:System.Collections.IEnumerator.MoveNext%2A> 枚举器对象的方法决定。 因此，你可能无法预测集合中第一个要返回的元素 `element` ，或者在给定元素后返回的下一个元素。 使用其他循环结构（例如 `For` ... `Next` 或 `Do` ...）可以获得更可靠的 `Loop` 结果。

运行时必须能够将中的元素转换 `group` 为 `element` 。 [ `Option Strict` ] 语句控制是否允许扩大和收缩转换 (`Option Strict` 关闭、其默认值) ，或者是否只允许 `Option Strict` 在) 上使用扩大转换 (。 有关详细信息，请参阅 [收缩转换](#narrowing-conversions)。

的数据类型 `group` 必须为引用类型，该引用类型引用可枚举的集合或数组。 通常，这意味着 `group` 引用实现 <xref:System.Collections.IEnumerable> `System.Collections` 命名空间的接口或 <xref:System.Collections.Generic.IEnumerable%601> `System.Collections.Generic` 命名空间接口的对象。 `System.Collections.IEnumerable` 定义 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法，该方法返回集合的枚举器对象。 枚举器对象实现 `System.Collections.IEnumerator` 命名空间的接口 `System.Collections` ，并公开 <xref:System.Collections.IEnumerator.Current%2A> 属性和 <xref:System.Collections.IEnumerator.Reset%2A> 和 <xref:System.Collections.IEnumerator.MoveNext%2A> 方法。 Visual Basic 使用它们遍历集合。

### <a name="narrowing-conversions"></a>收缩转换

当 `Option Strict` 设置为时 `On` ，收缩转换通常会导致编译器错误。 但是，在语句中，将 `For Each` 在 `group` 运行时计算和执行从中的元素到的转换 `element` ，并取消收缩转换导致的编译器错误。

在下面的示例中，当为 `m` on 时，的赋值 `n` 不会进行编译， `Option Strict` 因为从到的转换 `Long` `Integer` 是收缩转换。 但是，在语句中， `For Each` 不会报告编译器错误，即使赋值 `number` 需要从到的相同转换 `Long` `Integer` 。 在 `For Each` 包含大量的语句中，当应用于大数值时，将发生运行时错误 <xref:Microsoft.VisualBasic.CompilerServices.Conversions.ToInteger%2A> 。

:::code language="vb" source="./snippets/for-each-next-statement/vb/Program.vb":::

### <a name="ienumerator-calls"></a>IEnumerator 调用

当执行 `For Each` ... `Next` 循环开始时，Visual Basic 验证是否 `group` 引用了有效的集合对象。 如果不是，则会引发异常。 否则，它会调用 <xref:System.Collections.IEnumerator.MoveNext%2A> 方法和 <xref:System.Collections.IEnumerator.Current%2A> 枚举器对象的属性以返回第一个元素。 如果 `MoveNext` 指示没有下一个元素（即，如果集合为空），则 `For Each` 循环停止，并将控制传递给语句后面的语句 `Next` 。 否则，Visual Basic 设置 `element` 为第一个元素，并运行语句块。

每次 Visual Basic 遇到 `Next` 语句时，它都会返回到 `For Each` 语句。 再次调用 `MoveNext` 并 `Current` 返回下一个元素，并再次运行该块或停止循环，这取决于结果。 此过程将继续 `MoveNext` ，直到指示没有下一个元素或 `Exit For` 遇到语句。

**修改集合。** 通常，返回的枚举器对象 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 不允许您通过添加、删除、替换或重新排序任何元素来更改集合。 如果你在启动 ... 循环后更改集合 `For Each` `Next` ，则枚举器对象将变为无效，并且下一次尝试访问元素时将导致 <xref:System.InvalidOperationException> 异常。

但是，这种修改不会由 Visual Basic 确定，而是由接口的实现来决定 <xref:System.Collections.IEnumerable> 。 可以通过 `IEnumerable` 允许在迭代期间进行修改的方式实现。 如果考虑执行此类动态修改，请确保在所 `IEnumerable` 使用的集合上了解实现的特征。

**修改集合元素。** <xref:System.Collections.IEnumerator.Current%2A>枚举器对象的属性是[只读](../modifiers/readonly.md)的，它返回每个集合元素的本地副本。 这意味着不能在 `For Each` ... 循环中修改元素本身。 `Next` 您所做的任何修改只会影响的本地副本 `Current` ，并且不会反映回基础集合。 但是，如果元素是引用类型，则可以修改它所指向的实例的成员。 下面的示例修改 `BackColor` 每个元素的成员 `thisControl` 。 但不能修改 `thisControl` 自身。

```vb
Sub LightBlueBackground(thisForm As System.Windows.Forms.Form)
    For Each thisControl In thisForm.Controls
        thisControl.BackColor = System.Drawing.Color.LightBlue
    Next thisControl
End Sub
```

前面的示例可以修改 `BackColor` 每个元素的成员 `thisControl` ，但它不能修改 `thisControl` 自身。

**遍历数组。** 由于 <xref:System.Array> 类实现 <xref:System.Collections.IEnumerable> 接口，因此所有数组都公开了 <xref:System.Array.GetEnumerator%2A> 方法。 这意味着可以使用 `For Each` ... 循环来循环访问数组。 `Next` 但是，只能读取数组元素。 不能更改它们。

## <a name="example"></a>示例

下面的示例列出了 C：\ 中的所有文件夹目录 <xref:System.IO.DirectoryInfo> 。

:::code language="vb" source="./snippets/for-each-next-statement/vb/Examples.vb" id="foreachdir":::

## <a name="example"></a>示例

以下示例阐释了对集合排序的过程。 该示例对中存储的类的实例进行排序 `Car` <xref:System.Collections.Generic.List%601> 。 `Car` 类实现 <xref:System.IComparable%601> 接口，此操作需要实现 <xref:System.IComparable%601.CompareTo%2A> 方法。

对方法的每次调用 <xref:System.IComparable%601.CompareTo%2A> 都会进行一个用于排序的比较。 `CompareTo` 方法中用户编写的代码针对当前对象与另一个对象的每个比较返回一个值。 如果当前对象小于另一个对象，则返回的值小于零；如果当前对象大于另一个对象，则返回的值大于零；如果当前对象等于另一个对象，则返回的值等于零。 这使你可以在代码中定义大于、小于和等于条件。

在 `ListCars` 方法中，`cars.Sort()` 语句对列表进行排序。 对 <xref:System.Collections.Generic.List%601> 的 <xref:System.Collections.Generic.List%601.Sort%2A> 方法的此调用将导致为 `List` 中的 `Car` 对象自动调用 `CompareTo` 方法。

:::code language="vb" source="./snippets/for-each-next-statement/vb/Examples.vb" id="sort":::

## <a name="see-also"></a>另请参阅

- [集合](../../../standard/collections/index.md)
- [For...Next 语句](for-next-statement.md)
- [循环结构](../../programming-guide/language-features/control-flow/loop-structures.md)
- [While...End While 语句](while-end-while-statement.md)
- [Do...Loop 语句](do-loop-statement.md)
- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [对象初始值设定项：命名和匿名类型](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [集合初始值设定项](../../programming-guide/language-features/collection-initializers/index.md)
- [数组](../../programming-guide/language-features/arrays/index.md)
