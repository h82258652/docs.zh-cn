---
description: 了解详细信息： Declare 语句
title: Declare Statement
ms.date: 07/20/2015
f1_keywords:
- vb.Declare
- vb.Lib
- vb.Any
helpviewer_keywords:
- Lib keyword [Visual Basic]
- declaring procedures [Visual Basic], Declare statement
- functions [Visual Basic], function procedures
- declarations [Visual Basic], procedures
- procedures [Visual Basic], declaration
- procedures [Visual Basic], external
- Alias keyword [Visual Basic]
- external references [Visual Basic], Visual Basic
- DLLs, declaring procedures
- Declare statement [Visual Basic]
- declarations [Visual Basic], external
- Visual Basic code, Function procedures
- As keyword [Visual Basic], in Declare statement
- resources [Visual Basic], declaring
- Public keyword [Visual Basic], Declare statement
- platform invoke, Visual Basic external references
- Sub procedures [Visual Basic], declarations
- APIs, declaring API functions
- Visual Basic code, Sub procedures
- Function procedures [Visual Basic], declaring
ms.assetid: d3f21fb0-b804-4c99-97ed-583b23894cf1
ms.openlocfilehash: 4ceca6b1095e34a437f9363d95e4133e77809e83
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494553"
---
# <a name="declare-statement"></a>Declare Statement

声明对外部文件中所实现过程的引用。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Overloads ] _
Declare [ charsetmodifier ] [ Sub ] name Lib "libname" _
[ Alias "aliasname" ] [ ([ parameterlist ]) ]
' -or-
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Overloads ] _
Declare [ charsetmodifier ] [ Function ] name Lib "libname" _
[ Alias "aliasname" ] [ ([ parameterlist ]) ] [ As returntype ]
```

## <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`attributelist`|可选。 请参阅 [特性列表](attribute-list.md)。|
|`accessmodifier`|可选。 可以是以下值之一：<br /><br /> -   [公布](../modifiers/public.md)<br />-   [避免](../modifiers/protected.md)<br />-   [友好](../modifiers/friend.md)<br />-   [专有](../modifiers/private.md)<br />- [受保护的朋友](../modifiers/protected-friend.md)<br />- [私有受保护](../modifiers/private-protected.md)<br /><br /> 请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。|
|`Shadows`|可选。 请参阅 [阴影](../modifiers/shadows.md)。|
|`charsetmodifier`|可选。 指定字符集和文件搜索信息。 可以是以下值之一：<br /><br /> -   [Ansi](../modifiers/ansi.md) (默认值) <br />-   [Unicode](../modifiers/unicode.md)<br />-   [自动](../modifiers/auto.md)|
|`Sub`|可选，但 `Sub` `Function` 必须出现或。 指示外部过程不返回值。|
|`Function`|可选，但 `Sub` `Function` 必须出现或。 指示外部过程返回值。|
|`name`|必需。 此外部引用的名称。 有关详细信息，请参阅已 [声明的元素名称](../../programming-guide/language-features/declared-elements/declared-element-names.md)。|
|`Lib`|必需。 引入一个 `Lib` 子句，该子句标识包含外部过程的外部文件 (DLL 或代码资源) 。|
|`libname`|必需。 包含已声明过程的文件的名称。|
|`Alias`|可选。 指示无法在其文件中按中指定的名称标识所声明的过程 `name` 。 在中指定其标识 `aliasname` 。|
|`aliasname`|如果使用关键字，则为必需 `Alias` 。 通过以下两种方式之一标识过程的字符串：<br /><br /> 过程在其文件中的入口点名称，在引号 (`""`) <br /><br /> -或-<br /><br /> 数字符号 (`#`) 后跟一个整数，该整数指定过程入口点在其文件中的序号|
|`parameterlist`|如果过程使用参数，则为必需。 请参阅 [参数列表](parameter-list.md)。|
|`returntype`|如果 `Function` 指定了并为，则 `Option Strict` 为必需 `On` 。 过程返回的值的数据类型。|

