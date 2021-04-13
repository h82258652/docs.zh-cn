---
title: C# 保留属性：其他
ms.date: 03/18/2021
description: 了解影响编译器生成的代码的属性：Conditional、Obsolete、AttributeUsage、ModuleInitializer 和 SkipLocalsInit 属性。
ms.openlocfilehash: 6b8cda658ec5b3f81a7f903d8cadae0fe30e8ac2
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111306"
---
# <a name="reserved-attributes-miscellaneous"></a>保留属性：其他

这些特性可应用于代码中的元素。 它们为这些元素添加语义。 编译器使用这些语义来更改其输出，并报告使用你的代码的开发人员可能犯的错误。

## <a name="conditional-attribute"></a>`Conditional` 特性

`Conditional` 特性使得方法执行依赖于预处理标识符。 `Conditional` 属性是 <xref:System.Diagnostics.ConditionalAttribute> 的别名，可以应用于方法或特性类。

在以下示例中，`Conditional` 应用于启用或禁用显示特定于程序的诊断信息的方法：

:::code language="csharp" source="snippets/trace.cs" interactive="try-dotnet" :::

如果未定义 `TRACE_ON` 标识符，则不会显示跟踪输出。 在交互式窗口中自己探索。

`Conditional` 特性通常与 `DEBUG` 标识符一起使用，以启用调试生成（而非发布生成）中的跟踪和日志记录功能，如下例所示：

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetConditional" :::

当调用标记为条件的方法时，指定的预处理符号是否存在将决定是包含还是省略该调用。 如果定义了符号，则将包括调用；否则，将忽略该调用。 条件方法必须是类或结构声明中的方法，而且必须具有 `void` 返回类型。 与将方法封闭在 `#if…#endif` 块内相比，`Conditional` 更简洁且较不容易出错。

如果某个方法具有多个 `Conditional` 特性，则如果定义了一个或多个条件符号（通过使用 OR 运算符将这些符号逻辑链接在一起），则包含对该方法的调用。 在以下示例中，存在 `A` 或 `B` 将导致方法调用：

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetMultipleConditions" :::

### <a name="using-conditional-with-attribute-classes"></a>使用带有特性类的 `Conditional`

`Conditional` 特性还可应用于特性类定义。 在以下示例中，如果定义了 `DEBUG`，则自定义特性 `Documentation` 将仅向元数据添加信息。

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetConditionalConditionalAttribute" :::

## <a name="obsolete-attribute"></a>`Obsolete` 特性

`Obsolete` 特性将代码元素标记为不再推荐使用。 使用标记为已过时的实体会生成警告或错误。 `Obsolete` 特性是一次性特性，可以应用于任何允许特性的实体。 `Obsolete` 是 <xref:System.ObsoleteAttribute> 的别名。

在以下示例中，`Obsolete` 特性应用于类 `A` 和方法 `B.OldMethod`。 因为应用于 `B.OldMethod` 的特性构造函数的第二个参数设置为 `true`，所以此方法将导致编译器错误，而使用类 `A` 只会生成警告。 但是，调用 `B.NewMethod` 不会生成任何警告或错误。 例如，将其与先前的定义一起使用时，以下代码会生成两个警告和一个错误：

:::code language="csharp" source="snippets/ObsoleteExample.cs" interactive="try-dotnet" :::

作为特性构造函数的第一个参数提供的字符串将作为警告或错误的一部分显示。 将生成类 `A` 的两个警告：一个用于声明类引用，另一个用于类构造函数。 `Obsolete` 特性可以在不带参数的情况下使用，但建议说明改为使用哪个项目。

## <a name="attributeusage-attribute"></a>`AttributeUsage` 特性

`AttributeUsage` 特性确定自定义特性类的使用方式。 <xref:System.AttributeUsageAttribute> 是应用到自定义特性定义的特性。 `AttributeUsage` 特性帮助控制：

- 可能应用到的具体程序元素特性。 除非使用限制，否则特性可能应用到以下任意程序元素：
  - 程序集 (assembly)
  - name
  - field
  - event
  - method
  - param
  - property
  - return
  - type
- 某特性是否可多次应用于单个程序元素。
- 特性是否由派生类继承。

显式应用时，默认设置如以下示例所示：

:::code language="csharp" source="snippets/NewAttribute.cs" id="SnippetUsageFirst" :::

在此示例中，`NewAttribute` 类可应用于任何受支持的程序元素。 但是它对每个实体仅能应用一次。 特性应用于基类时，它由派生类继承。

<xref:System.AttributeUsageAttribute.AllowMultiple> 和 <xref:System.AttributeUsageAttribute.Inherited> 参数是可选的，因此以下代码具有相同效果：

:::code language="csharp" source="snippets/NewAttribute.cs" id="SnippetUsageSecond" :::

