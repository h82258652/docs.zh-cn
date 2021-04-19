---
title: 使用记录类型 - C# 教程
description: 了解如何使用记录类型、构建记录的层次结构，以及何时选择记录而不是类。
ms.date: 11/12/2020
ms.openlocfilehash: c8a1922fdf113d046dc087ec289accce4312c4ae
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075532"
---
# <a name="create-record-types"></a>创建记录类型

C# 9 引入了 [*记录*](../../language-reference/builtin-types/record.md)，这是一种可以创建的新引用类型，而不是类或结构。 记录与类不同，区别在于记录类型使用基于值的相等性。 两个记录类型的变量在它们的类型和值都相同时，它们是相等的。 两个类类型的变量如果引用的对象属于同一类类型并且引用相同的对象，则这两个变量是相等的。 基于值的相等性意味着可能需要的记录类型中的其他功能。 声明 `record` 而不是 `class` 时，编译器将生成许多这些成员。

本教程介绍以下操作：

> [!div class="checklist"]
>
> - 决定是否应声明 `class` 或 `record`。
> - 声明记录类型和位置记录类型。
> - 在记录中将你的方法替换为编译器生成的方法。

## <a name="prerequisites"></a>必备条件

需要将计算机设置为运行 .NET 5 或更高版本，包括 C# 9.0 或更高版本编译器。 自 [Visual Studio 2019 版本 16.8](https://visualstudio.microsoft.com/vs) 或 [.NET 5.0 SDK](https://dotnet.microsoft.com/download) 起，开始随附 C# 9.0 编译器。

## <a name="characteristics-of-records"></a>记录的特征

通过使用 `record` 关键字（而不是 `class` 或 `struct` 关键字）声明一个类型，可以定义记录。 记录是一种引用类型并遵循基于值的相等性语义。 为了强制执行值语义，编译器将为记录类型生成多种方法：

- <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> 的替代。
- 一个虚拟的 `Equals` 方法，其参数为记录类型。
- <xref:System.Object.GetHashCode?displayProperty=nameWithType> 的替代。
- 用于 `operator ==` 和 `operator !=` 的方法。
- 记录类型实现 <xref:System.IEquatable%601?displayProperty=nameWithType>。

此外，记录还提供 <xref:System.Object.ToString?displayProperty=nameWithType> 的替代。 编译器使用 <xref:System.Object.ToString?displayProperty=nameWithType> 合成用于显示记录的方法。 在编写本教程的代码时，你将浏览这些成员。 记录支持 `with` 表达式，以启用记录的非破坏性修改。

还可使用更简洁的语法来声明位置记录。 声明以下位置记录时，编译器会合成更多方法：

- 主构造函数，它的参数与记录声明上的位置参数匹配。
- 主构造函数的每个参数的公共 init-only 属性。
- 用于从记录中提取属性的 `Deconstruct` 方法。

## <a name="build-temperature-data"></a>生成温度数据

数据和统计信息是你要使用记录时所需的内容。 在本教程中，你将构建一个用于计算度日数的应用程序，以用于不同用途。 度日数是反映几天、几周或几个月内采暖（或采暖不足）的度量。 度日数可跟踪和预测能源使用情况。 高温天数越多表示使用空调的时间越多，降温天数越多意味着使用暖气炉的时间越多。 度日数有助于管理植物种群，并且随着季节的变化，与植物的生长密切相关。 度日数有助于跟踪动物为适应气候而进行的物种迁徙。

此公式基于给定的某一天的平均温度和基准温度。 若要计算一段时间内的度日数，需要这段时间的每日最高温度和最低温度。 首先，我们要创建一个新的应用程序。 生成新的控制台应用程序。 在名为“DailyTemperature.cs”的新文件中创建新的记录类型：

:::code language="csharp" source="snippets/record-types/InterimSteps.cs" ID="DailyRecord":::

上述代码定义了位置记录。 你已创建包含两个属性（`HighTemp` 和 `LowTemp`）的引用类型。 这些属性是 init-only 属性，这意味着可在构造函数中设置它们，或使用属性初始化表达式设置它们。 `DailyTemperature` 类型还有一个主构造函数，该构造函数具有两个与这两个属性匹配的参数。 使用该主构造函数初始化 `DailyTemperature` 记录：

:::code language="csharp" source="snippets/record-types/Program.cs" ID="DeclareData":::

可将你自己的属性或方法添加到记录，包括位置记录。 需要计算每天的平均温度。 可将该属性添加到 `DailyTemperature` 记录：

