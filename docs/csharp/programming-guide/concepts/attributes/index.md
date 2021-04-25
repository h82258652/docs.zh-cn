---
title: 特性 (C#)
description: 了解如何使用特性将元数据或声明性信息与 C# 中的代码关联起来。 可以在运行时使用反射来查询特性。
ms.date: 04/26/2018
ms.openlocfilehash: 4c85e36a36cbaa16ab8d1344237e5d685ffd60d9
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740366"
---
# <a name="attributes-c"></a>特性 (C#)

使用特性，可以有效地将元数据或声明性信息与代码（程序集、类型、方法、属性等）相关联。 将特性与程序实体相关联后，可以在运行时使用 *反射* 这项技术查询特性。 有关详细信息，请参阅[反射 (C#)](../reflection.md)。

特性具有以下属性：

- 特性向程序添加元数据。 *元数据* 是程序中定义的类型的相关信息。 所有 .NET 程序集都包含一组指定的元数据，用于描述程序集中定义的类型和类型成员。 可以添加自定义特性来指定所需的其他任何信息。 有关详细信息，请参阅[创建自定义特性 (C#)](creating-custom-attributes.md)。
- 可以将一个或多个特性应用于整个程序集、模块或较小的程序元素（如类和属性）。
- 特性可以像方法和属性一样接受自变量。
- 程序可使用反射来检查自己的元数据或其他程序中的元数据。 有关详细信息，请参阅[使用反射访问特性 (C#)](accessing-attributes-by-using-reflection.md)。

## <a name="using-attributes"></a>使用特性

可以将特性附加到几乎任何声明中，尽管特定特性可能会限制可有效附加到的声明的类型。 在 C# 中，通过用方括号 ([]) 将特性名称括起来，并置于应用该特性的实体的声明上方以指定特性。

在此示例中，<xref:System.SerializableAttribute> 特性用于将具体特征应用于类：

[!code-csharp[Using the serializable attribute](~/samples/snippets/csharp/attributes/AttributesOverview.cs#1)]

下方示例声明了一个具有特性 <xref:System.Runtime.InteropServices.DllImportAttribute> 的方法：

[!code-csharp[Declaring a method to import from an external library](../../../../../samples/snippets/csharp/attributes/AttributesOverview.cs#2)]

如下方示例所示，可以将多个特性附加到声明中：

[!code-csharp[Including the interop namespace](~/samples/snippets/csharp/attributes/AttributesOverview.cs#3)]
[!code-csharp[Declaring two way marshaling for arguments](~/samples/snippets/csharp/attributes/AttributesOverview.cs#4)]

对于给定实体，一些特性可以指定多次。 <xref:System.Diagnostics.ConditionalAttribute> 便属于此类多用途特性：

[!code-csharp[Using the conditional attribute](~/samples/snippets/csharp/attributes/AttributesOverview.cs#5)]

> [!NOTE]
> 按照约定，所有特性名称均以“Attribute”一词结尾，以便与 .NET 库中的其他项区分开来。 不过，在代码中使用特性时，无需指定特性后缀。 例如，`[DllImport]` 等同于 `[DllImportAttribute]`，但 `DllImportAttribute` 是此特性在 .NET 类库中的实际名称。

### <a name="attribute-parameters"></a>特性参数

许多属性都有参数，可以是位置参数、未命名参数或已命名参数。 必须以特定顺序指定任何位置参数，且不能省略。 已命名参数是可选参数，可以通过任何顺序指定。 首先指定的是位置参数。 例如，下面这三个特性是等同的：

```csharp
[DllImport("user32.dll")]
[DllImport("user32.dll", SetLastError=false, ExactSpelling=false)]
[DllImport("user32.dll", ExactSpelling=false, SetLastError=false)]
```

第一个参数（DLL 名称）是位置参数，始终第一个出现；其他是已命名参数。 在此示例中，两个已命名参数的默认值均为 false，因此可以省略。 位置参数与特性构造函数的参数相对应。 已命名或可选参数与该特性的属性或字段相对应。 若要了解默认参数值，请参阅各个特性文档。

有关允许的参数类型的详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[属性](~/_csharplang/spec/attributes.md#attribute-parameter-types)部分。

### <a name="attribute-targets"></a>特性目标

特性目标是指应用特性的实体。 例如，特性可应用于类、特定方法或整个程序集。 默认情况下，特性应用于紧跟在它后面的元素。 不过，还可以进行显式标识。例如，可以标识为将特性应用于方法，还是应用于其参数或返回值。

若要显式标识特性目标，请使用以下语法：

```csharp
[target : attribute-list]
```

下表列出了可能的 `target` 值。

|目标值|适用对象|
|------------------|----------------|
|`assembly`|整个程序集|
|`module`|当前程序集模块|
|`field`|类或结构中的字段|
|`event`|事件|
|`method`|方法或 `get` 和 `set` 属性访问器|
|`param`|方法参数或 `set` 属性访问器参数|
|`property`|Property|
|`return`|方法、属性索引器或 `get` 属性访问器的返回值|
|`type`|结构、类、接口、枚举或委托|

指定 `field` 目标值，将特性应用到为[自动实现的属性](../../../properties.md)创建的支持字段。

下面的示例展示了如何将特性应用于程序集和模块。 有关详细信息，请参阅[通用特性 (C#)](../../../language-reference/attributes/global.md)。

```csharp
using System;
using System.Reflection;
[assembly: AssemblyTitleAttribute("Production assembly 4")]
[module: CLSCompliant(true)]
```

以下示例演示如何将特性应用于 C# 中的方法、方法参数和方法返回值。

[!code-csharp[Applying attributes to different code elements](../../../../../samples/snippets/csharp/attributes/AttributesOverview.cs#6)]

> [!NOTE]
> 无论在哪个目标上将 `ValidatedContract` 定义为有效，都必须指定 `return` 目标，即使 `ValidatedContract` 定义为仅应用于返回值也是如此。 换言之，编译器不会使用 `AttributeUsage` 信息来解析不明确的特性目标。 有关详细信息，请参阅 [AttributeUsage (C#)](../../../language-reference/attributes/general.md)。

## <a name="common-uses-for-attributes"></a>特性的常见用途

下面列出了代码中特性的一些常见用途：

- 在 Web 服务中使用 `WebMethod` 特性标记方法，以指明方法应可通过 SOAP 协议进行调用。 有关详细信息，请参阅 <xref:System.Web.Services.WebMethodAttribute>。
- 描述在与本机代码互操作时如何封送方法参数。 有关详细信息，请参阅 <xref:System.Runtime.InteropServices.MarshalAsAttribute>。
- 描述类、方法和接口的 COM 属性。
- 使用 <xref:System.Runtime.InteropServices.DllImportAttribute> 类调用非托管代码。
- 从标题、版本、说明或商标方面描述程序集。
- 描述要序列化并暂留类的哪些成员。
- 描述如何为了执行 XML 序列化在类成员和 XML 节点之间进行映射。
- 描述的方法的安全要求。
- 指定用于强制实施安全规范的特征。
- 通过实时 (JIT) 编译器控制优化，这样代码就一直都易于调试。
- 获取方法调用方的相关信息。

## <a name="related-sections"></a>相关章节

有关详细信息，请参见:

- [创建自定义特性 (C#)](creating-custom-attributes.md)  
- [使用反射访问特性 (C#)](accessing-attributes-by-using-reflection.md)  
- [如何使用特性创建 C/C++ 联合 (C#)](how-to-create-a-c-cpp-union-by-using-attributes.md)  
- [通用特性 (C#)](../../../language-reference/attributes/global.md)  
- [调用方信息 (C#)](../../../language-reference/attributes/caller-information.md)  

## <a name="see-also"></a>请参阅

- [C# 编程指南](../../index.md)
- [反射 (C#)](../reflection.md)
- [特性](../../../../standard/attributes/index.md)
- [在 C# 中使用特性](../../../tutorials/attributes.md)