第一个 <xref:System.AttributeUsageAttribute> 参数必须是 <xref:System.AttributeTargets> 枚举的一个或多个元素。 可将多个目标类型与 OR 运算符链接在一起，如下例所示：

:::code language="csharp" source="snippets/NewPropertyOrFieldAttribute.cs" id="SnippetDefinePropertyAttribute" :::

从 C# 7.3 开始，特性可应用于自动实现的属性的属性或支持字段。 特性应用于属性，除非在特性上指定 `field` 说明符。 都在以下示例中进行了演示：

:::code language="csharp" source="snippets/NewPropertyOrFieldAttribute.cs" id="SnippetUsePropertyAttribute" :::

如果 <xref:System.AttributeUsageAttribute.AllowMultiple> 参数为 `true`，那么结果特性可多次应用于单个实体，如以下示例所示：

:::code language="csharp" source="snippets/MultiUseAttribute.cs" id="SnippetMultiUse" :::

在本例中，`MultiUseAttribute` 可重复应用，因为 `AllowMultiple` 设置为 `true`。 所显示的两种用于应用多个特性的格式均有效。

如果 <xref:System.AttributeUsageAttribute.Inherited> 为 `false`，那么该特性不是由特性类派生的类继承。 例如：

:::code language="csharp" source="snippets/NonInheritedAttribute.cs" id="SnippetNonInherited" :::

在本例中，`NonInheritedAttribute` 不会通过继承应用于 `DClass`。

## <a name="moduleinitializer-attribute"></a>`ModuleInitializer` 特性

从 C# 9 开始，`ModuleInitializer` 属性标记程序集加载时运行时调用的方法。 `ModuleInitializer` 是 <xref:System.Runtime.CompilerServices.ModuleInitializerAttribute> 的别名。

`ModuleInitializer` 属性只能应用于以下方法：

* 静态方法。
* 无参数方法。
* 返回 `void`。
* 能够从包含模块（即 `internal` 或 `public`）访问的方法。
* 不是泛型的方法。
* 没有包含在泛型类中的方法。
* 不是本地函数的方法。

`ModuleInitializer` 属性可应用于多种方法。 在这种情况下，运行时调用它们的顺序是确定的，但未指定。

下面的示例阐释了如何使用多个模块初始化表达式方法。 `Init1` 和 `Init2` 方法在 `Main` 之前运行，并且每种方法都将一个字符串添加到 `Text` 属性。 因此，当 `Main` 运行时，`Text` 属性已具有来自两个初始化表达式方法中的字符串。

:::code language="csharp" source="snippets/ModuleInitializerExampleMain.cs" :::

:::code language="csharp" source="snippets/ModuleInitializerExampleModule.cs" :::

源代码生成器有时需要生成初始化代码。 模块初始化表达式为该代码提供了一个标准的驻留位置。

## <a name="skiplocalsinit-attribute"></a>`SkipLocalsInit` 特性

从 C# 9 开始，`SkipLocalsInit` 属性可防止编译器在发出到元数据时设置 `.locals init` 标志。 `SkipLocalsInit` 属性是一个单用途属性，可应用于方法、属性、类、结构、接口或模块，但不能应用于程序集。 `SkipLocalsInit` 是 <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute> 的别名。

`.locals init` 标志会导致 CLR 将方法中声明的所有局部变量初始化为其默认值。 由于编译器还可以确保在为变量赋值之前永远不使用变量，因此通常不需要使用 `.locals init`。 但是，在某些情况下，额外的零初始化可能会对性能产生显著影响，例如使用 [stackalloc](../operators/stackalloc.md) 在堆栈上分配一个数组时。 在这些情况下，可添加 `SkipLocalsInit` 属性。 如果直接应用于方法，该属性会影响该方法及其所有嵌套函数，包括 lambda 和局部函数。 如果应用于类型或模块，则它会影响嵌套在内的所有方法。 此属性不会影响抽象方法，但会影响为实现生成的代码。

此属性需要 [AllowUnsafeBlocks](../compiler-options/language.md#allowunsafeblocks) 编译器选项。 这是为了发出信号，在某些情况下，代码可以查看未分配的内存（例如，读取未初始化的堆栈分配的内存）。

下面的示例阐释 `SkipLocalsInit` 属性对使用 `stackalloc` 的方法的影响。 该方法显示分配整数数组后内存中的任何内容。

:::code language="csharp" source="snippets/SkipLocalsInitExample.cs" id="ReadUninitializedMemory":::

若要亲自尝试此代码，请在 .csproj 文件中设置 `AllowUnsafeBlocks` 编译器选项：

```xml
<PropertyGroup>
  ...
  <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
</PropertyGroup>
```

## <a name="see-also"></a>请参阅

- <xref:System.Attribute>
- <xref:System.Reflection>
- [特性](../../../standard/attributes/index.md)
- [反射](../../programming-guide/concepts/reflection.md)