:::code language="csharp" source="snippets/record-types/DailyTemperature.cs" ID="TemperatureRecord":::

现在需确保你可以使用此数据。 将以下代码添加到 `Main` 方法：

```csharp
foreach (var item in data)
    Console.WriteLine(item);
```

运行应用程序，然后你将看到类似于以下显示内容的输出（因空间有限，删除了几行内容）：

```dotnetcli
DailyTemperature { HighTemp = 57, LowTemp = 30, Mean = 43.5 }
DailyTemperature { HighTemp = 60, LowTemp = 35, Mean = 47.5 }


DailyTemperature { HighTemp = 80, LowTemp = 60, Mean = 70 }
DailyTemperature { HighTemp = 85, LowTemp = 66, Mean = 75.5 }
```

上述代码显示了由编译器合成的 `ToString` 的替代输出。 如果希望使用不同的文本，可编写自己的 `ToString` 版本，以防止编译器为你合成一个版本。

## <a name="compute-degree-days"></a>计算度日数

若要计算度日数，需要获得给定的某一天的基准温度和平均温度之间的差额。 若要测量一段时间内的采暖，需要忽略平均温度低于基准温度的任何日期。 若要测量一段时间内的降温，需要忽略平均温度高于基准温度的任何日期。 例如，美国使用 65F 作为采暖和制冷度日数的基准。 在此温度下，无需采暖或制冷。 如果某一天的平均温度为 70F，则这一天的制冷度日数为 5，采暖度日数为 0。 相反，如果某一天的平均温度为 55F，则这一天的采暖度日数为 10，制冷度日数为 0。

可将这些公式表示为记录类型的小型层次结构：一种抽象度日数类型以及两种具体的采暖度日数和制冷度日数类型。 这些类型也可以是位置记录。 它们将基准温度和一系列每日温度记录作为主构造函数的参数：

:::code language="csharp" source="snippets/record-types/InterimSteps.cs" ID="DegreeDaysRecords":::

抽象的 `DegreeDays` 记录是 `HeatingDegreeDays` 和 `CoolingDegreeDays` 记录的共享基类。 派生记录的主构造函数声明显示了如何管理基本记录初始化。 派生记录为基本记录主构造函数中的所有参数声明参数。 基本记录声明并初始化这些属性。 派生记录不会隐藏它们，而只会创建和初始化未在其基本记录中声明的参数的属性。 在此示例中，派生记录不会添加新的主构造函数参数。 通过将以下代码添加到 `Main` 方法来测试代码：

:::code language="csharp" source="snippets/record-types/Program.cs" ID="HeatingAndCooling":::

将获得类似以下显示内容的输出：

```dotnetcli
HeatingDegreeDays { BaseTemperature = 65, TempRecords = record_types.DailyTemperature[], DegreeDays = 85 }
CoolingDegreeDays { BaseTemperature = 65, TempRecords = record_types.DailyTemperature[], DegreeDays = 71.5 }
```

## <a name="define-compiler-synthesized-methods"></a>定义编译器合成方法

代码将计算该时间段内正确的采暖和制冷度日数。 但此示例展示了为何需要替换记录的某些合成方法。 可以在记录类型中声明你自己的版本的任意编译器合成方法（clone 方法除外）。 clone 方法具有编译器生成的名称，你无法提供不同的实现。 这些合成方法包括复制构造函数、<xref:System.IEquatable%601?displayProperty=nameWithType> 接口的成员、相等性和不相等测试以及 <xref:System.Object.GetHashCode>。 为此，你需要合成 `PrintMembers`。 你还可以声明自己的 `ToString`，但 `PrintMembers` 为继承方案提供了更好的选择。 若要提供自己的合成方法版本，签名必须与合成方法相匹配。

控制台输出中的 `TempRecords` 元素不起作用。 它只显示类型。 可通过提供自己的合成 `PrintMembers` 方法的实现来更改此行为。 签名取决于应用于 `record` 声明的修饰符：

- 如果记录类型为 `sealed`，则签名为 `private bool PrintMembers(StringBuilder builder);`
- 如果记录类型不为 `sealed` 并派生自 `object`（即，它不声明基本记录），则签名为 `protected virtual bool PrintMembers(StringBuilder builder);`
- 如果记录类型不为 `sealed` 并派生自其他记录，则签名为 `protected override bool PrintMembers(StringBuilder builder);`

