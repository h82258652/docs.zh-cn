---
title: 教程：使用模型生成器对卫生违规行为进行分类
description: 此教程说明如何使用 ML.NET 模型生成器生成一个多级分类模型，以对旧金山的餐馆卫生违规严重性进行分类。
author: luisquintanilla
ms.author: luquinta
ms.date: 11/21/2019
ms.topic: tutorial
ms.custom: mvc,mlnet-tooling
ms.openlocfilehash: 468d09510cb55971d25877e40b7c54aae479ac84
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875611"
---
# <a name="tutorial-classify-the-severity-of-restaurant-health-violations-with-model-builder"></a>教程：使用模型生成器对餐馆卫生违规行为的严重程度进行分类

了解如何使用模型生成器生成一个多级分类模型，以对在卫生检查期间发现的餐馆违规行为的风险级别进行分类。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 准备和了解数据
> - 选择方案
> - 从数据库加载数据
> - 定型模型
> - 评估模型
> - 使用预测模型

> [!NOTE]
> 模型生成器当前为预览版。

## <a name="prerequisites"></a>先决条件

有关先决条件和安装说明列表，请访问[模型生成器安装指南](../how-to-guides/install-model-builder.md)。

## <a name="model-builder-multiclass-classification-overview"></a>模型生成器多级分类概述

此示例会创建一个 C# .NET Core 控制台应用程序，用于通过模型生成器生成的机器学习模型对卫生违规风险进行分类。 有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub 存储库中找到。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 创建一个名为“RestaurantViolations”的 C# .NET Core 控制台应用程序。 请确保未选中“将解决方案和项目放置在同一目录中”(VS 2019) 或已选中“创建解决方案的目录”(VS 2017)     。

## <a name="prepare-and-understand-the-data"></a>准备和了解数据

