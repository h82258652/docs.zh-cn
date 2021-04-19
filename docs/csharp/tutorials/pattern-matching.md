---
title: 教程：使用模式匹配构建算法
description: 本高级教程展示了如何使用模式匹配技术，通过单独创建的数据和算法来创建功能。
ms.date: 10/06/2020
ms.technology: csharp-whats-new
ms.custom: contperf-fy21q1
ms.openlocfilehash: 5d132784f591405b906068e2ed0e04347faa5e2c
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255462"
---
# <a name="tutorial-use-pattern-matching-to-build-type-driven-and-data-driven-algorithms"></a>教程：使用模式匹配来生成类型驱动和数据驱动的算法

C# 7 引入了基本模式匹配功能。 C# 8 和 C# 9 通过新增表达式和模式，扩展了这些功能。 可以编写行为类似于扩展其他库中可能有的类型的功能。 模式的另一个用途是，创建应用程序需要的功能，但此功能不是要扩展的类型的基本功能。

本教程介绍以下操作：

> [!div class="checklist"]
>
> - 识别应使用模式匹配的情况。
> - 使用模式匹配表达式来实现基于类型和属性值的行为。
> - 结合使用模式匹配和其他方法来创建完整算法。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET 5，其中包括 C# 9 编译器。 自 [Visual Studio 2019 版本 16.9 预览 1](https://visualstudio.microsoft.com/vs/preview/) 或 [.NET 5.0 SDK](https://dot.net/get-dotnet5) 起，开始随附 C# 9 编译器。

本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。

## <a name="scenarios-for-pattern-matching"></a>模式匹配方案

新式开发通常包括从多个源集成数据，并在一个整体应用程序中呈现以相应数据为依据的信息和见解。 你和你的团队无法控制或访问表示传入数据的所有类型。

面向对象的经典设计要求，在应用程序中创建表示多个数据源中的所有数据类型的类型。 然后，应用程序便能处理这些新类型、生成继承层次结构、创建虚拟方法并实现抽象。 这些技术起作用，而且有时候是最佳工具。 不过，在其他时候，你可以编写更少的代码。 可使用将数据与管理相应数据的操作分离开来的技术，编写更明确的代码。

在本教程中，你将创建并探索从一个方案的多个外部源中提取传入数据的应用程序。 你将看到，模式匹配如何通过原始系统中没有的方式高效地使用和处理相应数据。

假设某大都市区通过通行费和高峰时段定价来管理交通。 你编写的应用程序根据车辆类型来计算车辆通行费。 后续增强功能包括，定价因车内乘客数而异。 进一步增强功能包括，定价因时间和周几而异。

通过上述简要说明，你可能已快速勾勒出用于对此系统进行建模的对象层次结构。 不过，数据来自多个源，如其他车辆注册管理系统。 这些系统提供不同的类来对相应数据进行建模，而你连可使用的一个对象模型都没有。 在本教程中，你将使用这些简化后的类，对这些外部系统中的车辆数据进行建模，如下面的代码所示：

[!code-csharp[ExternalSystems](~/samples/snippets/csharp/tutorials/patterns/start/toll-calculator/ExternalSystems.cs)]

