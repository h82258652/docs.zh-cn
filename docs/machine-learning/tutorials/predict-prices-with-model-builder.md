---
title: 教程：通过模型生成器使用回归来预测价格
description: 本教程演示如何使用 ML.NET 模型生成器生成回归模型以预测价格，特别是纽约市的出租车费。
author: luisquintanilla
ms.author: luquinta
ms.date: 11/21/2019
ms.topic: tutorial
ms.custom: mvc, mlnet-tooling
ms.openlocfilehash: c34586014b5617f7712a4b708cb2e4a4814da684
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874753"
---
# <a name="tutorial-predict-prices-using-regression-with-model-builder"></a>教程：通过模型生成器使用回归来预测价格

了解如何使用 ML.NET 模型生成器来生成用于预测价格的回归模型。  在本教程中开发的.NET 控制台应用根据纽约市出租车费的历史数据预测出租车费。

模型生成器价格预测模板可用于任何需要数值预测值的方案。 示例方案包括：房价预测、需求预测和销售额预测。

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 准备和了解数据
> - 选择方案
> - 加载数据
> - 定型模型
> - 评估模型
> - 使用预测模型

> [!NOTE]
> 模型生成器当前为预览版。

## <a name="pre-requisites"></a>先决条件

请访问[模型生成器安装指南](../how-to-guides/install-model-builder.md)，查看先决条件和安装说明的列表。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 创建名为“TaxiFarePrediction”的 C# .NET Core 控制台应用程序。 请确保未选中“将解决方案和项目放置在同一目录中”(VS 2019) 或已选中“创建解决方案的目录”(VS 2017)     。

## <a name="prepare-and-understand-the-data"></a>准备和了解数据

1. 在项目中创建一个名为“数据”的目录来保存数据集文件。