> 培训和评估机器学习模型所用的数据集源自[旧金山公共卫生部餐馆安全性评分](https://www.sfdph.org/dph/EH/Food/score/default.asp)。 为了便于使用，已将此数据集精简为仅包含与模型培训和进行预测相关的列。 如需详细了解此[数据集](https://data.sfgov.org/Health-and-Social-Services/Restaurant-Scores-LIVES-Standard/pyih-qa8i?row_index=0)，请访问以下网站。

[下载餐馆安全性评分数据集](https://github.com/luisquintanilla/machinelearning-samples/raw/AB1608219/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantScores.zip)，并将其解压缩。

此数据集中的各个行包含卫生部进行检查时发现的违规行为的相关信息，以及这些违规行为对公共卫生和安全构成威胁的风险评估。

| 检查类型 | 违规行为描述 | 风险类别 |
| --- | --- | --- |
| 常规 - 不定期 | 未对食物接触面进行充分清洁或消毒 | 中等风险 |
| 新营业场所 | 高危害虫成群出现 | 高风险 |
| 常规 - 不定期 | 擦拭布不干净或存放不当或消毒液不足 | 低风险 |

- 检查类型：检查的类型。 它可以是对新场所的首次检查、常规检查、投诉检查，以及其他各种类型的检查。
- 违规行为描述：对检查期间发现的违规行为的描述。
- 风险类别：违规行为对公共健康和安全构成风险的严重性。

`label` 是要预测的列。 执行分类任务时，目标是分配一个类别（文本或数值）。 在此分类方案中，对违规行为的严重性赋值为：低风险、中等风险或高风险。 因此，“风险类别”是标签。 `features` 是你为模型提供的用来预测 `label` 的输入。 在此案例中，“检查类型”和“违规行为描述”用作预测“风险类别”的特性或输入。

## <a name="choose-a-scenario"></a>选择方案

![Visual Studio 中的“模型生成器”向导](./media/sentiment-analysis-model-builder/model-builder-screen.png)

为了训练模型，请从模型生成器提供的可用机器学习方案列表中进行选择。 在此案例中，方案为“问题分类”。

1. 在“解决方案资源管理器”中，右键单击“餐馆违规行为”项目，然后选择“添加” > “机器学习”。
1. 在此示例中，方案为多级分类。 在模型生成器的“方案”步骤中，选择“问题分类”方案。

## <a name="load-the-data"></a>加载数据

模型生成器可接受来自 SQL Server 数据库或者 `csv` 或 `tsv` 格式的本地文件中的数据。

1. 在模型生成器工具的数据步骤中，从数据源下拉列表中选择“SQL Server”。
1. 选择“连接到 SQL Server 数据库”文本框旁的按钮。
    1. 在“选择数据”对话框中，选择“Microsoft SQL Server 数据库文件”。
    1. 取消选中“始终使用此选择”复选框，然后选择“继续”。 
    1. 在“连接属性”对话框中，选择“浏览”，然后选择已下载的“RestaurantScores.mdf”文件。
    1. 选择“确定”。
1. 从“表名称”下拉列表中选择“违规行为”。
1. 在“要预测的列(标签)”下拉列表中选择“风险类别”。
1. 保留默认的列选择，即在“输入列(特性)”下拉列表中选择的“检查类型”和“违规行为描述”。
1. 选择“培训”链接，转到模型生成器中的下一步。

## <a name="train-the-model"></a>定型模型

在本教程中，用于培训问题分类模型的机器学习任务是多级分类。 在模型培训过程中，模型生成器使用不同的多级分类算法和设置来培训各个模型，以便为数据集找到性能最佳的模型。

模型培训所需的时间与数据量成正比。 模型生成器会根据数据源的大小自动选择“训练时间(秒)”的默认值  。

1. 尽管模型生成器将“训练时间(秒)”  的值设置为 10 秒，但可以将其增加到 30 秒。 通过较长时间段的训练，模型生成器可以在最佳模型的搜索中浏览更多的算法和参数组合。
1. 选择“开始训练”  。

    在训练过程中，进度数据显示在训练步骤中的 `Progress` 部分。

    - “状态”显示培训进程的完成状态。
    - “最高准确性”显示截至目前由模型生成器找到的性能最佳的模型的准确性。  准确性越高，意味着模型对测试数据的预测越准确。
    - “最佳算法”显示截至目前由模型生成器找到的性能最佳的算法的名称。 
    - “最新算法”显示模型生成器为了培训模型采用的最新算法名称。 

1. 培训完成后，选择“评估”链接以转到下一步。

## <a name="evaluate-the-model"></a>评估模型

培训步骤的成果将是一个具备最佳性能的模型。 在模型生成器的评估步骤中，输出部分将包含“最佳模型”项中性能最佳模型使用的算法，并包含“最佳模型准确度”中的指标。 此外，还会显示一个摘要表，其中最多包含五个已研究的模型及其指标。

如果你对自己的准确性指标不满意，则尝试提高模型准确性的简单方法是增加模型的训练时间或使用更多数据。 否则，选择“代码”链接，转到模型生成器中的最后一步。

## <a name="add-the-code-to-make-predictions"></a>添加代码进行预测

培训期间会创建两个项目。

- RestaurantViolationsML.ConsoleApp：一个 C# .NET Core 控制台应用程序，其中包含模型训练和示例使用代码。
- RestaurantViolationsML.Model：一个 .NET Standard 类库，包含定义输入和输出模型数据架构的数据模型、训练期间性能最佳的模型的保存版本，以及用于执行预测的帮助程序类（称为 `ConsumeModel`）。

1. 在模型生成器的“代码”步骤中，选择“添加项目”，以将自动生成的项目添加到解决方案。
1. 打开“餐馆违规行为”项目中的“Program.cs”文件。
1. 添加以下 using 语句以引用 RestaurantViolationsML.Model 项目：

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L2)]

1. 要使用模型对新数据进行预测，请在应用程序的 `Main` 方法内创建 `ModelInput` 类的新实例。 请注意，风险类别不是输入的一部分。 这是因为模型将为它生成预测。

    [!code-csharp [TestData](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L11-L15)]

1. 使用 `ConsumeModel` 类中的 `Predict` 方法。 `Predict` 方法将加载经过培训的模型，为模型创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 并使用它对新数据进行预测。

    [!code-csharp [Prediction](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L17-L24)]

1. 运行应用程序。

    该程序生成的输出应类似于下面的代码段：

    ```bash
    Inspection Type: Complaint
    Violation Description: Inadequate sewage or wastewater disposal
    Risk Category: Moderate Risk
    ```

如果稍后需要在另一个解决方案中引用生成的项目，可以在 `C:\Users\%USERNAME%\AppData\Local\Temp\MLVSTools` 目录中找到它们。

祝贺你！ 你已成功使用模型生成器生成用于对卫生违规行为风险进行分类的机器学习模型。 有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub 存储库中找到。

## <a name="additional-resources"></a>其他资源

若要详细了解本教程中所述的主题，请访问以下资源:

- [模型生成器应用场景](../automate-training-with-model-builder.md#scenario)
- [多类分类](../resources/glossary.md#multiclass-classification)
- [多级分类模型指标](../resources/metrics.md#evaluation-metrics-for-multi-class-classification)