## <a name="remarks"></a>注解

有时，您需要调用在项目外部)  (文件中定义的过程，如 DLL 或代码资源。 当你执行此操作时，Visual Basic 编译器不能访问正确调用过程所需的信息，例如过程所在的位置、标识方法、调用序列和返回类型，以及它使用的字符串字符集。 `Declare`语句创建对外部过程的引用并提供此必要信息。

只能在模块级别使用 `Declare`。 这意味着外部引用的 *声明上下文* 必须是类、结构或模块，不能是源文件、命名空间、接口、过程或块。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

外部引用默认为 [公共](../modifiers/public.md) 访问。 您可以使用访问修饰符调整其访问级别。

## <a name="rules"></a>规则

- **属性.** 您可以将属性应用于外部引用。 所应用的任何属性仅在您的项目中有效，而不是在外部文件中。

- **组成.** 外部过程是隐式 [共享](../modifiers/shared.md)的。 `Shared`声明外部引用时不能使用关键字，也不能更改其共享状态。

  外部过程不能参与重写、实现接口成员或处理事件。 因此，不能 `Overrides` `Overridable` 在语句中使用、、、 `NotOverridable` `MustOverride` 、 `Implements` 或 `Handles` 关键字 `Declare` 。

- **外部过程名称。** 您不必为此外部引用指定 (在 `name`) 中与过程的入口点名称相同的名称， (`aliasname`) 。 您可以使用 `Alias` 子句来指定入口点名称。 如果外部过程与同一范围内的 Visual Basic 保留修饰符、变量、过程或任何其他编程元素同名，这会很有用。

  > [!NOTE]
  > 大多数 Dll 中的入口点名称都区分大小写。

- **外部过程号。** 或者，您可以使用 `Alias` 子句来指定外部文件的导出表中入口点的序号。 为此，你必须以 `aliasname` 数字符号 () 开头 `#` 。 如果 Visual Basic 中不允许使用外部过程名称中的任何字符，或者如果外部文件导出没有名称的过程，则这会很有用。

## <a name="data-type-rules"></a>数据类型规则

- **参数数据类型。** 如果 `Option Strict` 为 `On` ，则必须在中指定每个参数的数据类型 `parameterlist` 。 这可以是任何数据类型，也可以是枚举、结构、类或接口的名称。 在中 `parameterlist` ，可以使用 `As` 子句来指定要传递给每个参数的参数的数据类型。

  > [!NOTE]
  > 如果没有为 .NET Framework 编写外部过程，则必须注意数据类型是对应的。 例如，如果您声明一个对 Visual Basic 6.0 过程的外部引用，而 `Integer` 参数的参数 (16 位 Visual Basic 6.0) ，则您必须在语句中标识相应的参数 `Short` `Declare` ，因为这是 Visual Basic 中的16位整数类型。 同样， `Long` 在 Visual Basic 6.0 中具有不同的数据宽度，并以 `Date` 不同的方式实现。

- **返回数据类型。** 如果外部过程是 `Function` 且 `Option Strict` 为 `On` ，则必须指定返回到调用代码的值的数据类型。 这可以是任何数据类型，也可以是枚举、结构、类或接口的名称。

  > [!NOTE]
  > Visual Basic 编译器不会验证数据类型是否与外部过程的数据类型兼容。 如果存在不匹配的情况，则公共语言运行时将 <xref:System.Runtime.InteropServices.MarshalDirectiveException> 在运行时生成异常。

- **默认数据类型。** 如果 `Option Strict` 为 `Off` ，并且未在中指定参数的数据类型，则 `parameterlist` Visual Basic 编译器会将相应的参数转换为 [对象数据类型](../data-types/object-data-type.md)。 同样，如果不指定 `returntype` ，编译器会将返回的数据类型采用 `Object` 。

  > [!NOTE]
  > 因为你要处理的是可能已在其他平台上编写的外部过程，所以对数据类型进行任何假设或允许其默认值是危险的。 最安全的方法是指定每个参数的数据类型和返回值（如果有）。 这也会提高代码的可读性。

## <a name="behavior"></a>行为

- **划分范围。** 外部引用在其类、结构或模块的范围内。

- **生存期.** 外部引用与声明它的类、结构或模块具有相同的生存期。

- **调用外部过程。** 调用外部过程的方法与调用或过程的方法 `Function` 相同 `Sub` ，即在表达式返回值的情况下，或者在 [call 语句](call-statement.md) 中指定它（如果不返回值）。

  直接将参数传递给外部过程，如语句中所指定的那样 `parameterlist` `Declare` 。 不要考虑参数最初在外部文件中的声明方式。 同样，如果有返回值，请按语句中的指定完全相同的值 `returntype` `Declare` 。

- **字符集。** 可以指定 Visual Basic 在 `charsetmodifier` 调用外部过程时应如何封送字符串。 `Ansi`修饰符指示 Visual Basic 将所有字符串封送为 ANSI 值， `Unicode` 修饰符指示它将所有字符串封送为 Unicode 值。 `Auto`修饰符指示 Visual Basic 根据外部引用 .NET Framework 规则封送字符串 `name` ，如果指定，则指示 `aliasname` 。 默认值为 `Ansi`。

  `charsetmodifier` 还指定 Visual Basic 应如何查找外部文件中的外部过程。 `Ansi``Unicode`同时直接 Visual Basic 在搜索过程中查找，而无需修改其名称。 `Auto` 指示 Visual Basic 确定运行时平台的基本字符集，并可能修改外部过程名称，如下所示：

  - 在 Unicode 平台（如 Windows）上，首先查找外部过程，而不修改名称。 如果此操作失败，请将 "W" 追加到外部过程名称的末尾，并再次查找。

  - 在 ANSI 平台上，首先查找外部过程，而不修改名称。 如果此操作失败，请在外部过程名称末尾追加 "A"，并再次查找。

- **机制.** Visual Basic 使用 .NET Framework *平台调用* (PInvoke) 机制来解析和访问外部过程。 `Declare`语句和 <xref:System.Runtime.InteropServices.DllImportAttribute> 类都自动使用这种机制，无需任何有关 PInvoke 的知识。 有关详细信息，请参阅 [演练：调用 Windows api](../../programming-guide/com-interop/walkthrough-calling-windows-apis.md)。

> [!IMPORTANT]
> 如果外部过程在公共语言运行时 (CLR) 之外运行，则它是不 *受管理的代码*。 如果调用此类过程（例如 Windows API 函数或 COM 方法），则可能会向应用程序带来安全风险。 有关详细信息，请参阅 [非托管代码的安全编码指导原则](/previous-versions/dotnet/framework/security/secure-coding-guidelines-for-unmanaged-code)。

## <a name="example"></a>示例

下面的示例声明一个对 `Function` 返回当前用户名的过程的外部引用。 然后，它将调用外部过程 `GetUserNameA` 作为过程的一部分 `getUser` 。

[!code-vb[VbVbalrStatements#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#15)]

## <a name="example"></a>示例

提供了一 <xref:System.Runtime.InteropServices.DllImportAttribute> 种在非托管代码中使用函数的替代方法。 下面的示例声明一个导入的函数，而不使用 `Declare` 语句。

[!code-vb[VbVbalrStatements#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#16)]

[!code-vb[VbVbalrStatements#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#1)]

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.ErrObject.LastDllError%2A>
- [Imports 语句（.NET 命名空间和类型）](imports-statement-net-namespace-and-type.md)
- [AddressOf 运算符](../operators/addressof-operator.md)
- [Function 语句](function-statement.md)
- [Sub 语句](sub-statement.md)
- [参数列表](parameter-list.md)
- [Call 语句](call-statement.md)
- [演练：调用 Windows API](../../programming-guide/com-interop/walkthrough-calling-windows-apis.md)
