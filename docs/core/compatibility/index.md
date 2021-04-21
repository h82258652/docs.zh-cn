---
title: 重大更改的类型
description: 了解 .NET 如何试着保证开发人员在不同的 .NET 版本中享有兼容性，以及哪种类型的变更被视为中断性变更。
ms.date: 01/28/2021
ms.topic: conceptual
ms.openlocfilehash: a1a55a95cedca4aecba875bb44b952173327606d
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740613"
---
# <a name="changes-that-affect-compatibility"></a>会影响兼容性的变更

在 .NET 的整个历史记录中，它都尝试在版本之间以及 .NET 各个实现之间保持高级别的兼容性。 与 .NET Framework 相比，尽管可以将 .NET 5（和 .NET Core）以及更高版本视为一种新技术，但下面的两个因素使 .NET 的此种实现无法脱离 .NET Framework：

- 有许多最初开发过或在继续开发 .NET Framework 应用程序的开发人员。 他们希望各个 .NET 实现中的行为保持一致。

- .NET Standard 库项目允许开发人员创建面向 .NET 5（和 .NET Core）以及更高版本和 .NET Framework 共享的通用 API 的库。 开发人员希望用于 .NET 5 应用程序的库与用于 .NET Framework 应用程序的同一个库的行为相同。

在希望保持各个 .NET 实现之间的兼容性的同时，开发人员还希望在 .NET 的给定实现的版本之间保持高级别的兼容性。 具体而言，为 .NET Core 早期版本编写的代码应在 .NET 5 或更高版本上无缝运行。 实际上，许多开发人员都希望新发布的 .NET 版本中的新 API 也应该与引入这些 API 的预发布版本兼容。