若要下载起始代码，可以访问 [dotnet/samples](https://github.com/dotnet/samples/tree/main/csharp/tutorials/patterns/start) GitHub 存储库。 可以看到，车辆类来自不同的系统，且位于不同的命名空间中。 没有常见基类，可利用的 `System.Object` 除外。

## <a name="pattern-matching-designs"></a>模式匹配设计

本教程中使用的方案重点介绍了非常适合适用模式匹配解决的问题类型：

- 需要使用的对象不在匹配目标的对象层次结构中。 可能要使用属于不相关系统的类。
- 要添加的功能不属于这些类的核心抽象。 车辆通行费因不同车辆类型而异，但通行费不是车辆的核心功能。

如果不一起描述数据形状和对相应数据执行的操作，C# 中的模式匹配功能可以简化这一切。

## <a name="implement-the-basic-toll-calculations"></a>实现基本通行费计算

最基本的通行费计算仅依赖车辆类型：

- `Car` 的通行费为 2.00 美元。
- `Taxi` 的通行费为 3.50 美元。
- `Bus` 的通行费为 5.00 美元。
- `DeliveryTruck` 的通行费为 10.00 美元

新建 `TollCalculator` 类，并对车辆类型实现模式匹配，以获取通行费金额。 以下代码显示了 `TollCalculator` 的初始实现。

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    public class TollCalculator
    {
        public decimal CalculateToll(object vehicle) =>
            vehicle switch
        {
            Car c           => 2.00m,
            Taxi t          => 3.50m,
            Bus b           => 5.00m,
            DeliveryTruck t => 10.00m,
            { }             => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
            null            => throw new ArgumentNullException(nameof(vehicle))
        };
    }
}
```

上面的代码使用测试[声明类型](../language-reference/operators/patterns.md#declaration-and-type-patterns)的 [`switch` 表达式](../language-reference/operators/switch-expression.md)（与 [`switch`](../language-reference/keywords/switch.md) 语句不同）。 switch 表达式以变量（上面代码中的 `vehicle`）开头，后跟 `switch` 关键字。 接下来是大括号内的所有 switch 臂。 `switch` 表达式对 `switch` 语句周围的语法进行了其他优化。 不仅省略了 `case` 关键字，还让每个臂的结果成为表达式。 最后两个臂展示了一种新语言功能。 `{ }` 子句匹配与之前的臂不匹配的任何非 null 对象。 此臂捕获传递到这个方法的所有不正确类型。 `{ }` 事例必须遵循每种车辆类型的情况。 如果订单被撤销，则 `{ }` 事例优先。 最后，`null` [常量模式](../language-reference/operators/patterns.md#constant-pattern)检测何时将 `null` 传递给此方法。 `null` 模式可以是最后一个，因为其他模式仅匹配正确类型的非 NULL 对象。

可使用 `Program.cs` 中的以下代码来测试上面的代码：

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    class Program
    {
        static void Main(string[] args)
        {
            var tollCalc = new TollCalculator();

            var car = new Car();
            var taxi = new Taxi();
            var bus = new Bus();
            var truck = new DeliveryTruck();

            Console.WriteLine($"The toll for a car is {tollCalc.CalculateToll(car)}");
            Console.WriteLine($"The toll for a taxi is {tollCalc.CalculateToll(taxi)}");
            Console.WriteLine($"The toll for a bus is {tollCalc.CalculateToll(bus)}");
            Console.WriteLine($"The toll for a truck is {tollCalc.CalculateToll(truck)}");

            try
            {
                tollCalc.CalculateToll("this will fail");
            }
            catch (ArgumentException e)
            {
                Console.WriteLine("Caught an argument exception when using the wrong type");
            }
            try
            {
                tollCalc.CalculateToll(null!);
            }
            catch (ArgumentNullException e)
            {
                Console.WriteLine("Caught an argument exception when using null");
            }
        }
    }
}
```

此代码虽然包含在初学者项目中，但已被注释掉。删除注释即可测试已编写的代码。

你正开始了解，模式如何有助于创建将代码和数据分离开来的算法。 `switch` 表达式测试类型，并根据结果生成不同的值。 这仅仅是开始。

## <a name="add-occupancy-pricing"></a>添加因乘客数而异的定价

通行费收取机构希望鼓励车辆以最大载客量出行。 他们已决定，对乘客数较少的车辆收取更多费用，并通过更低定价来鼓励车辆乘客满员：

- 没有乘客的汽车和出租车需额外支付 0.50 美元。
- 载有两名乘客的汽车和出租车可享受 0.50 美元折扣。
- 载有三名或更多乘客的汽车和出租车可享受 1.00 美元折扣。
- 乘客数不到满载量 50% 的巴士需额外支付 2.00 美元。
- 乘客数超过满载量 90% 的巴士可享受 1.00 美元折扣。