了解 `PrintMembers` 的目的之后，就可以轻松地理解这些规则。 `PrintMembers` 将记录类型中每个属性的相关信息添加到字符串。 该协定要求基本记录添加其要显示的成员，并假设派生成员将添加其成员。 每个记录类型都会合成一个 `ToString` 替代，与下面的 `HeatingDegreeDays` 示例类似：

```csharp
public override string ToString()
{
    StringBuilder stringBuilder = new StringBuilder();
    stringBuilder.Append("HeatingDegreeDays");
    stringBuilder.Append(" { ");
    if (PrintMembers(stringBuilder))
    {
        stringBuilder.Append(" ");
    }
    stringBuilder.Append("}");
    return stringBuilder.ToString();
}
```

在不打印集合类型的 `DegreeDays` 记录中声明 `PrintMembers` 方法：

:::code language="csharp" source="snippets/record-types/DegreeDays.cs" ID="AddPrintMembers":::

签名声明一个 `virtual protected` 方法来匹配编译器的版本。 如果访问器出错，请不要担心；语言会强制执行正确的签名。 如果你忘记了任何合成方法的正确修饰符，则编译器会发出警告或错误，帮助你获取正确的签名。

## <a name="non-destructive-mutation"></a>非破坏性修改

位置记录中的合成成员不会修改记录的状态。 目的是帮助你更轻松地创建不可变记录。 请再次查看前面的关于 `HeatingDegreeDays` 和 `CoolingDegreeDays` 的声明。 添加的成员对记录的值执行计算，但不会改变状态。 位置记录使你可以更轻松地创建不可变引用类型。

创建不可变的引用类型意味着需要使用非破坏性修改。 使用 [`with` 表达式](../../language-reference/operators/with-expression.md) 创建与现有记录实例类似的新记录实例。 这些表达式是一个副本构造，其中包含修改副本的其他赋值。 结果是一个新的记录实例，其中每个属性都已从现有记录进行复制并选择性地进行了修改。 原始记录未发生更改。

让我们向程序添加一些演示 `with` 表达式的功能。 首先，创建一条新记录，使用相同数据计算增长的度日数。 增长的度日数通常使用 41F 作为基准，并测量超出基准的温度。 若要使用相同的数据，可创建一条类似于 `coolingDegreeDays` 的新记录，但基准温度不同：

:::code language="csharp" source="snippets/record-types/Program.cs" ID="GrowingDegreeDays":::

可将计算得出的度数与在较高基准温度下生成的数字进行比较。 请记住，记录是引用类型，这些副本是浅表副本。 不会复制数据的数组，但两条记录都引用相同的数据。 在另一种场景中，这是一个优势。 对于增长的度日数，记录前 5 天的总度数非常有用。 可以使用 `with` 表达式创建具有不同源数据的新记录。 下面的代码将生成这些累计数据的集合，然后显示这些值：

:::code language="csharp" source="snippets/record-types/Program.cs" ID="RunningFiveDayTotal":::

还可使用 `with` 表达式来创建记录的副本。 请勿指定 `with` 表达式的大括号之间的任何属性。 这意味着将创建一个副本，并且不会更改任何属性：

```csharp
var growingDegreeDaysCopy = growingDegreeDays with { };
```

运行已完成的应用程序以查看结果。

## <a name="summary"></a>总结

本教程介绍了记录的几个方面。 记录为引用类型提供了简洁的语法，它的基本用途是存储数据。 对于面向对象的类，基本用途是定义责任。 本教程重点介绍了位置记录，在这种记录中，你可以使用简洁的语法来声明记录的 init-only 属性。 编译器会合成记录的多个成员，以复制和比较记录。 你可针对记录类型添加所需的任何其他成员。 在明确编译器生成的所有成员都不会改变状态的情况下，可以创建不可变的记录类型。 可借助 `with` 表达式轻松实现非破坏性修改。

记录提供了另一种定义类型的方法。 使用 `class` 定义来创建面向对象的层次结构，这些层次结构重点关注对象的责任和行为。 可为数据结构创建 `struct` 类型，这些数据结构可存储数据，并且足够小，以便进行有效复制。 当你需要基于值的相等性和比较、不需要复制值以及要使用引用变量时，可以创建 `record` 类型。

若要了解记录的完整说明，可阅读[记录类型的 C# 语言参考文章](../../language-reference/builtin-types/record.md)和[建议的记录类型规范](~/_csharplang/proposals/csharp-9.0/records.md)。