本文概述了会影响兼容性的变更，以及 .NET 评估每种变更的方式。 如果开发人员需打开要求修改 [现有 .NET API](https://github.com/dotnet/runtime) 的行为的拉取请求，则了解 .NET 团队如何处理可能的中断性变更对他们来说尤其有用。

以下各个部分说明了 .NET API 的变更类别，以及它们对应用程序兼容性的影响。 ✔️ 表示允许更改；❌ 表示不允许更改；❓ 表示需要评判之前行为的可预测性、显著性和一致性。

> [!NOTE]
>
> - 除了将这些准则用作 .NET 库变更评估指南以外，库开发人员还可使用它们评估他们自己的面向多个 .NET 实现和版本的库更改。
> - 要了解兼容性类别（例如向前兼容性和向后兼容性），请参阅[兼容性](categories.md)。

## <a name="modifications-to-the-public-contract"></a>公共协定修改

此类别的变更会修改类型的公共外围应用。 禁止此类别中的多数变更，因为它们违反了向后兼容性（使用早期 API 版本生成的应用程序的功能：无需在较高版本上重新编译即可运行）。

### <a name="types"></a>类型

- ✔️ 允许：基类型已实现接口时，从类型中删除接口实现

- ❓ 需要评判：向类型添加新的接口实现

  此为可接受的变更，因为它不会对现有客户端产生不良影响。 对此类型的任何变更必须在此处定义的可接受变更的边界内工作，新实现才能继续成为可接受的实现。 添加直接影响设计器或序列化程序功能（生成无法供低级使用的代码或数据的功能）的接口时，需要格外注意。 例如 <xref:System.Runtime.Serialization.ISerializable> 接口。

- ❓ 需要评判：引入新的基类

  若类型未引入任何新的[抽象](../../csharp/language-reference/keywords/abstract.md)成员且未更改现有类型的语义或行为，可以将它引入到两个现有类型之间的层次结构。 例如，在 .NET Framework 2.0 中，<xref:System.Data.Common.DbConnection> 类成为之前直接派生自 <xref:System.ComponentModel.Component> 的 <xref:System.Data.SqlClient.SqlConnection> 的新基类。

- ✔️ 允许：将某个程序集中的类型移动到另一个程序集中

  旧程序集必须标有指向新程序集的 <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute>。

- ✔️ 允许：将 [struct](../../csharp/language-reference/builtin-types/struct.md) 类型更改为 `readonly struct` 类型

  不允许将 `readonly struct` 类型更改为 `struct` 类型。

- ✔️ 允许：没有可访问的（public 或 protected）构造函数时，向类型添加 [sealed](../../csharp/language-reference/keywords/sealed.md) 或 [abstract](../../csharp/language-reference/keywords/abstract.md) 关键字

- ✔️ 允许：扩展类型的可见性

- ❌ 不允许：更改类型的命名空间或名称

- ❌ 不允许：重命名或删除公共类型

   这将中断使用重命名的或删除的类型的所有代码。

- ❌ 不允许：更改枚举的基础类型

   此为编译时和行为中断性变更，此外它还是可能会导致属性参数不可分析的二进制中断性更改。

- ❌ 不允许：密封之前未密封的类型

- ❌ 不允许：向接口的一组基类型添加接口

   若接口实现它未曾实现过的接口，将中断实现此接口的原始版本的所有类型。

- ❓ 需要评判：从一组基类删除某个类，或从一组实现的接口删除某个接口

  接口删除规则有一个例外情况：可以添加派生自删除的接口的接口实现。 例如，如果类型或接口现在实现将实现 <xref:System.IDisposable> 的 <xref:System.ComponentModel.IComponent>，则可以删除 <xref:System.IDisposable>。

- ❌ 不允许：将 `readonly struct` 类型更改为 [struct](../../csharp/language-reference/builtin-types/struct.md) 类型

  但允许将 `struct` 类型更改为 `readonly struct` 类型。

- ❌ 不允许：将 [struct](../../csharp/language-reference/builtin-types/struct.md) 类型更改为 `ref struct` 类型，或将后者改为前者

- ❌ 不允许：缩小类型的可见性

   但允许增大类型的可见性。

### <a name="members"></a>成员

- ✔️ 允许：扩展非 [virtual](../../csharp/language-reference/keywords/sealed.md) 成员的可见性

- ✔️ 允许：向不包含任何可访问的（public 或 protected）构造函数或为 [sealed](../../csharp/language-reference/keywords/sealed.md) 类型的公共类型添加抽象成员

  但不允许向包含可访问的（公共或受保护的）构造函数且非 `sealed` 类型的类型添加抽象成员。

- ✔️ 允许：类型不包含任何可访问的（public 或 protected）构造函数或类型为 [sealed](../../csharp/language-reference/keywords/sealed.md) 类型时，限制 [protected](../../csharp/language-reference/keywords/protected.md) 成员的可见性

- ✔️ 允许：将成员移动到层次结构中高于删除的成员所在的类型的类

- ✔️ 允许：添加或删除重写

  引入重写可能会导致先前的使用者在调用 [base](../../csharp/language-reference/keywords/base.md) 时跳过重写。

- ✔️ 允许：向类添加构造函数及无参数构造函数（若该类过去没有任何构造函数）

   但是，不允许在未对过去不包含任何构造函数的类添加无参数构造函数的情况下向其添加构造函数。

- ✔️ 允许：将成员从 [abstract](../../csharp/language-reference/keywords/abstract.md) 更改为 [virtual](../../csharp/language-reference/keywords/virtual.md)

- ✔️ 允许：从 `ref readonly` 更改为 `ref` 返回值（虚拟方法或接口除外）

- ✔️ 允许：若字段的静态类型为非可变的值类型，从字段删除 [readonly](../../csharp/language-reference/keywords/readonly.md)

- ✔️ 允许：调用未曾定义的新事件

- ❓ 需要评判：向类型添加新实例字段

   此变更影响序列化。

- ❌ 不允许：重命名或删除公共成员或参数

   这将中断使用重命名的或删除的成员或参数的所有代码。

   这包括删除或重命名属性中的 Getter 或资源库，以及重命名或删除枚举成员。

- ❌ 不允许：向接口添加成员

- ❌ 不允许：更改公共常量或枚举成员的值

- ❌ 不允许：更改属性类型、字段、参数或返回值

- ❌ 不允许：添加、删除、或更改参数的顺序

- ❌ 不允许：向参数添加或从中删除 [in](../../csharp/language-reference/keywords/in.md)、[out](../../csharp/language-reference/keywords/out.md) 或 [ref](../../csharp/language-reference/keywords/ref.md) 关键字

- ❌ 不允许：重命名参数（包括更改其大小写）

  鉴于以下两个原因将此视为中断性变更：

  - 它将中断后期绑定方案，如 Visual Basic 中的后期绑定功能和 C# 的 [dynamic](../../csharp/language-reference/builtin-types/reference-types.md#the-dynamic-type)。

  - 如果开发人员使用[命名参数](../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md#named-arguments)，它将中断[源兼容性](categories.md#source-compatibility)。

- ❌ 不允许：从 `ref` 返回值更改为 `ref readonly` 返回值

- ❌️ 不允许：在虚拟方法或接口上从 `ref readonly` 更改为 `ref` 返回值

- ❌ 不允许：向成员添加或从中删除 [abstract](../../csharp/language-reference/keywords/abstract.md)

- ❌ 不允许：从成员删除 [virtual](../../csharp/language-reference/keywords/virtual.md) 关键字

  通常这不属于中断性变更，因为 C# 编译器通常会发出 [callvirt](<xref:System.Reflection.Emit.OpCodes.Callvirt>) 中间语言 (IL) 指令来调用非虚拟方法（`callvirt` 执行 null 检查，而常规调用不会执行此检查），鉴于下列原因此行为非恒定：
  - C# 并非 .NET 面向的唯一语言。

  - 目标方法为非虚拟且可能非 null 时（如通过 [?. null 传播运算符](../../csharp/language-reference/operators/member-access-operators.md#null-conditional-operators--and-)访问的方法），C# 编译器逐渐尝将 `callvirt` 优化为常规调用。

  使方法成为虚拟方法意味着使用者代码通常最终要以非虚拟方式调用它。

- ❌ 不允许：向成员添加 [virtual](../../csharp/language-reference/keywords/virtual.md) 关键字

- ❌ 不允许：使 virtual 成员成为 abstract 成员

  [抽象成员](../../csharp/language-reference/keywords/virtual.md)提供可以由派生类重写的方法实现。 [抽象成员](../../csharp/language-reference/keywords/abstract.md)不提供任何实现，且必须重写。

- ❌ 不允许：向包含可访问的（public 或 protected）构造函数且非 [sealed](../../csharp/language-reference/keywords/sealed.md) 类型的公共类型添加抽象成员

- ❌ 不允许：向成员添加或从中删除 [static](../../csharp/language-reference/keywords/static.md) 关键字

- ❌ 不允许：添加排除现有重载并定义其他行为的重载

  这将中断已绑定先前重载的现有客户端。 例如，若类包含单个接受 <xref:System.UInt32> 的方法的版本，传递 <xref:System.Int32> 值时，现有使用者将成功地绑定该重载。 但是，如果添加接受 <xref:System.Int32> 的重载，重新编译或使用晚期绑定时，编译器现在将绑定新的重载。 若生成不同的行为，则它属于中断性变更。

- ❌ 不允许：只向过去不包含任何构造函数的类添加构造函数而不添加无参数构造函数

- ❌️ 不允许：向字段添加 [readonly](../../csharp/language-reference/keywords/readonly.md)

- ❌ 不允许：降低成员的可见性

   这包括在存在可访问的（`public` 或 `protected`）构造函数且类型非 [sealed](../../csharp/language-reference/keywords/sealed.md) 的情况下降低 [protected](../../csharp/language-reference/keywords/protected.md) 成员的可见性。 若不属于上述情况，则允许降低受保护的成员的可见性。

   允许增大成员的可见性。

- ❌ 不允许：更改成员的类型

   不可修改方法的返回值、属性类型或字段。 例如，不可将返回 <xref:System.Object> 的方法的签名更改为返回 <xref:System.String>，反之亦然。

- ❌ 不允许：向先前没有任何状态的结构添加字段

  有明确的分配规则规定，只要变量类型为无状态结构，即允许使用未初始化的变量。 若结构成为有状态结构，代码可能最终成为未初始化的数据。 这可能既是源中断性变更，又是二进制中断性变更。

- ❌ 不允许：触发先前从未触发过的现有事件

## <a name="behavioral-changes"></a>行为变更

### <a name="assemblies"></a>程序集

- ✔️ 允许：使程序集成为可移植的程序集并且仍支持同样的平台

- ❌ 不允许：更改程序集的名称
- ❌ 不允许：更改程序集的公钥

### <a name="properties-fields-parameters-and-return-values"></a>属性、字段、参数和返回值

- ✔️ 允许：将属性、字段、返回值或 [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) 参数的值更改为派生程度更大的类型

  例如，返回 <xref:System.Object> 的类型的方法可能返回 <xref:System.String> 实例。 （但是不可更改方法签名。）

- ✔️ 允许：在成员为非 [virtual](../../csharp/language-reference/keywords/virtual.md) 成员时，扩大属性或参数的可接受值的范围

  可以扩展可传递到方法或由成员返回的值范围，但不可扩展参数或成员类型。 例如，传递到方法的值可以从 0-124 扩展到 0-255，但参数类型不可从 <xref:System.Byte> 更改为 <xref:System.Int32>。

- ❌ 不允许：在成员为 [virtual](../../csharp/language-reference/keywords/virtual.md) 成员时，扩大属性或参数的可接受值的范围

   此变更将中断已重写的现有成员，面向扩展的值范围时它们将无法正常运行。

- ❌ 不允许：缩小属性或参数的可接受值的范围

- ❌ 不允许：扩大属性的返回值范围、字段、返回值或 [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) 参数

- ❌ 不允许：更改属性的返回值、字段、方法返回值或 [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) 参数

- ❌ 不允许：更改属性、字段或参数的默认值

- ❌ 不允许：更改数值返回值的精度

- ❓ 需要评判：关于输入分析和新异常引发的变更（尽管本文档未指定分析行为）

### <a name="exceptions"></a>异常

- ✔️ 允许：引发派生程度高于现有异常的异常

  由于新异常是现有异常的子类，先前的异常处理代码将继续处理异常。 例如，在 .NET Framework 4 中，找不到区域性时，区域性生成和检索方法开始引发 <xref:System.Globalization.CultureNotFoundException> 而不引发 <xref:System.ArgumentException>。 由于 <xref:System.Globalization.CultureNotFoundException> 派生自 <xref:System.ArgumentException>，因此这是可接受的变更。

- ✔️ 允许：引发比 <xref:System.NotSupportedException>、<xref:System.NotImplementedException>、<xref:System.NullReferenceException> 更加具体的异常

- ✔️ 允许：引发被视为无法恢复的异常

  不应捕获无法恢复的异常，而应该由高级别的全部捕获处理程序处理它们。 因此，用户不应该拥有捕获这些显式异常的代码。 无法恢复的异常包括：

  - <xref:System.AccessViolationException>
  - <xref:System.ExecutionEngineException>
  - <xref:System.Runtime.InteropServices.SEHException>
  - <xref:System.StackOverflowException>

- ✔️ 允许：在新的代码路径中引发新的异常

  异常必须仅适用于使用新参数值或状态执行并且无法由面向先前版本的现有代码执行的新代码路径。

- ✔️ 允许：删除异常，以启用更可靠的行为或新方案

  例如，可以以其他方式将先前仅处理正值并引发 <xref:System.ArgumentOutOfRangeException> 的 `Divide` 方法更改为同时支持正值和负值并且不引发异常。

- ✔️ 允许：更改错误消息的文本

  开发人员不应依赖也会基于用户区域性更改的错误消息文本。

- ❌ 不允许：在上文未列出的任何其他的情况下引发异常

- ❌ 不允许：在上文未列出的任何其他的情况下删除异常

### <a name="attributes"></a>特性

- ✔️ 允许：更改不可观测的属性的值

- ❌ 不允许：更改可观测的属性的值

- ❓ 需要评判：删除属性

  多数情况下，删除属性（如 <xref:System.NonSerializedAttribute>）为中断性变更。

## <a name="platform-support"></a>平台支持

- ✔️ 允许：在平台上支持先前不支持的操作

- ❌ 不允许：对于平台先前支持的操作，不再支持或者现在需要特定服务包

## <a name="internal-implementation-changes"></a>内部实现变更

- ❓ 需要评判：更改内部类型的外围应用

   尽管此类更改将中断私有反射，但通常允许这些变更。 如果常用的第三方库或大量开发人员依赖内部 API，在这些情况下，可能不允许此类变更。

- ❓ 需要评判：更改成员的内部实现

  尽管此类更改将中断私有反射，但通常允许这些变更。 如果客户代码频繁依赖私有反射，或者变更引入意外的负面影响，在这些情况下，可能不允许这些变更。

- ✔️ 允许：提高操作的性能

   修改操作性能的功能必不可少，但此类变更可能会中断依赖操作的当前速度的代码。 这一点尤其适用于对于依赖异步操作计时的代码。 性能更改应不影响所说的 API 的其他行为；否则，变更将属于中断性变更。

- ✔️ 允许：间接更改操作性能（通常产生的是负面影响）

  若出于某些其他的原因未将所说的变更归类为中断性变更，这是可以接受的。 通常需要执行可能包含额外操作或添加新功能的操作。 这几乎都会影响性能，但对于使所说的 API 按预期方式运行而言它可能必不可少。

- ❌ 不允许：将同步 API 更改为异步（反之亦然）

## <a name="code-changes"></a>代码更改

- ✔️ 允许：向参数添加 [params](../../csharp/language-reference/keywords/params.md)

- ❌ 不允许：将 [struct](../../csharp/language-reference/builtin-types/struct.md) 更改为 [class](../../csharp/language-reference/keywords/class.md)，或将后者改为前者

- ❌ 不允许：向代码块添加 [checked](../../csharp/language-reference/keywords/virtual.md) 关键字

   此变更可能导致先前执行的代码引发 <xref:System.OverflowException>，此为不可接受的变更。

- ❌ 不允许：从参数删除 [params](../../csharp/language-reference/keywords/params.md)

- ❌ 不允许：更改事件的触发顺序

  开发人员可以合理地期望事件按相同的顺序触发，开发人员代码频繁依赖事件的触发顺序。

- ❌ 不允许：删除给定操作上的事件引发

- ❌ 不允许：更改给定事件的调用次数

- ❌ 不允许：向枚举类型添加 <xref:System.FlagsAttribute>