可使用[属性模式](../language-reference/operators/patterns.md#property-pattern)在同一 switch 表达式中实现这些规则。 属性模式将属性值与常数值进行比较。 属性模式在类型已确定后检查对象的属性。 `Car` 的一个子句扩展为四个不同的子句：

```csharp
vehicle switch
{
    Car {Passengers: 0}        => 2.00m + 0.50m,
    Car {Passengers: 1}        => 2.0m,
    Car {Passengers: 2}        => 2.0m - 0.50m,
    Car c                      => 2.00m - 1.0m,

    // ...
};
```

前三个子句测试类型 `Car`，然后检查 `Passengers` 属性的值。 如果两个条件都匹配，系统便会计算并返回相应表达式。

还可以类似方式扩展出租车的子句：

```csharp
vehicle switch
{
    // ...

    Taxi {Fares: 0}  => 3.50m + 1.00m,
    Taxi {Fares: 1}  => 3.50m,
    Taxi {Fares: 2}  => 3.50m - 0.50m,
    Taxi t           => 3.50m - 1.00m,

    // ...
};
```

接下来，通过扩展巴士的子句来实现载客量规则，如下面的示例所示：

```csharp
vehicle switch
{
    // ...

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
    Bus b => 5.00m,

    // ...
};
```

通行费收取机构并不关注运货卡车中的乘客数。 相反，它们根据卡车的重量级别调整通行费金额，如下所示：

- 超过 5000 磅的运货卡车需额外支付 5.00 美元。
- 3000 磅以下的轻型卡车可享受 2.00 美元折扣。

此规则通过以下代码实现：

```csharp
vehicle switch
{
    // ...

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

以上代码展示了 switch 臂的 `when` 子句。 `when` 子句用于对属性测试条件（相等性除外）。 完成后的方法如以下代码所示：

```csharp
vehicle switch
{
    Car {Passengers: 0}        => 2.00m + 0.50m,
    Car {Passengers: 1}        => 2.0m,
    Car {Passengers: 2}        => 2.0m - 0.50m,
    Car c                      => 2.00m - 1.0m,

    Taxi {Fares: 0}  => 3.50m + 1.00m,
    Taxi {Fares: 1}  => 3.50m,
    Taxi {Fares: 2}  => 3.50m - 0.50m,
    Taxi t           => 3.50m - 1.00m,

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
    Bus b => 5.00m,

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,

    { }     => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
    null    => throw new ArgumentNullException(nameof(vehicle))
};
```

其中许多 switch 臂都是递归模式示例。 例如，`Car { Passengers: 1}` 表明属性模式内有常量模式。

可使用嵌套的 switch 来减少此代码中重复的地方。 在上面的示例中，`Car` 和 `Taxi` 都有四个不同的臂。 在这两种案例中，都可创建向常量模式馈送数据的声明模式。 下面的代码展示了这项技术：

```csharp
public decimal CalculateToll(object vehicle) =>
    vehicle switch
    {
        Car c => c.Passengers switch
        {
            0 => 2.00m + 0.5m,
            1 => 2.0m,
            2 => 2.0m - 0.5m,
            _ => 2.00m - 1.0m
        },

        Taxi t => t.Fares switch
        {
            0 => 3.50m + 1.00m,
            1 => 3.50m,
            2 => 3.50m - 0.50m,
            _ => 3.50m - 1.00m
        },

        Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
        Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
        Bus b => 5.00m,

        DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
        DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
        DeliveryTruck t => 10.00m,

        { }  => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
        null => throw new ArgumentNullException(nameof(vehicle))
    };
```

在上面的示例中，使用递归表达式意味着不用重复包含测试属性值的子臂的 `Car` 和 `Taxi` 臂。 此技术不适用于 `Bus` 和 `DeliveryTruck` 臂，因为这些臂测试的是属性范围，而不是离散值。

## <a name="add-peak-pricing"></a>添加高峰时段定价

对于最后一个功能，通行费收取机构希望添加有时效性的高峰时段定价。 在早晚高峰时段，通行费翻倍。 此规则只影响一个方向的交通：早高峰时段入城和晚高峰时段出城。 在工作日的其他时段，通行费增加 50%。 在深夜和清晨，通行费减少 25%。 在周末，无论什么时间，都按正常费率收费。 如果 `if` 和 `else` 语句使用以下代码表达此内容，则可以使用系列：

[!code-csharp[FullTuplePattern](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#SnippetPremiumWithoutPattern)]

前面的代码可以正常工作，但无法读取。 必须链接所有输入事例和嵌套的 `if` 语句，才能对代码进行推理。 相反，虽然将对此功能使用模式匹配，但要将它与其他技术集成。 可以生成一个模式匹配表达式，将方向、周几和时间所有这一切都考虑在内。 生成的结果是一个复杂的表达式。 它既难读取，也难理解。 这就加大了确保正确性的难度。 请改为将这些方法合并为生成值元组，用于简要描述所有这些状态。 然后，使用模式匹配来计算通行费乘数。 元组包含以下三个离散条件：

- 是工作日，还是周末。
- 收取通行费时所处的时间带区。
- 方向是入城，还是出城

下表展示了输入值和高峰时段定价乘数的组合：

| 日期        | 时间         | 方向 | 高级 |
| ---------- | ------------ | --------- |--------:|
| 星期    | 早高峰 | 入城   | x 2.00  |
| 星期    | 早高峰 | 出城  | x 1.00  |
| 星期    | 日间      | 入城   | x 1.50  |
| 星期    | 日间      | 出城  | x 1.50  |
| 星期    | 晚高峰 | 入城   | x 1.00  |
| 星期    | 晚高峰 | 出城  | x 2.00  |
| 星期    | 夜间    | 入城   | x 0.75  |
| 星期    | 夜间    | 出城  | x 0.75  |
| 周末    | 早高峰 | 入城   | x 1.00  |
| 周末    | 早高峰 | 出城  | x 1.00  |
| 周末    | 日间      | 入城   | x 1.00  |
| 周末    | 日间      | 出城  | x 1.00  |
| 周末    | 晚高峰 | 入城   | x 1.00  |
| 周末    | 晚高峰 | 出城  | x 1.00  |
| 周末    | 夜间    | 入城   | x 1.00  |
| 周末    | 夜间    | 出城  | x 1.00  |

三个变量有 16 种不同的组合。 通过结合某些条件，将能简化最终的 switch 表达式。

通行费收取系统在收取通行费时对时间使用 <xref:System.DateTime> 结构。 生成根据上表创建变量的成员方法。 以下函数用作模式匹配 switch 表达式，以表示 <xref:System.DateTime> 是表示周末，还是表示工作日：

```csharp
private static bool IsWeekDay(DateTime timeOfToll) =>
    timeOfToll.DayOfWeek switch
    {
        DayOfWeek.Monday    => true,
        DayOfWeek.Tuesday   => true,
        DayOfWeek.Wednesday => true,
        DayOfWeek.Thursday  => true,
        DayOfWeek.Friday    => true,
        DayOfWeek.Saturday  => false,
        DayOfWeek.Sunday    => false
    };
```

此方法虽然正确，但是具有重复性。 可以简化它，如下面的代码所示：

[!code-csharp[IsWeekDay](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#IsWeekDay)]

接下来，添加将时间分类为块的类似函数：

[!code-csharp[GetTimeBand](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#GetTimeBand)]

添加将每个时间范围转换为离散值的专用 `enum`。 然后，`GetTimeBand` 方法使用[关系模式](../language-reference/operators/patterns.md#relational-patterns)和[合取 `or` 模式](../language-reference/operators/patterns.md#logical-patterns)，两者都已添加到 C# 9.0 中。 通过关系模式，可使用 `<`、`>`、`<=` 或 `>=` 来测试数值。 `or` 模式测试表达式是否与一个或多个模式匹配。 还可以使用 `and` 模式来确保表达式匹配两个不同的模式，并使用 `not` 模式来测试表达式是否与模式不匹配。

创建这些方法后，可以结合使用另一个 `switch` 表达式和元组模式，以计算定价附加费。 可以生成包含所有 16 个臂的 `switch` 表达式：

[!code-csharp[FullTuplePattern](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#TuplePatternOne)]

上面的代码虽起作用，但可以进行简化。 周末对应的所有八个组合的通行费都相同。 可以将所有八个组合都替换为下面的代码：

```csharp
(false, _, _) => 1.0m,
```

入城和出城交通乘数在工作日日间和夜间时段都相同。 可以将四个 switch 臂替换为以下两行代码：

```csharp
(true, TimeBand.Overnight, _) => 0.75m,
(true, TimeBand.Daytime, _)   => 1.5m,
```

执行这两项更改后，代码应如下所示：

```csharp
public decimal PeakTimePremium(DateTime timeOfToll, bool inbound) =>
    (IsWeekDay(timeOfToll), GetTimeBand(timeOfToll), inbound) switch
    {
        (true, TimeBand.MorningRush, true)  => 2.00m,
        (true, TimeBand.MorningRush, false) => 1.00m,
        (true, TimeBand.Daytime,     _)     => 1.50m,
        (true, TimeBand.EveningRush, true)  => 1.00m,
        (true, TimeBand.EveningRush, false) => 2.00m,
        (true, TimeBand.Overnight,   _)     => 0.75m,
        (false, _,                   _)     => 1.00m,
    };
```

最后，可以删除通行费正常的两个高峰时段。 删除这些臂后，可以在最后的 switch 臂中将 `false` 替换为弃元 (`_`)。 完成的方法如下所示：

[!code-csharp[SimplifiedTuplePattern](../../../samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#FinalTuplePattern)]

此示例突出了模式匹配的一个优点：模式分支是依序计算的。 如果将它们重排为更早的分支处理后续事例之一，编译器便会提示你无法访问的代码。 借助这些语言规则，可以更轻松地执行前面的简化，同时确信代码未更改。

模式匹配使某些类型的代码更具可读性，并且当你无法向类添加代码时，它会提供面向对象技术的替代方法。 云会导致数据和功能分离。 数据形状和对相应数据执行的操作不一定在一起进行描述。 在本教程中，你通过与原始功能完全不同的方法使用了现有数据。 使用模式匹配，可以覆盖这些类型编写功能，即使无法扩展类型，也不例外。

## <a name="next-steps"></a>后续步骤

若要下载完成后的代码，可以访问 [dotnet/samples](https://github.com/dotnet/samples/tree/main/csharp/tutorials/patterns/finished) GitHub 存储库。 请自行探索模式，并将此技术纳入你的常规编码活动。 学些这些技术，你可以通过其他方式来处理问题和新建功能。

## <a name="see-also"></a>另请参阅

- [模式](../language-reference/operators/patterns.md)
- [`switch`表达式](../language-reference/operators/switch-expression.md)
