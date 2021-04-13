---
title: 界面
description: '了解 F # 接口如何指定其他类实现的相关成员集。'
ms.date: 08/15/2020
ms.openlocfilehash: ca6ff206758cab12f1a240c4b79ea5143a2a0b7d
ms.sourcegitcommit: bbc724b72fb6c978905ac715e4033efa291f84dc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107369629"
---
# <a name="interfaces"></a>界面

*接口* 指定其他类实现的一组相关成员。

## <a name="syntax"></a>语法

```fsharp
// Interface declaration:
[ attributes ]
type [accessibility-modifier] interface-name =
    [ interface ]     [ inherit base-interface-name ...]
    abstract member1 : [ argument-types1 -> ] return-type1
    abstract member2 : [ argument-types2 -> ] return-type2
    ...
[ end ]

// Implementing, inside a class type definition:
interface interface-name with
    member self-identifier.member1argument-list = method-body1
    member self-identifier.member2argument-list = method-body2

// Implementing, by using an object expression:
[ attributes ]
let class-name (argument-list) =
    { new interface-name with
        member self-identifier.member1argument-list = method-body1
        member self-identifier.member2argument-list = method-body2
        [ base-interface-definitions ]
    }
    member-list
```

## <a name="remarks"></a>备注

接口声明类似于类声明，但没有任何成员实现。 相反，所有成员都是抽象的，如关键字所示 `abstract` 。 您不提供抽象方法的方法体。 但是，还可以提供默认实现，方法是将成员的单独定义同时包含为方法和 `default` 关键字。 这样做等效于在其他 .NET 语言的基类中创建虚方法。 可在实现接口的类中重写此类虚方法。

接口的默认可访问性为 `public` 。

您可以根据需要使用常规 F # 语法为每个方法参数指定名称：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet24032.fs)]

在上面的 `ISprintable` 示例中， `Print` 方法具有名称为的类型的单个参数 `string` `format` 。

可以通过两种方法实现接口：使用对象表达式和使用类类型。 在任一情况下，类类型或对象表达式都提供接口的抽象方法的方法体。 实现特定于实现接口的每个类型。 因此，不同类型的接口方法可能不同。

`interface` `end` 使用轻量语法时，用于标记定义的开始和结束的关键字和（可选）是可选的。 如果不使用这些关键字，编译器将尝试通过分析你使用的构造来推断该类型是类还是接口。 如果定义成员或使用其他类语法，则会将该类型解释为类。

.NET 编码样式是以大写字母开头的所有接口 `I` 。

可以通过两种方式指定多个参数： F # 样式和。NET 样式。 对于 .NET 使用者，这两种方法都是相同的，但 F # 样式将强制 F # 调用方使用 F # 样式参数应用程序和。NET 样式将强制 F # 调用方使用元组参数应用程序。

```fsharp
type INumericFSharp =
    abstract Add: x: int -> y: int -> int

type INumericDotNet =
    abstract Add: x: int * y: int -> int
```

## <a name="implementing-interfaces-by-using-class-types"></a>使用类类型实现接口

您可以通过使用关键字、接口的名称和关键字以及接口成员定义来实现类类型中的一个或多个接口， `interface` `with` 如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2801.fs)]

接口实现是继承的，因此任何派生类都不需要重新实现它们。

## <a name="calling-interface-methods"></a>调用接口方法

接口方法只能通过接口调用，而不能通过实现接口的类型的任何对象进行调用。 因此，你可能必须通过使用运算符或运算符来向上转换到接口类型， `:>` `upcast` 才能调用这些方法。

若要在具有类型为的对象时调用接口方法 `SomeClass` ，必须将对象向上转换为接口类型，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2802.fs)]

一种替代方法是在对象上声明一个方法，该方法向上转换和调用接口方法，如以下示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2803.fs)]

## <a name="implementing-interfaces-by-using-object-expressions"></a>使用对象表达式实现接口

对象表达式提供了一种简单的方法来实现接口。 当不必创建命名类型并且只需要支持接口方法的对象，而无需任何其他方法时，它们非常有用。 下面的代码演示了对象表达式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2804.fs)]

## <a name="interface-inheritance"></a>接口继承

接口可从一个或多个基接口继承。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2805.fs)]

## <a name="implementing-interfaces-with-default-implementations"></a>实现具有默认实现的接口

C # 支持用默认实现定义接口，如下所示：

```csharp
using System;

namespace CSharp
{
    public interface MyDim
    {
        public int Z => 0;
    }
}
```

可以直接从 F # 中利用这些内容：

```fsharp
open CSharp

// You can implement the interface via a class
type MyType() =
    member _.M() = ()

    interface MyDim

let md = MyType() :> MyDim
printfn $"DIM from C#: %d{md.Z}"

// You can also implement it via an object expression
let md' = { new MyDim }
printfn $"DIM from C# but via Object Expression: %d{md'.Z}"
```

您可以使用重写默认实现 `override` ，如重写任何虚拟成员。

接口中没有默认实现的所有成员仍必须显式实现。

## <a name="implementing-the-same-interface-at-different-generic-instantiations"></a>在不同的泛型实例化上实现相同的接口

F # 支持在不同的泛型实例化实现相同的接口，如下所示：

```fsharp
type IA<'T> =
    abstract member Get : unit -> 'T

type MyClass() =
    interface IA<int> with
        member x.Get() = 1
    interface IA<string> with
        member x.Get() = "hello"

let mc = MyClass()
let iaInt = mc :> IA<int>
let iaString = mc :> IA<string>

iaInt.Get() // 1
iaString.Get() // "hello"
```

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [对象表达式](object-expressions.md)
- [类](classes.md)
