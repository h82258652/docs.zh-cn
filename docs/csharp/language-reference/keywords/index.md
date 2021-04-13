---
description: C# 关键字
title: C# 关键字
ms.date: 03/17/2021
f1_keywords:
- cs.keywords
helpviewer_keywords:
- keywords [C#]
- C# language, keywords
- Visual C#, keywords
- '@ keyword'
ms.custom: updateeachrelease
ms.openlocfilehash: 7fe4c82f3736232ffc5f0d32694d7c8dbb90f7a8
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075493"
---
# <a name="c-keywords"></a>C# 关键字

关键字是预定义的保留标识符，对编译器有特殊意义。 除非前面有 `@` 前缀，否则不能在程序中用作标识符。 例如，`@if` 是有效标识符，而 `if` 则不是，因为 `if` 是关键字。  
  
 此主题中的第一个表列出了是 C# 程序任意部分中的保留标识符的关键字。 此主题中的第二个表列出了 C# 中的上下文关键字。 上下文关键字仅在一部分程序上下文中有特殊含义，可以在相应上下文范围之外用作标识符。 一般来说，C# 语言中新增的关键字会作为上下文关键字添加，以免破坏用旧版语言编写的程序。  
  
|||||  
|---|---|---|---|  
|[abstract](abstract.md)|[as](../operators/type-testing-and-cast.md#as-operator)|[base](base.md)|[bool](../builtin-types/bool.md)|
|[break](break.md)|[byte](../builtin-types/integral-numeric-types.md)|[case](switch.md)|[catch](try-catch.md)|
|[char](../builtin-types/char.md)|[checked](checked.md)|[class](class.md)|[const](const.md)|
|[continue](continue.md)|[decimal](../builtin-types/floating-point-numeric-types.md)|[default](default.md)|[delegate](../builtin-types/reference-types.md)|
|[do](do.md)|[double](../builtin-types/floating-point-numeric-types.md)|[else](if-else.md)|[enum](../builtin-types/enum.md)|
|[event](event.md)|[explicit](../operators/user-defined-conversion-operators.md)|[extern](extern.md)|[false](../builtin-types/bool.md)|
|[finally](try-finally.md)|[fixed](fixed-statement.md)|[float](../builtin-types/floating-point-numeric-types.md)|[for](for.md)|
|[foreach](foreach-in.md)|[goto](goto.md)|[if](if-else.md)|[implicit](../operators/user-defined-conversion-operators.md)|
|[in](in.md)|[int](../builtin-types/integral-numeric-types.md)|[interface](interface.md)|[internal](internal.md)|
|[is](is.md)|[lock](lock-statement.md)|[long](../builtin-types/integral-numeric-types.md)|[namespace](namespace.md)|
|[new](../operators/new-operator.md)|[null](null.md)|[object](../builtin-types/reference-types.md)|[operator](../operators/operator-overloading.md)|
|[out](out.md)|[override](override.md)|[params](params.md)|[private](private.md)|
|[protected](protected.md)|[public](public.md)|[readonly](readonly.md)|[ref](ref.md)|
|[return](return.md)|[sbyte](../builtin-types/integral-numeric-types.md)|[sealed](sealed.md)|[short](../builtin-types/integral-numeric-types.md)|
|[sizeof](../operators/sizeof.md)|[stackalloc](../operators/stackalloc.md)|[static](static.md)|[string](../builtin-types/reference-types.md)|
|[struct](../builtin-types/struct.md)|[switch](switch.md)|[this](this.md)|[throw](throw.md)|
|[true](../builtin-types/bool.md)|[try](try-catch.md)|[typeof](../operators/type-testing-and-cast.md#typeof-operator)|[uint](../builtin-types/integral-numeric-types.md)|
|[ulong](../builtin-types/integral-numeric-types.md)|[unchecked](unchecked.md)|[unsafe](unsafe.md)|[ushort](../builtin-types/integral-numeric-types.md)|
|[using](using.md)|[virtual](virtual.md)|[void](../builtin-types/void.md)|[volatile](volatile.md)|
|[while](while.md)||||

## <a name="contextual-keywords"></a>上下文关键字

 上下文关键字用于在代码中提供特定含义，但它不是 C# 中的保留字。 一些上下文关键字（如 `partial` 和 `where`）在两个或多个上下文中有特殊含义。  
  
||||  
|---|---|---|  
|[add](add.md)|[and](../operators/patterns.md#logical-patterns)|[alias](extern-alias.md)|
|[ascending](ascending.md)|[async](async.md)|[await](../operators/await.md)|
|[by](by.md)|[descending](descending.md)|[dynamic](../builtin-types/reference-types.md)|
|[equals](equals.md)|[from](from-clause.md)|[get](get.md)|
|[global](../operators/namespace-alias-qualifier.md)|[group](group-clause.md)|[init](init.md)|
|[into](into.md)|[join](join-clause.md)|[let](let-clause.md)|
|[nameof](../operators/nameof.md)|[nint](../builtin-types/nint-nuint.md)|[not](../operators/patterns.md#logical-patterns)|
|[notnull](../../programming-guide/generics/constraints-on-type-parameters.md#notnull-constraint)|[nuint](../builtin-types/nint-nuint.md)|[on](on.md)|
|[or](../operators/patterns.md#logical-patterns)|[orderby](orderby-clause.md)|[partial（类型）](partial-type.md)|
|[partial（方法）](partial-method.md)|[record](../../programming-guide/classes-and-structs/records.md)|[remove](remove.md)|
|[select](select-clause.md)|[set](set.md)|[unmanaged（泛型类型约束）](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint)|
|[value](value.md)|[var](var.md)|[when（筛选条件）](when.md)|
|[where（泛型类型约束）](where-generic-type-constraint.md)|[where（查询子句）](where-clause.md)|[with](../operators/with-expression.md)|
|[yield](yield.md)|||

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