1. 用于训练和评估机器学习模型的数据集最初来自 NYC TLC 出租车行程数据集。

    1. 要下载数据集，请导航至 [taxi-fare-train.csv 下载链接](https://raw.githubusercontent.com/dotnet/machinelearning/main/test/data/taxi-fare-train.csv)。

    1. 页面加载完成后，右键单击页面上的任意位置，然后选择“另存为”。

    1. 使用“另存为”对话框将文件保存在你在上一步创建的“数据”文件夹中。

1. 在解决方案资源管理器中，右键单击“taxi-fare-train.csv”文件并选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

`taxi-fare-train.csv` 数据集中的每一行都包含一辆出租车的详细行程。

1. 打开“taxi-fare-train.csv”数据集

    提供的数据集包含以下列：

    - **vendor_id：** 出租车供应商的 ID 是一项特征。
    - **rate_code：** 出租车行程的费率类型是一项特征。
    - **passenger_count：** 行程中的乘客人数是一项特征。
    - **trip_time_in_secs：** 这次行程所花的时间。 希望在行程完成前预测行程费用。 当时并不知道行程有多长。 因此，行程时间不是一项特征，需要从模型删除此列。
    - **trip_distance：** 行程距离是一项特征。
    - **payment_type：** 付款方式（现金或信用卡）是一项特征。
    - **fare_amount：** 支付的总出租车费用是一个标签。

`label` 是要预测的列。 在执行回归任务时，目标是预测一个数字值。 在此价格预测方案中，要预测的是出租车的乘车费用。 所以“fare_amount”是标签。 标识的 `features` 是为模型提供的用来预测 `label` 的输入。 在这种情况下，剩余的列（trip_time_in_secs 除外）都用作特征或输入来预测车费金额。

## <a name="choose-a-scenario"></a>选择方案

为了训练模型，需要从模型生成器提供的可用机器学习方案列表中进行选择。 在本例中，选择的方案是 `Price Prediction`。

1. 在“解决方案资源管理器”中，右键单击“TaxiFarePrediction”项目，然后选择“添加” > “机器学习”。
1. 在模型生成器工具的方案步骤中，选择“价格预测”方案。

## <a name="load-the-data"></a>加载数据

模型生成器接受来自两个源的数据：SQL Server 数据库或者 csv 或 tsv 格式的本地文件。

1. 在模型生成器工具的数据步骤中，选择数据源下拉列表中的“文件”  。
1. 选择“选择文件”文本框旁边的按钮，并使用文件资源管理器浏览到“数据”目录中的“taxi-fare-test.csv”，然后选择该文件
1. 在“要预测的列(标签)”下拉列表中选择“fare_amount”。
1. 展开“输入列(特征)”下拉列表，取消选中 trip_time_in_secs 列，以在训练时排除，不其作为特征。  导航到模型生成器工具的训练步骤。

## <a name="train-the-model"></a>定型模型

在本教程中，用于训练价格预测模型的机器学习任务是回归。 在模型训练过程中，模型生成器使用不同的回归算法和设置训练各个模型，以便为数据集找到性能最佳的模型。

模型训练所需的时间与数据量成正比。 模型生成器会根据数据源的大小自动选择“训练时间(秒)”的默认值  。

1. 如果不希望延长训练时间，则保持“训练时间(秒)”的默认值不变。
2. 选择“开始训练”  。

在训练过程中，进度数据显示在训练步骤中的 `Progress` 部分。

- “状态”显示训练进程的完成状态。
- “最高准确性”显示截至目前由模型生成器找到的性能最佳的模型的准确性。 准确性越高，意味着模型对测试数据的预测越准确。
- “最佳算法”显示截至目前由模型生成器找到的性能最佳的算法的名称。
- “最新算法”显示模型生成器为了训练模型采用的最新算法名称。

训练完成后，导航到评估步骤。

## <a name="evaluate-the-model"></a>评估模型

训练步骤的成果将是一个模型，该模型具备最佳的性能。 在模型生成器工具的评估步骤中，输出部分将包含“最佳模型”项中性能最佳的模型使用的算法，并包含“最佳模型质量 (RSquared)”中的指标。 此外还有一个摘要表格，包含性能最佳的前五种模型以及它们的指标信息。

如果对自己的准确性指标不满意，尝试提高模型准确性的简单方法是增加模型的训练时间或使用更多数据。 否则，导航到代码步骤。

## <a name="add-the-code-to-make-predictions"></a>添加代码进行预测

训练期间会创建两个项目。

- TaxiFarePredictionML.ConsoleApp：一个 .NET Core 控制台应用程序，其中包含模型训练和示例使用代码。
- TaxiFarePredictionML.Mode：一个 .NET Standard 类库，其中包含定义输入和输出模型数据架构的数据模型、训练期间性能最佳的模型的保存版本，以及用于执行预测的帮助程序类（称为 `ConsumeModel`）。

1. 在模型生成器工具的代码步骤中，选择“添加项目”，将自动生成的项目添加到解决方案。
1. 打开 TaxiFarePrediction 项目中的 Program.cs 文件。
1. 添加以下 using 语句以引用 TaxiFarePredictionML.Model 项目：

    ```csharp
    using System;
    using TaxiFarePredictionML.Model;
    ```

1. 要使用模型对新数据进行预测，请在应用程序的 `Main` 方法内创建 `ModelInput` 类的新实例。 请注意，费用金额不是输入的一部分。 这是因为模型将为它生成预测。

    ```csharp
    // Create sample data
    ModelInput input = new ModelInput()
    {
        Vendor_id = "CMT",
        Rate_code = 1,
        Passenger_count = 1,
        Trip_distance = 3.8f,
        Payment_type = "CRD"
    };
    ```

1. 使用 `ConsumeModel` 类中的 `Predict` 方法。 `Predict` 方法将加载经过训练的模型，为模型创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 并使用它对新数据进行预测。

    ```csharp
    // Make prediction
    ModelOutput prediction = ConsumeModel.Predict(input);

    // Print Prediction
    Console.WriteLine($"Predicted Fare: {prediction.Score}");
    Console.ReadKey();
    ```

1. 运行应用程序。

    该程序生成的输出应类似于下面的代码段：

    ```bash
    Predicted Fare: 14.96086
    ```

如果稍后需要在另一个解决方案中引用生成的项目，可以在 `C:\Users\%USERNAME%\AppData\Local\Temp\MLVSTools` 目录中找到它们。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
>
> - 准备和了解数据
> - 选择方案
> - 加载数据
> - 定型模型
> - 评估模型
> - 使用预测模型

### <a name="additional-resources"></a>其他资源

若要详细了解本教程中所述的主题，请访问以下资源:

- [模型生成器应用场景](../automate-training-with-model-builder.md#scenario)
- [回归](../resources/glossary.md#regression)
- [回归模型指标](../resources/metrics.md#evaluation-metrics-for-regression-and-recommendation)
- [NYC TLC 出租车行程数据集](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
